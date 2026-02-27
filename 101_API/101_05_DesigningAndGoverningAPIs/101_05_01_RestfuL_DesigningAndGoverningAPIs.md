# 1 Comparision


![](image/Pasted%20image%2020260218113439.png)


Approach	What you define	Strengths	Trade-offs	Use it when

REST	Resources identified by URIs + representations + uniform operations via HTTP semantics (verbs, status codes, caching)	Simple mental model, great tooling (OpenAPI), easy debugging, HTTP caching/CDN friendliness, wide interoperability	Multiple round-trips for rich graphs; "over/under-fetching"; weak built-in server push; strictness varies a lot in practice	Public-facing APIs; CRUD-ish domains (YAOS: Products, Carts, Orders); integrations where standards + caching + observability matter most

GraphQL	A strongly-typed schema of types/fields + query/mutation operations (often one endpoint); server resolves requested fields.	Client gets exactly the data it needs; great for UI-driven apps; single round-trip for data graphs; strong introspection/tooling.	More complex server/resolvers; performance pitfalls (N+1); caching needs extra strategy; transport/auth/pagination are largely "convention".	Frontends that aggregate many backend resources (YAOS storefront: product + availability + price + recommendations in one shot); when client flexibility beats simple caching

gRPC	Service methods in .proto (IDL) + generated stubs; binary messages (often Protobuf) over HTTP/2, including streaming RPCs.	High performance/low latency; strong typing; great internal S2S; built-in streaming patterns; codegen across languages.	Browser support needs gRPC-Web/proxies; less human-readable; not cache/CDN friendly like REST; operationally "heavier" for public APIs.	Backend-to-backend in microservices (YAOS internal calls: Order → Payment/Shipping/Inventory); latency-sensitive RPC; high-throughput service meshes.

WebSockets	An application-level message protocol (message types + JSON/binary schemas + conversation rules) over a long-lived, full-duplex channel; handshake then message framing over TCP. 	True server push + low-latency interaction; efficient alternative to polling/long polling; ideal for real-time UIs. 	You must define semantics yourself (errors, correlation, versioning, auth); stateful connections complicate scaling/load balancing; not cacheable like REST.	Real-time features (YAOS: order status live updates, chat/support, live inventory/price ticks, dashboards). Also used as transport for GraphQL subscriptions in practice.

# 2 RESTful APIs


![](image/Pasted%20image%2020260218113932.png)

- ==REST (Representational State Transfer) is a paradigm for creating RESTful APIs==
- RESTful APIs expose resources over HTTP, using standard HTTP methods like GET, POST, PUT, and DELETE
- Resources are typically identified by URIs (Uniform Resource Identifiers)
- A resource can be available in different representations, which are serialization formats like JSON (JavaScript Object Notation), XML (Extended Markup Language), and CSV (Comma-Separated Values) 

---

REST Usage of HTTP Requests and Responses

![](image/Pasted%20image%2020260218114831.png)

---

## 2.1 Resources and Resource structures

![](image/Pasted%20image%2020260218114857.png)

A service based on a RESTful API consists of multiple resources
==A resource is a coherent data set describing a relevant domain artifact in the context of the application for which the service is built==
A resource type is a set of resources that share the same data model, that is, they are described by the same set of attributes but differ in the value of those attributes 

RESTful APIs expose resources in different structural forms
- An **item resource** is a single entity that represents an atomic unit of data
- A **collection resource** is a group of resources that belong to the same resource type, forming a group relationship
- A **sub-resource** is a resource that exists in the context of another resource, forming an encapsulation relationship

![](image/Pasted%20image%2020260218115236.png)

## 2.2 ADDRESSING RESOURCES

![](image/Pasted%20image%2020260218115239.png)


- Each resource is identified by a unique URI (Uniform Resource Identifier)
- The structural properties of a resource – such as being a sub-resource or a collection – are reflected in the path part of the URI
- The resulting URI must uniquely identify the resource in the context of the application
- /products refers to the collection of all products
- /products/123456 refers to a concrete product with the identifier 123456
- /orders/234567/items refers to the collection of items in order 2345657
- /orders/234567/items/123456 refers to a specific item within that order


## 2.3 Resource Representations

![](image/Pasted%20image%2020260218115817.png)



JSON
```
{
  "id": 42,
  "name": "Bluetooth-Lautsprecher",
  "category": "ELECTRONICS",
  "price": 4599,
  "available": true
}

```


XML
```
<product>
  <id>42</id>
  <name>Bluetooth-Lautsprecher</name>
  <category>ELECTRONICS</category>
  <price>4599</price>
  <available>true</available>
</product>
```


CSV
```
id,name,category,price,available
42,Bluetooth-Lautsprecher,ELECTRONICS,4599,true
```

- The same resource can be represented in different serialization formats
- The desired representation is negotiated using the HTTP Accept header
    - Accept: application/json
    - Accept: application/xml
    - Accept: text/csv
- The resource itself remains the same, but its representation varies depending on the client's preferences 


## 2.4 USE OF HTTP VERBS 

![](image/Pasted%20image%2020260218120409.png)


RESTful APIs use standard HTTP methods to operate on resources
These methods define what kind of operation the client wants to perform
A method is safe if it does not modify the resource
A method is idempotent if repeating it has the same effect as doing it once
Safe methods can be cached, pre-fetched, and retried without risk
Idempotent methods can be safely retried if a network error occurs 

---

1 GET

![](image/Pasted%20image%2020260218120541.png)

---

2 POST


![](image/Pasted%20image%2020260218120705.png)

----

3  Put

![](image/Pasted%20image%2020260218120806.png)


----

4 Patch 

![](image/Pasted%20image%2020260218120817.png)


 PATCH 不保证幂
```
// 原始资源
{ "counter": 5 }

// PATCH 请求（增加计数）
PATCH /resource/1
{ "counter": "counter + 1" }  // 实际中可能是某种表达式

// 多次执行这个请求，每次都会增加计数
第一次：counter = 6
第二次：counter = 7
第三次：counter = 8
```


 什么时候用 PATCH vs PUT？

使用 PATCH 的场景：
只更新资源的少量字段
频繁的部分更新
大资源文档的部分修改
需要原子性操作的复杂更新

使用 PUT 的场景：
完整替换资源
创建资源（PUT 也支持创建）
需要保证幂等性
客户端知道资源的完整状态


---

5 Delete


![](image/Pasted%20image%2020260218120909.png)

## 2.5 Parameter Types

![](image/Pasted%20image%2020260218121027.png)

- Path parameters are part of the URI path and identify specific resources
- They are used to locate a resource by its unique ID or position in a hierarchy


- Query parameters are appended to the URI after a ? and used to filter, sort, or paginate results
- They do not identify resources, but modify the scope of a request to a collection


- Header parameters are part of the HTTP header, not the URI
- They are used to pass metadata, such as content types, authentication tokens, or language preferences



## 2.6 OPENAPI


OpenAPI is a ==standard== for describing RESTful APIs in a machine-readable format
==It defines the endpoints, methods, parameters, request/response formats, and authentication of an API==
The ==specification== is written in JSON or YAML
It allows humans and machines to understand how the API works without reading the source code
Tools can use OpenAPI to generate API documentation, server stubs, client SDKs, and mock servers


---

From API Specification to Tooling
![](image/Pasted%20image%2020260218121321.png)


In OpenAPI, t==he API specification defines the endpoints of resources and HTTP methods to access them, request and response structures, as well as status codes, headers, and media types==
From this specification, the service's documentation, validation tools, and client SDKs and server stubs are derived
The documentation is a human-readable and interactive API documentation, which enables exploration and testing of endpoints
Validation tools analyze requests and responses during runtime (e.g., in gateways or middleware)
Client SDKs are used to automatically construct HTTP requests and parse response based on the HTTP specification
A server stub represents the endpoint matching the OpenAPI specification, which must be filled with the business logic by the developer 

OpenAPI Example
![](image/Pasted%20image%2020260218121609.png)



## 2.7 HYPERMEDIA CONTROLS

![](image/Pasted%20image%2020260218121643.png)

==Hypermedia Controls are links or actions embedded in resource representations that guide clients through available operations dynamically==
==They turn a RESTful API into a navigable application, where clients discover what they can do based on links provided in the current response==
They make APIs self-descriptive and reduce the hard-coding of URI structures inside clients
The _link section provides Hypermedia Controls
Clients can follow update to cancel the order, customer to see the customer profile, or items to view order details
self is a link to the current resource
`HATEOAS (Hypermedia As the Engine Of Application State) `is a REST principle where the client interacts with the application entirely via hypermedia links included in responses 


---

![](image/Pasted%20image%2020260218121929.png)



## 2.8 RICHARDSON MATURITY MODEL

a framework developed by Leonard Richardson to evaluate the maturity of a web API based on its adherence to REST principles, categorized into four levels (0-3).

![](image/Pasted%20image%2020260218122003.png)

Problem: Many web services and their APIs are declared RESTful by their developers, but they implement the associated concepts only partially – or mix them with other paradigms for web application development, such as SOAP or XML-RPC
The Richardson Maturity Model (RMM) is a helpful framework for assessing how RESTful an API is
It was introduced by Leonard Richardson and popularized by Martin Fowler
Each level adds more REST principles – from simple HTTP to full hypermedia-driven systems

---

Level 0 – The Swamp of POX
"Plain Old XML/JSON over HTTP"
Everything goes through a single endpoint, like /api
Often only uses POST
Payload in the HTTP body defines actions like {"action" : "createOrder"}

```
POST /api
{
  "method": "getProduct",
  "params": { "id": 42 }
}

```


---

Level 1 – Resources
"Introduce nouns"
Each entity has its own resource URI
Still often uses only one HTTP verb (POST)

```
POST /orders/42

```


---



Level 2 – HTTP Methods
"Use the web as it was designed"
Resources are accessed using the correct HTTP methods

```
GET    /products/42
PATCH  /orders/42   { "status": "cancelled" }
DELETE /orders/42
```



Level 3 – Hypermedia Controls (HATEOAS)
"Discoverable APIs"
Clients navigate the API by following the links included in responses
Reduces coupling: clients don't need to know the full URI structure 

```
{
  "id": 42,
  "status": "placed",
  "_links": {
    "self": { "href": "/orders/42" },
    "cancel": { "href": "/orders/42", "method": "PATCH", 
      "body": { "status": "cancelled" } }
  }
}
```



## 2.9 REMOTE PROCEDURE CALL

![](image/Pasted%20image%2020260218122547.png)

APIs based on RPC (Remote Procedure Call) expose a service as a set of remote operations rather than as a collection of resources
The API typically provides one single endpoint (e.g., /rpc or /api)
All requests are sent to this endpoint, independent of the operation
The operation (name) is carried inside the request message (e.g., createProduct, getProduct)
Request messages contain the operation to execute and the input parameters for that operation
The server executes the operation and returns the result in the response body
==RPC-style APIs centralize all calls at a single endpoint and encode the invoked operation inside the message, whereas RESTful APIs distribute functionality across resource-specific endpoints and rely on HTTP semantics==

RPC 风格的 API 将所有调用集中在一个单一端点上，并将被调用的操作编码在消息内部；而 RESTful API 则将功能分布到特定于资源的端点上，并依赖于 HTTP 语义。


## 2.10 State Transfer


![](image/Pasted%20image%2020260218123531.png)

REST stands for Representational State Transfer – the distinction between client and server state is important
==The application state lives in the client, i.e., the server can influence it by sending representations that suggest what the client could do next (e.g., via HATEOAS links)==
The resource state lives in the server, i.e., the client can influence it by sending representations (requests with data) that change the resource's state


## 2.11 REST Drawbacks


Overfetching
- Fixed resource representations often include more data than a client needs
- Example: /products returns fields not required by a mobile UI

Underfetching
- Clients must call multiple endpoints to assemble one view
- Examples: product –> reviews –> stock –> recommendations 

Chattiness
- Multiple sequential HTTP requests increase latency
- Particularly problematic for mobile networks and high-latency links 



# 3 GRAPHQL


GraphQL is a ==query language for APIs ==and a runtime for executing those queries with existing data
Originally developed by Facebook in 2012, open-sourced in 2015
==Unlike REST, where you fetch data from different endpoints, GraphQL lets you fetch exactly the data you need in a single request==
It uses a strongly typed schema to describe what is possible in the API



## 3.1 SCHEMA

![](image/Pasted%20image%2020260218140851.png)

```
type Product {
  id: ID!
  name: String!
  available: Boolean!
  price: Int!
  category: String!
}

type Query {
  product(id: ID!): Product
  allProducts: [Product!]!
}
```
- A GraphQL schema is the core contract between client and server
- ==A GraphQL Schema is the strongly-typed metadata definition of a GraphQL API. It serves as the contract that explicitly defines all possible data interactions between the client and the server.==


- It defines
    - What data is available
    - How it can be queried or mutated
    - The structure and types of responses
    - The entry points into the API
- A schema is a strongly-typed blueprint that describes the capabilities of the GraphQL API
- The schema is defined first, the resolvers and services are implemented to match
- It enables automatic documenting of the API and validation during compile/runtime


GraphQL APIs receive requests over HTTP, ==typically as POST requests with a JSON payload==
==Unlike REST, there is only a single endpoint (e.g., /graphql) that handles all requests==
GraphQL responses always return a JSON object with either a data field (successful operation), and/or an errors array (failed execution or validation)
GraphQL allows partial success – some fields can resolve while others fail

---
## 3.2 Example 


### 3.2.1 Example Schema 
![](image/Pasted%20image%2020260218142157.png)


```
type Product {
  id: ID!
  name: String!
  price: Int!
  available: Boolean!
  category: Category!
}

enum Category {
  ELECTRONICS
  FASHION
  BOOKS
  FOOD
  OTHER
}

input ProductInput {
  name: String!
  price: Int!
  available: Boolean!
  category: Category!
}

type Query {
  product(id: ID!): Product
  allProducts: [Product!]!
}

type Mutation {
  addProduct(input: ProductInput!): Product
}
```



### 3.2.2 EXAMPLE QUERIES

![](image/Pasted%20image%2020260218142304.png)

![](image/Pasted%20image%2020260218142316.png)

![](image/Pasted%20image%2020260218142346.png)

### 3.2.3 Mutation

![](image/Pasted%20image%2020260218142355.png)


## 3.3 Mutation

![](image/Pasted%20image%2020260218142750.png)

- A mutation is a write operation that changes server-side state
- Unlike queries, which are read-only, mutations are used to create, update, or delete data
- A mutation is executed once and returns a response immediately after completion
- Mutations explicitly define which fields should be returned in the response, which allows the client to receive confirmation data
- Multiple mutations are executed sequentially, ensuring a well-defined order of side effects
- GraphQL does not define idempotency semantics for mutations
- Whether a mutations is idempotent or not depends entirely on its business semantics and implementation

## 3.4 SUBSCRIPTIONS

![](image/Pasted%20image%2020260218142842.png)

Schema for a Subscription
```
type Subscription {
  productAdded: Product
}
```


Client Subscription Request
```
subscription {
  productAdded {
    id
    name
    category
    available
  }
}
```

Server push Responses (Streamed over Time)
```
{
  "data": {
    "productAdded": {
      "id": "xyz789",
      "name": "Espressokocher",
      "category": "FOOD",
      "available": true
    }
  }
}
```

```
{
  "data": {
    "productAdded": {
      "id": "abc123",
      "name": "Gaming-Maus",
      "category": "ELECTRONICS",
      "available": true
    }
  }
}
```


- A subscription is a long-lived operation that allows the server to push data to the client over time
- Unlike queries or mutations, which are executed once, subscriptions stay open
- Technically, they require a WebSocket connection, not plain HTTP
- ==Use subscriptions when the client needs to react to events in real time, such as a chat message is sent, a product's availability changes, or an order status changes==

## 3.5 Schema, Server-side resolvers, Client side bindings

Schema as a runtime Contract


![](image/Pasted%20image%2020260218143935.png)

In GraphQL, the schema is loaded and interpreted at runtime by the GraphQL engine and acts as the central contract for request processing
During runtime, the schema is used to validate incoming requests and mutations, determine which resolver functions must be executed, and enforce type constraints
The ==server-side resolvers ==are invoked according to the schema by the GraphQL engine
==Client-side bindings== are manually written queries and mutations based on the understanding of the schema
Schema validation is enforced by the server at runtime, not by the client 

Client-side bindings：
- 因为 GraphQL 的服务端暴露了一个 Schema（契约），规定了能做什么操作（如 user、createOrder）以及需要什么数据格式。
- "理解"：指的是前端开发者阅读了这个 Schema 文档，知道了存在一个 user 查询，需要传入 id 参数，会返回 name 和 email。
- "手动编写"：知道了上述信息后，开发者在前端代码里手动写出对应的查询语句。

---

Resolver and Client – Examples
Resolver in node.js

```js
const resolvers = {
  Query: {
    // Return a single product by ID
    product: (_, { id }) => products.find(p => p.id === id),

    // Return all products
    allProducts: () => products
  },

  Mutation: {
    // Add a new product and return it
    addProduct: (_, { input }) => {
      const newProduct = {
        id: uuidv4(),
        ...input
      };
      products.push(newProduct);
      return newProduct;
    }
  }
};
```


Client Request
```js
const query = `
  query GetProduct($id: ID!) {
    product(id: $id) {
      name
      price
      available
      category
    }
  }
`;

fetch("https://api.yaos.com/graphql", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    query,
    variables: { id: "123" }
  })
})
.then(res => res.json())
.then(data => console.log(data.data.product));

```




# 4 gRPC
Procedure 的意思是 function 
- gRPC (gRPC Remote Procedure Call) is an open-source framework developed by Google and now part of the Cloud Native Computing Foundation (CNCF)
- It enables efficient, strongly typed communication between services
- ==gRPC allows clients to call remote functions on a server as if they were local, using Protocol Buffers (protobuf) as the interface definition language and serialization format==
    - gRPC 允许客户端像调用本地函数一样调用服务器上的远程函数，它使用协议缓冲区（protobuf）作为接口定义语言和序列化格式。
- It works across many platforms and languages and uses HTTP/2 for multiplexed, bidirectional streams

## 4.1 INTERFACE DEFINITION LANGUAGE

```bash
syntax = "proto3";

package yaos.catalog.v1;

service CatalogService {
  rpc GetProduct(GetProductRequest) returns (GetProductResponse);
  rpc CreateProduct(CreateProductRequest) returns (CreateProductResponse);
}

message GetProductRequest { string product_id = 1; }
message GetProductResponse { Product product = 1; }

message CreateProductRequest { Product product = 1; }
message CreateProductResponse { string product_id = 1; }

message Product {
  string name = 1;
  bool available = 2;
  int32 price = 3;     // cents
  Category category = 4;
}

enum Category { ELECTRONICS = 0; FASHION = 1; BOOKS = 2; FOOD = 3; OTHER = 4; }
```

gRPC is based on a contract-first approach using Protocol Buffers (.proto) as its Interface Definition Language (IDL)
A .proto file defines the complete public interface of a gRPC service, i.e.
- Services and their remote procedures
- Request and response message types
- Data structures and enumerations used by the API
- ==In gRPC, the .proto file defines a strict, compile-time service contract that defines both the service interface and the message formats==
Both client and server are generated from the same contract
Message structures are strongly typed and versionable
No manual serialization or parsing logic is required

## 4.2 TEXT-BASED VS BINARY MESSAGES


![](image/Pasted%20image%2020260218145732.png)


gRPC enables efficient, strongly typed RPCs by separating application-level objects from their on-the-wire representation
On the client and server side, developers work with native language objects, such as JavaScript objects
Serialization is the process of transforming a language-specific object (e.g., a JavaScript object) into a byte sequence that can be transmitted over a network via HTTP/2
The structure of these objects is defined centrally in a Protocol Buffer (.proto) definition, which serves as the contract between client and server
The byte sequence is generated according to the Protocol Buffer encoding rules

gRPC 通过将应用层对象与其在线传输的表示形式分离，实现了高效、强类型的远程过程调用。
在客户端和服务器端，开发者操作的是原生语言对象，例如 JavaScript 对象。
序列化，是将特定于语言的对象（如 JavaScript 对象）转换为可通过 HTTP/2 在网络中传输的字节序列的过程。
这些对象的结构集中定义在协议缓冲区（.proto）文件中，该文件充当客户端与服务器之间的契约。
而字节序列则是根据协议缓冲区的编码规则生成的。


## 4.3 WIRE TYPES

![](image/Pasted%20image%2020260218150054.png)

In Protocol Buffers, a wire type defines ==how a field's value is encoded on the wire==
It determines the binary layout, not the semantic type in the .proto file
The wire type is encoded in the lowest three bits of the field tag
It tells the parser how many bytes to read and how to interpret them
Multiple .proto types may share the same wire type
Wire types enable compact encoding while allowing forward compatibility




## 4.4 STRUCTURE OF A BINARY MESSAGE

![](image/Pasted%20image%2020260218150313.png)

![](image/Pasted%20image%2020260218152208.png)
### 4.4.1 Example

![](image/Pasted%20image%2020260218150500.png)

![](image/Pasted%20image%2020260218150520.png)


## 4.5 FROM API SPECIFICATION TO TOOLING
![](image/Pasted%20image%2020260218150543.png)

In gRPC, the .proto file defines a strict, compile-time service contract that defines both the service interface and the message formats
From the .proto definition, server interface, client stub, and validation tools are derived
- The server interface must be implemented by the server and consists of the message signatures
- The client stub is embedded in the consumer so that remote calls appear as local function invocations
    - 它将客户端的本地方法调用转化为网络消息，进行序列化（打包）后发送至服务端，并接收返回结果，使远程调用看起来像本地调用一样
- Validation tools check the correctness at compile time and message structures during serialization at runtime 

Server Interface

```js
const grpc = require("@grpc/grpc-js");
const loader = require("@grpc/proto-loader");

const def = loader.loadSync("catalog.proto");
const proto = grpc.loadPackageDefinition(def).yaos.catalog.v1;

const server = new grpc.Server();

server.addService(proto.CatalogService.service, {
  getProduct: (call, cb) =>
    cb(null, { product: { name: "Laptop", available: true, price: 89900, category: "ELECTRONICS" } }),

  createProduct: (call, cb) =>
    cb(null, { product_id: "123456" }),
});

server.bindAsync("0.0.0.0:50051", grpc.ServerCredentials.createInsecure(), () => {
  server.start();
  console.log("server on 50051");
});
```

Client Stub
```js
const grpc = require("@grpc/grpc-js");
const loader = require("@grpc/proto-loader");

const def = loader.loadSync("catalog.proto");
const proto = grpc.loadPackageDefinition(def).yaos.catalog.v1;

const client = new proto.CatalogService("localhost:50051", grpc.credentials.createInsecure());

client.getProduct({ product_id: "123456" }, (e, r) => console.log(r));
client.createProduct(
  { product: { name: "Laptop", available: true, price: 89900, category: "ELECTRONICS" } },
  (e, r) => console.log(r)
);
```


# 5 WEB SOCKET

## 5.1 HTTP vs WebSockets

![](image/Pasted%20image%2020260218151022.png)

- WebSockets are a communication technology for real-time, interactive APIs
- ==Unlike HTTP, which is based on request-response exchanges initiated by the consumer, a WebSocket connection is long-lived==
- The connection starts with an HTTP handshake and is then upgraded to a persistent WebSocket session
- After this upgrade, communication runs over ==a long-lived TCP connection== (typically secured as wss:// over TLS), so both sides can exchange messages at any time without repeated HTTP request-response cycles
- This makes WebSockets an efficient alternative to polling and long polling: instead of sending many periodic requests (often returning "no updates"), ==the consumer opens one connection and the service can push updates immediately with low latency and minimal overhead== 


## 5.2 WebSocket APIs

![](image/Pasted%20image%2020260218151200.png)


- A WebSocket API is defined very differently from a RESTful API
- ==It is defined as a message protocol that runs over a long-lived, bidirectional connection ==
- Instead of endpoints like /orders/{id} and verbs like GET or POST,  ==the API specifies message types (e.g., Order/SubscribeStatus, Order/StatusChanged, Order/Cancel) and the JSON schemas of their payloads==
- A well-defined WebSocket API also standardizes protocol elements such as 
    - a common message envelope (type, version, messageId, correlationId, timestamp), 
    - error messages and codes, 
    - authentication and authorization rules for opening the connection and executing operations, and 
    - lifecycle behaviors such as subscribe/unsubscribe, heartbeats, reconnect, and  optional resume mechanisms
- While REST inherits most of its meaning from HTTP, ==a WebSocket API must define its semantics explicitly in the message format and in the rules of the conversation between consumer and service   ==



Browser Websocket consumer
```js
const ws = new WebSocket("ws://localhost:8080");

ws.onopen = () => {
  ws.send(JSON.stringify({ type: "Order/GetStatus", orderId: "42" }));
  ws.send(JSON.stringify({ type: "Order/Cancel", orderId: "42" }));
};

ws.onmessage = (e) => console.log("from server:", JSON.parse(e.data));
```

Node.js Websocket Service
```js
const { WebSocketServer } = require("ws");
const wss = new WebSocketServer({ port: 8080 });

// tiny in-memory "order state"
const orders = { "42": { status: "SHIPPED" } };

wss.on("connection", (ws) => {
  ws.on("message", (raw) => {
    const msg = JSON.parse(raw);

    if (msg.type === "Order/GetStatus") {
      ws.send(JSON.stringify({
        type: "Order/Status",
        orderId: msg.orderId,
        status: orders[msg.orderId]?.status ?? "UNKNOWN"
      }));
    }

    if (msg.type === "Order/Cancel") {
      orders[msg.orderId] = { status: "CANCELLED" };
      ws.send(JSON.stringify({ type: "Order/Cancelled", 
                              orderId: msg.orderId }));
    }
  });
});

console.log("ws://localhost:8080");
```