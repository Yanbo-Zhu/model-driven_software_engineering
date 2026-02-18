
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


An operation is idempotent if executing it once or multiple times with the same input and on the same state leads to the **same resulting state**
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

Polling is a synchronous communication pattern where the consumer repeatedly sends a request (e.g., HTTP GET) to check whether some state on the server changed or a server-side event happened
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
Compared to the message queue, the focus is not on distributing work to a single worker, but on distributing information to many listeners
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

RabbitMQ is an open-source message broker that sits between producers and consumers and takes care of reliable message delivery
It implements the AMQP protocol messaging model, where messages are published to exchanges and then routed into queues from which one or more consumers can pick them up
RabbitMQ supports multiple protocols (AMQP, MQTT, STOMP), offers features such as durable queues, acknowledgements, and routing patterns (direct, fan-out, topic), and can be clustered for high availability
In many microservice architectures it is used as a central building block for decoupling services and implementing the message queue and pub/sub patterns discussed before