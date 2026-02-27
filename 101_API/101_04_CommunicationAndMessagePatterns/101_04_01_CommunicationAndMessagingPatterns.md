
# 1 MESSAGES

![](image/Pasted%20image%2020260218004948.png)

- In all communication patterns between services – request-response, one-way notifications, or asynchronous events – the basic unit of information is a message
- A message is a self-contained piece of data that is sent from one participant to another
- It describes what is being communicated, independently of how it is transported (HTTP, message queues, Kafka,...)
- A message has two parts, a **header**, which contains control data that the recipient needs to process the message correctly, and a **body**, carrying the payload
- Messages are typically immutable: once sent, their content does not change, which simplifies logging, retries, auditing, and event sourcing


- Communication patterns differ in how messages flow:
- In synchronous request-response, the consumer sends a request message and waits for a response message
- In one-way messaging, the sender submits a message and does not expect a (direct) reply
- In publish-subscribe, a producer sends a message to a topic and multiple consumers may receive copies of it

## 1.1 Message Types


![](image/Pasted%20image%2020260218005249.png)

## 1.2 MESSAGE FORMATS

1 Text-based Message
![](image/Pasted%20image%2020260218010247.png)
Text-based formats encode data as human-readable text, typically using UTF-8
Example format: JSON, XML or CSV
They have a self-describing structure as they carry field names and structural markers (braces, tags) directly in the text
Schemas (e.g., JSON Schema, XSD) are optional but often used for validation
Compared to compact binary encoding, text-based messages are typically larger
Parsing and serialization require more CPU, especially at high message rates
Widely supported by programming languages, frameworks, and tools
Common default for HTTP/REST and public Web APIs 


---
2  Binary formats encode data 
Binary formats encode data as compact binary structures that are optimized for machines, not humans
Examples include Protocol Buffers (Protobuf), Avro, Thrift, Message Pack
Messages appear as sequences of bytes (often shown in hexdumps)
Most binary formats rely on an Interface Definition Language (IDL) or schema definition
Messages are typically significantly smaller than equivalent JSON or XML
Parsing and serialization are faster, which reduces CPU usage and latency for high-throughput scenarios
Well-suited for internal service-to-service communication

![](image/Pasted%20image%2020260218010345.png)


# 2 SYNCHRONOUS COMMUNICATION


## 2.1 Basic Request-response

Synchronous communication means that the consumer sends a request to a service and waits for the response before continuing
Request and response form a tightly coupled interaction: both parties must be available at the same time, and the consumer's control flow is blocked until the service replies or a timeout occurs
In service landscapes, synchronous communication is often implemented using HTTP-based APIs (e.g., REST, gRPC over HTTP/2)
It is the natural choice for user-driven, short-running operations such as reading data or performing simple updates
The consumer calls a service, the service calls other downstream services, and each synchronous step returns a result that is immediately used by the consumer

---
### 2.1.1 Synchronous Service chains


![](image/Pasted%20image%2020260218010735.png)

==The benefits of synchronous communication are a simple programming model and immediate feedback: the consumer either receives the requested data or an error status==
However, synchronous communication introduces **temporal coupling**: if a downstream service is slow or unavailable, the upstream consumer becomes slow or fails as well
In longer call chains, latency adds up and failures can propagate through the entire chain
Designing synchronous interactions therefore always includes reliability mechanisms such as timeouts, retries, and fallbacks
In many systems, critical user-facing paths are built on synchronous communication, while non-critical or long-running tasks are offloaded to asynchronous messaging


### 2.1.2 Reducing latencies with Worker Threads
![](image/Pasted%20image%2020260218011042.png)

The problem of increased latencies in service chains can be mitigated to execute synchronous calls in parallel, but a multi-threading approach inside the consumer, instead of sequentially
Each worker thread performs a standard synchronous request/response interaction with its downstream service and then returns the result to its upstream consumer
The total response time is now dominated by the slowest downstream call, not by the sum of all downstream latencies
Parallel synchronous calls are a useful optimization for read-heavy, user-facing requests, but they do not remove the fundamental temporal coupling of synchronous communication



**Benefits** of multi-threaded synchronous communication
It reduces the perceived latency for composite operations
It can improve throughput by better utilizing CPU and I/O while other threads are waiting
It fits naturally into existing synchronous APIs; downstream services do not need to change

**Trade-offs**
The internal implementation of the multi-threaded service becomes more complex (thread pools, futures/promises, async error handling)
More concurrent calls can increase load on downstream services and databases, potentially shifting the bottleneck

## 2.2 Reasons for not receiving a response


![](image/Pasted%20image%2020260218011243.png)

If the response does not arrive within a configured timeout, the consumer has to assume that something went wrong – but it does not know what exactly happened
Internally, several things may have happened
Request never left the consumer: Local crash, DNS error, misconfigured URL, connection refused
- Request was lost in transit: Network partition, broken load balancer, packet loss without retry
- Service is down: Crash, restart, deployment, power/network outage
- Service is overloaded: The request is queued but not processed before the timeout, or there are long latencies in the downstream service chain
- Response was lost: Business action may have succeeded, but the response never reached the consumer
All these cases look the same: the consumer experiences a timeout with unknown outcome

---

Fallback Strategies
Retry the request
- Works well for read-only or idempotent operations, but is dangerous for non-idempotent operations
- Combined with a retry limit (e.g., 2-3 attempts), backoff (wait longer between attempts), and a circuit breaker to avoid flooding an already unhealthy service

Return an error
- The consumer gives up and propagates the error upwards
- In a user-facing service, this becomes an error message in the UI

Return a degraded response
- Instead of failing, the consumer gets partial or stale data
- Use cached data or show the data being available

Switch to an alternative path
- Call a replica or backup service
- Try getting served from a computing centre in a different region

## 2.3 IDEMPOTENCY

![](image/Pasted%20image%2020260218011510.png)


==An operation is idempotent if executing it once or multiple times with the same input and on the same state leads to the **same resulting state**==
Idempotency is important for error handling in synchronous communication: if a call times out and the consumer retries, repeated execution must not create duplicate or inconsistent effects 

Idempotent methods in HTTP
GET - retrieve a resource
PUT - create/replace the resource at the given URI
DELETE – delete the resource
HEAD – like GET, but headers only
OPTIONS – ask for communication options
TRACE – diagnostic loopback

POST is not idempotent by definition, because it usually adds something, so repeating the same POST can create duplicates




## 2.4 Polling 

![](image/Pasted%20image%2020260218011935.png)

Polling is a ==synchronous communication== pattern where the consumer repeatedly sends a request (e.g., HTTP GET) to check whether some state on the server changed or a server-side event happened
Each poll is a normal synchronous request-response; the service responds immediately with the current state, even if nothing has changed
The consumer chooses a polling interval (e.g., 2 seconds, 10 seconds, 1 minute)
Polling is executed in a dedicated worker thread to avoid blocking in the consumer's main thread
Pros
Very simple to implement; uses standard synchronous HTTP
Works with existing infrastructure (load balancer, firewalls, proxies)
Client fully controls when and how often to ask
Cons
Potentially wasteful: many requests that just say "nothing changed"
If many client poll frequently, the pattern can overload backend servers


![](image/Pasted%20image%2020260218012103.png)

![](image/Pasted%20image%2020260218012114.png)

## 2.5 Long Polling 

![](image/Pasted%20image%2020260218012142.png)


Long Polling is a variant of polling where the consumer still sends a normal synchronous request, but the the service does not respond immediately if nothing has changed
Instead, it holds the request open until there is an update or a polling timeout occurs

Instead of many "nothing changed" responses, the consumer has fewer, longer requests, and only receives data when there is something new (or the polling timeout triggers)

Pros
Reduces the number of empty responses
Provides near real-time updates

Cons
More complex to implement on the server-side
Each open long-poll request ties up connection slots on servers, proxies, and gateways
Not as efficient as dedicated streaming mechanisms (WebSockets, Server-sent Events) for very frequent updates


![](image/Pasted%20image%2020260218012250.png)


![](image/Pasted%20image%2020260218012258.png)

# 3 Asynchronous Communication

## 3.1 FIRE-AND-FORGET

![](image/Pasted%20image%2020260218012338.png)

高龄初  producer和 consumer 的关系 
In a Fire-and-Forget interaction, the sender sends a message to another service and does not wait for a business response
The send operation completes as soon as the message is handed over to the communication layer; the sender continues its work immediately
For Fire-and-Forget, the consumer is the receiver of the message, not the HTTP client
In synchronous communication, the consumer sends a request to the services
==In asynchronous communication, the producer (sender, publisher) sends a request to the consumer (receiver, subscriber) ==
HTTP stays synchronous at the transport level (every request gets a response), but the HTTP response is treated as a technical acknowledgement only, not as the business result


Pros
Sender is non-blocking: "send" –> "continue work"
Decouples timing: receiver can process messages later, at its own speed
Simple with plain HTTP: no extra infrastructure needed

Cons
No immediate business outcome: "sent" ≠ "done successfully"
Error handling and retries must be designed explicitly
==Without a broker: no durable queue, no buffering, no dead-letter handling; if the receiver is down agt send time, messages can be lost==

![](image/Pasted%20image%2020260218012633.png)


## 3.2 ASYNCHRONOUS REQUEST/RESPONSE

![](image/Pasted%20image%2020260218012652.png)

Instead of doing all the work inside a single synchronous HTTP call, in the Asynchronous Request/Response pattern the service only accepts the request, starts background processing, and returns a status handle to the client
The client sends a request that might take a long time to process
The server does minimal work: validates the request, stores it, and starts processing asynchronously in the background
The server immediately responds with HTTP 202 Accepted and includes a URL to a status resource, which represents the long-running operation
The client can then poll this status URL until the operation is completed and the final result is available
The pattern explicitly models a long-running job as its own resource and returns 202 + status URL, while in polling/long polling the client repeatedly requests any resource to see whether its state has changed  



## 3.3 ASYNCHRONOUS CALLBACK

![](image/Pasted%20image%2020260218013002.png)

The Asynchronous Callback pattern is used to notify another service when something relevant has happened, without that service having to poll regularly
There are two main roles: the **producer**, where the interesting thing happens, and the **callback consumer**, which wants to be informed about it
==The callback consumer first registers a callback address with the producer – a URL, a queue, or some other endpoint==
==Whenever the event occurs, the producer sends a message to this address==
From the producer's perspective this is a fire-and-forget interaction: it pushes out the notification and does not wait for the full business processing result on the consumer side

---


Webhooks are simply the HTTP-based realization of the asynchronous callback pattern
==In this case, the callback address is a public HTTPS endpoint, for example POST /subscriptions/789, that the consumer exposes to the outside world
Whenever an event occurs, the producer issues an HTTP POST to this endpoint==

The body of the request contains the event payload, usually as JSON; headers may carry authentication information, signatures, or correlation identifiers
This approach is widely used in practice: payment providers inform shop backends about completed or failed payments, Git platforms notify CI systems about new commits, and SaaS applications push customer events into CRM systems



# 4 BROKER-BASED ASYNCHRONOUS MESSAGING

## 4.1 MESSAGE BROKERS

![](image/Pasted%20image%2020260218013938.png)


A message broker is a middleware component that takes over the responsibility of transporting messages between services
Instead of sender and receiver talking to each other directly, both sides talk only to the broker: producers publish messages to the broker, consumers retrieve messages from it
This indirection decouples the two sides in time (sender and receiver do not have to be up at the same time), space (they do not need to know each other's network address), and load (the broker can buffer bursts of messages and smooth them out over time)
In modern service landscapes, message brokers form the backbone of asynchronous communication: they enable fire-and-forget interactions at scale, support event-driven architectures, and help isolate failures between services

## 4.2 MESSAGE QUEUES

![](image/Pasted%20image%2020260218014021.png)

In the Message Queue pattern, a service does not send work directly to another service, but places a message into a queue managed by a broker
One or more worker services then take messages from this queue and process them independently; each message is handled exactly once by a single consumer and is removed afterwards
This indirection decouples producers and consumers in time and in load
==The broker can additionally take care of persistence, retries, and dead-letter handling so that individual failures do not immediately propagate through the system==
A load balancer distributes synchronous client requests across service instances; a message broker buffers and routes asynchronous messages between decoupled services
## 4.3 PUB/SUB

![](image/Pasted%20image%2020260218014105.png)

In the Publish/Subscriber pattern, services do not send messages to specific consumers, but publish events to an event broker
Each event describes that something has happened in the domain (e.g., "Order Shipped")
Other services that are interested in this kind of event subscribe to the corresponding topic or channel
When a new event is published, the broker forwards it to all current subscribers
==Compared to the message queue, the focus is not on distributing work to a single worker, but on distributing information to many listeners==
Producers stay unaware of who consumes their events, and new consumers can be added without changing the publisher 

## 4.4 EVENT LOGS/STREAMS

![](image/Pasted%20image%2020260218014207.png)

In the Event Log (or Event Stream) pattern, a broker does not just forward events to consumers, but maintains a durable, append-only log of all events on a stream or topic
Producers continuously append new events to this log; they are never updated or deleted, only added at the end
Each consumer keeps its own position (offset) in the log and can read events at its own pace, jump back to an earlier offset, or start from the beginning to replay the full history
Compared to simple pub/sub, the focus is not only on distributing events to many listeners, but also on preserving an ordered history of what happened in the system
This enables services to rebuild their local state from the event history, new services to catch up by replaying past events, and analytics to continuously process the stream



## 4.5 COMPARISON OF BROKER PATTERNS

![](image/Pasted%20image%2020260218014316.png)

![](image/Pasted%20image%2020260218014325.png)

# 5 RABBITMQ

- RabbitMQ is an open-source message broker that sits between producers and consumers and takes care of reliable message delivery
- It implements the AMQP protocol messaging model, where messages are published to exchanges and then routed into queues from which one or more consumers can pick them up
- RabbitMQ supports multiple protocols (AMQP, MQTT, STOMP), offers features such as durable queues, acknowledgements, and routing patterns (direct, fan-out, topic), and can be clustered for high availability
- In many microservice architectures it is used as a central building block for ==decoupling services== and implementing the message queue and pub/sub patterns discussed before
## 5.1 AMQP

![](image/Pasted%20image%2020260218103253.png)

The AMQP (Advanced Message Queuing Protocol) is an open, standardized wire protocol for message-oriented middleware
It defines how clients, brokers, and messages interact on the network level – independent of any specific implementation
AMQP specifies core concepts such as exchanges, queues, bindings, and routing keys, as well as how messages are delivered reliably with acknowledgement, durability, and transactions
AMQP is an application-layer protocol that runs on top of a long-lived TCP connection, optionally protected by TLS for encryption and authentication


A queue is a named, durable buffer that stores messages until they are consumed
It is an ordered sequence of messages to which exchanges can append messages and from which consumers remove messages (typically First-In-First-Out, FIFO)
Queues are the only place where messages persist between sending and consumption
Consumers subscribe to a queue and receive messages in delivery order; acknowledgement mechanisms determine when messages are removed from the queue or re-sequenced in case of failure

### 5.1.1 Exchange
An exchange is a named entity in RabbitMQ that accepts messages from producers and routes them to one or more queues according to a routing algorithm
Exchanges do not store messages; they only decide where messages should go

Different exchange types (direct, topic, fanout, headers) correspond to different routing functions
- A direct exchange routes based on exact matching of routing keys and binding keys
- A topic exchange routes based on pattern matching
- A fanout exchange broadcasts to all bound queues, ignoring the routing key

### 5.1.2 Bindings
Bindings connect an exchange to queues and specifiy the conditions under which a queue should receive a message

### 5.1.3 Queue

A queue is a named, durable buffer that stores messages until they are consumed
It is an ordered sequence of messages to which exchanges can append messages and from which consumers remove messages (typically First-In-First-Out, FIFO)
Queues are the only place where messages persist between sending and consumption
Consumers subscribe to a queue and receive messages in delivery order; acknowledgement mechanisms determine when messages are removed from the queue or re-sequenced in case of failure

Queues come with different properties that influence reliability and lifetime:
- Durable queues survive a broker restart; combined with persistent messages, which allows messages to survive crashes
- Non-durable queues exists only in memory; they are lost when the broker restarts
- Exclusive queues can only be used by the connection that declared it and is deleted when that connection closes
- Non-exclusive queues can be shared by multiple connections/consumers
- A queue marked as auto-delete is automatically removed when the last consumer unsubscribes, which is useful for temporary or per-session queues

## 5.2 Message Queue with one Consumer

![](image/Pasted%20image%2020260218104552.png)


![](image/Pasted%20image%2020260218104215.png)

Establishes a long-lived TCP connection

Establishes a session inside a connection

Callback called when channel is confirmed

Establishes a channel or use the existing one with this name

Send message 

Producer.js
```javascript
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
 
        var queue = 'hello';
        var msg = 'Hello World!';
 
        channel.assertQueue(queue, {
            durable: false
        });
        channel.sendToQueue(queue, Buffer.from(msg));
 
        console.log(" [x] Sent %s", msg);
    });
    setTimeout(function() {
        connection.close();
        process.exit(0);
    }, 500);
});
```


---
Consumer.js
![](image/Pasted%20image%2020260218104234.png)

Establishes a channel or use the existing one with this name

Subscribe to the queue 'hello' and whenever a message arrives, call the callback function to process it locally

Consumer.js
```javascript
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
 
        var queue = 'hello';
 
        channel.assertQueue(queue, {
            durable: false
        });
 
        console.log(" [*] Waiting for messages in %s. To exit press CTRL+C", queue);
 
        channel.consume(queue, function(msg) {
            console.log(" [x] Received %s", msg.content.toString());
        }, {
            noAck: true
        });
    });
});
```


## 5.3 Work Queues


![](image/Pasted%20image%2020260218104345.png)

![](image/Pasted%20image%2020260218104357.png)


Producer.js
```javascript
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
 
        var queue = 'task_queue';
        var msg = process.argv.slice(2).join(' ') || "Hello World!";
 
        channel.assertQueue(queue, {
            durable: true
        });
        channel.sendToQueue(queue, Buffer.from(msg), {
            persistent: true
        });
 
        console.log(" [x] Sent %s", msg);
    });
    setTimeout(function() {
        connection.close();
        process.exit(0);
    }, 500);
});
```

Get message as an argument when starting the producer in the shell

Queue survives the shutdown of the broker and is reloaded on restart

Message survives the shutdown of the broker and is reloaded on restar

---

![](image/Pasted%20image%2020260218104437.png)

Calculation of the duration for processing the fake workload

Faking the workload for processing the message

Worker.js
```javascript
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
 
        var queue = 'task_queue';
 
        channel.assertQueue(queue, {
            durable: true
        });
 
        channel.consume(queue, function(msg) {
            var secs=msg.content.toString().split('.').length-1;
 
            console.log(" [x] Received %s", msg.content.toString());
            setTimeout(function() {
                console.log(" [x] Done");
            }, secs*1000);
        }, {
            noAck: true
        });
    });
});
```


## 5.4 Publish/Subscribe

![](image/Pasted%20image%2020260218105804.png)

Producer.js
![](image/Pasted%20image%2020260218110201.png)
```
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
 
        var exchange = 'logs';
        var msg = process.argv.slice(2).join(' ') || "Hello World!";
 
        channel.assertExchange(exchange, 'fanout', {
            durable: false
        });
        channel.publish(exchange, '', Buffer.from(msg));
        console.log(" [x] Sent %s", msg);
    });
    setTimeout(function() {
        connection.close();
        process.exit(0);
    }, 500);
});
```

Establishes an exchange or use the existing one with this name

Sends a message to this exchange (the empty argument ' ' is the routing key not being used for fanout exchanges


---
Worker.js
![](image/Pasted%20image%2020260218110210.png)
```javascript
var amqp = require('amqplib/callback_api');
 
amqp.connect('amqp://localhost', function(error0, connection) {
    if (error0) {
        throw error0;
    }
    connection.createChannel(function(error1, channel) {
        if (error1) {
            throw error1;
        }
        var exchange = 'logs';
        channel.assertExchange(exchange, 'fanout', {
            durable: false
        });
 
        channel.assertQueue('', {
            exclusive: true
        }, function(error2, q) {
            if (error2) {
                throw error2
            }
            console.log(" [*] Waiting for messages in %s. To exit press CTRL+C", q.queue);
            channel.bindQueue(q.queue, exchange, '');
            channel.consume(q.queue, function(msg) {
                if (msg.content) {
                    console.log(" [x] %s", msg.content.toString());
                }
            }, {
                noAck: true
            });
        });
    });
});

```

Establishes an exchange or use the existing one with this name

Makes sure that is worker instance is the only one listening to that queue

Connects the queue to the exchange

## 5.5 Stream Processing

![](image/Pasted%20image%2020260218110315.png)

RabbitMQ Streams extends the classic message broker with a long-style storage model: ==producers continuously append messages to an ordered stream, and consumers read them sequentially based on their position (offset) in that stream==
This allows multiple consumers to process the same data at different speeds, replay past messages, and build event-driven applications that need both high throughput and history (e.g., analytics, monitoring,...)
RabbitMQ streams rely on a dedicated binary RabbitMQ Stream Protocol over TCP port 5552, rather than AMQP


---

Publisher.js
![](image/Pasted%20image%2020260218110443.png)

```javascript
const rabbit = require("rabbitmq-stream-js-client");

async function main() {
  console.log("Connecting...");
  const client = await rabbit.connect({
    vhost: "/",
    port: 5552,
    hostname: "localhost",
    username: "guest",
    password: "guest",
  });

  console.log("Making sure the stream exists...");
  const streamName = "stream-offset-tracking-javascript";
  await client.createStream({ stream: streamName, arguments: {} });

  console.log("Creating the publisher...");
  const publisher = await client.declarePublisher({ stream: streamName });

  const messageCount = 100;
  console.log(`Publishing ${messageCount} messages`);
  for (let i = 0; i < messageCount; i++) {
    const body = i === messageCount - 1 ? "marker" : `hello ${i}`;
    await publisher.send(Buffer.from(body));
  }

  console.log("Closing the connection...");
  await client.close();
}

main()
  .then(() => console.log("done!"))
  .catch((res) => {
    console.log("Error in publishing message!", res);
    process.exit(-1);
  });
```

Connect to the stream endpoint

Creates the stream under the given name (idempotent operation: if the stream already exists, nothing is changed)

Defines the publisher

Write 100 messages to the stream with the last message being a marker indicating the latest stream head

----

consumer.js – Part 1
![](image/Pasted%20image%2020260218110605.png)
```javascript
const rabbit = require("rabbitmq-stream-js-client");

async function main() {
  console.log("Connecting...");
  const client = await rabbit.connect({
    hostname: "localhost",
    port: 5552,
    username: "guest",
    password: "guest",
    vhost: "/",
  });

  console.log("Making sure the stream exists...");
  const streamName = "stream-offset-tracking-javascript";
  await client.createStream({ stream: streamName, arguments: {} });

  const consumerRef = "offset-tracking-tutorial";

  const cliOffsetArg = process.argv[2];
  let offsetSpecification;
  let info = "";

  if (cliOffsetArg !== undefined) {
    const cliOffset = BigInt(cliOffsetArg);
    offsetSpecification = rabbit.Offset.offset(cliOffset);
    info = `starting offset overridden from CLI: ${cliOffset}`;
  } else {
    offsetSpecification = rabbit.Offset.first();
    try {
      const storedOffset = await client.queryOffset({
        reference: consumerRef,
        stream: streamName,
      });
      offsetSpecification = rabbit.Offset.offset(storedOffset + 1n);
      info = `resuming from stored offset on server: ${storedOffset} -> start at ${storedOffset + 1n}`;
    } catch (e) {
      info = "no stored offset found -> start at beginning (offset 0)";
    }
  }
```

Connect to the stream endpoint

Creates the stream under the given name (idempotent operation: if the stream already exists, nothing is changed)

Name of the consumer-specific read offset

If consumer was started with an offset as command-line-argument, use this offset to address the first message to be read

If the consumer was started without an offset argument, get the last offset from the server and continue with the next message after the offset

---

consumer.js – Part 2
```javascript

  const startingOffset = offsetSpecification.value;
  let messageCount = 0;
  let firstOffset;
  console.log(`Starting consumption at offset ${startingOffset} (${info})`);

  const consumer = await client.declareConsumer(
    { stream: streamName, offset: offsetSpecification, consumerRef },
    async (message) => {
      messageCount++;
      console.log(
        `[x] Received message #${messageCount} at offset ${message.offset}:`,
        message.content.toString()
      );

      if (!firstOffset && messageCount === 1) {
        firstOffset = message.offset;
        console.log("First message received");
      }

      if (messageCount % 10 === 0) {
        await consumer.storeOffset(message.offset);
      }

      if (message.content.toString() === "marker") {
        console.log("Marker found");
        await consumer.storeOffset(message.offset);
      }
    }
  );
  console.log("Consumer is now running and waiting for messages...");
}

main().catch((err) => {
  console.log("Error while starting consumer!", err);
  process.exit(-1);
});


```

Defines the consumer for a specific stream and with a callback that is invoked whenever a new message arrives

Prints every message to the console

After every tenth message received, persist the current offset on the server

Marker messages indicate the current and a former head of the stream; persist the current offset if such a marker is received

#### 5.5.1.1 中文解释 offset


在 RabbitMQ 流处理中，**Offset**（偏移量）是一个核心概念，它表示消费者在流中的**读取位置**。你可以把它想象成一个书签或者一个进度指针，标记了消费者当前处理到哪一条消息了。

以下是关于 RabbitMQ Stream 中 Offset 的详细解释：

Offset 的本质是什么？
-   **单调递增的整数**：在 RabbitMQ 的流中，消息一旦被追加到流中，就会被分配一个从 0 开始的、严格单调递增的序列号。这个序列号就是 Offset。
-   **绝对位置**：Offset 代表了消息在流中的绝对位置。消费者可以通过指定 Offset 来精确地定位到某一条消息。


---

为什么 Offset 很重要？
与传统的队列（Queue）不同，RabbitMQ 的 Stream 是一个**持久化的、只能追加的日志结构**。在传统队列中，消息一旦被消费确认（ACK）通常就会被删除。但在 Stream 中：
-   **消息不会被删除**：消息会一直保留在流中，直到磁盘空间上限（`max-age` 或 `max-length-bytes`）被触发。
-   **Offset 决定读取什么**：消费者并不是"拿走"消息，而是"读取"消息。Offset 决定了消费者从哪里开始读。

---

当触发上限（如 max-age 或 max-length-bytes）时，消息并不会被立即删除。RabbitMQ Streams 采用了一种基于段（Segment）的、惰性的删除策略。

好问题，这个细节很关键。当触发上限（如 `max-age` 或 `max-length-bytes`）时，**消息并不会被立即删除**。RabbitMQ Streams 采用了一种基于**段（Segment）**的、**惰性的**删除策略。

以下是触发上限后发生事情的具体步骤：

1.  **消息被标记为"可删除"**：当流达到配置的年龄或大小上限时，相关消息不会立刻消失，而是被逻辑上标记为"过期"或"符合删除条件"。此时，它们依然物理存储在磁盘上，并且如果你尝试从很早的 Offset 开始消费，这些消息可能在一段时间内仍然可读。

2.  **等待段（Segment）切换**：Stream 在磁盘上是由多个**段文件**组成的。RabbitMQ 会持续将新消息写入一个活跃的段文件，直到该文件达到上限（由 `x-stream-max-segment-size-bytes` 决定）。只有当一个段文件被**关闭**（即切换到新段）后，它才有可能被删除。

3.  **执行删除操作**：在段文件关闭后，RabbitMQ 会定期检查所有已关闭的段。如果一个段中的所有消息都已过期（超过 `max-age`），并且整个流的大小超过了 `max-length-bytes` 的限制，这个段文件就会被从磁盘上彻底删除，从而释放磁盘空间。

关键特性与例外
-   **至少保留一个段**：为了保证 Stream 的正常运行逻辑，即使所有数据都已过期，RabbitMQ 也**始终会保留至少一个段文件**（只要这个段里至少包含一条消息）。这意味着 Stream 永远不会变成完全空的。
-   **监控指标的差异**：你可能会在管理界面看到"Ready"消息数量一直增长，即使设置了保留策略。这是因为该计数反映的是所有未过期的消息总和，而删除动作是滞后的。因此，这个指标对于 Stream 来说，并不像传统队列那样能准确反映待处理的工作量。




---

Offset 的存储方式（服务端追踪）
RabbitMQ 的 Stream 插件提供了**服务端偏移量追踪**功能。这意味着消费者不需要在本地记住自己读到哪里了，RabbitMQ 服务器会帮你记着。

-   **存储名（Store）**：消费者在消费时，可以指定一个名称（通常称为消费者名称或引用名称）。RabbitMQ 会以这个名称作为 key，将当前已处理的 Offset 存储在服务端。
-   **故障恢复**：如果消费者进程崩溃后重启，并再次使用同一个名称连接，它可以告诉服务器："从上一次存储的 Offset 之后开始消费。" 这样就实现了**断点续传**。



---

常用的 Offset 策略（从哪里开始读？）
当消费者（Consumer）首次连接或手动指定时，可以通过设置 Offset 策略来决定从流的哪个位置开始读取消息。常见的策略包括：

1.  **`next` （下一个）**：
    -   只读取消费者启动后**新写入**的消息。
    -   *相当于说："我只关心从现在开始发生的新事件。"*

2.  **`first` （第一个）**：
    -   从流中现存的最早的消息（Offset 0）开始读。
    -   *相当于说："我要重放整个历史数据。"*

3.  **`last` （最后一个）**：
    -   从流中最后一条消息的下一个位置开始读（通常意味着读取即将到来的最新消息）。
    -   *相当于说："我要接上最新的。"*

4.  **`offset （偏移量）`**：
    -   明确指定一个数字 Offset，从该位置开始读。
    -   *相当于说："请从第 10,000 条消息开始给我。"*

5.  **`timestamp （时间戳）`**：
    -   指定一个时间点，RabbitMQ 会找到该时间点对应的 Offset，然后从这里开始读。
    -   *相当于说："把昨天早上 8 点之后的消息给我。"*


----


一个简单的比喻
想象一个无限长的书架（Stream）：

-   **Offset**：就是书架上的页码（0， 1， 2， 3...）。
-   **消费者**：就像一个读者。
-   **服务端存储**：就像管理员有一个笔记本，记录了读者"张三"上次读到了第 100 页。

如果"张三"今天回来继续读，他告诉管理员："我还叫张三，继续读。" 管理员看笔记本说："你上次读到了 100 页，现在从 101 页开始看吧。" 这就实现了精确的一次处理语义。


总结
在 RabbitMQ Stream 中，**Offset 是消息的索引**。它结合服务端的追踪能力，使得 RabbitMQ 能够支持**大容量日志存储、消息重放、以及精确的故障恢复**，这是 Stream 插件相比传统 AMQP 队列的核心优势之一。

