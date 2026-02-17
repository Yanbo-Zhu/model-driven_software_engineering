
# 1 BASIC INFRASTRUCTURE COMPONENTS

## 1.1 SINGLE-SERVER SETUP

A service system combines all the software and hardware components required to provide a digital service to its consumers – whether these are human users, other services, or connected devices
It includes user interfaces, communication mechanisms, business logic, and data storage, all working together over the network
The frontend consists of one or several clients and represents the part of the service system that interacts directly with consumers – it sends requests to the backend and presents the results in a suitable form
The backend executes core functions of the service – it receives requests from the frontend, processes them according to the service's business logic, and interacts with the database for data persistence

In a single-server setup, the frontend and backend both depend on a single physical or virtual machine
The same (web) server may serve static web pages, execute business logic, and store data – a design that is simple to deploy but limited in scalability and resilience
Single-server architectures are typical for small prototypes or early-stage applications
As systems grow, they are usually split into multiple tiers or distributed services

![](image/Pasted%20image%2020260216233236.png)


---

![](image/Pasted%20image%2020260216233334.png)



The client requests the domain name of the service to find out where (in the network topology) the backend is located
The **Domain Name System (DNS)** translates the human-readable domain name into a numerical IP address. This lookup allows the client to establish a network connection without knowing the server's physical location. DNS thus acts as the entry point to the system and provides flexibility: service can move between machines or data centers without changing their public names
Using the IP address returned by the DNS, the client opens a TCP/HTTPS connection to the web server
The backend processes the client's request, executes the necessary business logic, and returns the response


## 1.2 The Protocols


![](image/Pasted%20image%2020260216233750.png)

The **Hypertext Transfer Protocol (HTTP)** is the application-layer protocol that defines how clients and servers exchange information on the web
It follows a request-response model
The client sends an HTTP request to a server's URL (Uniform Resource Locator)
The server processes the request and returns the HTTP response containing a status code, headers, and optionally a message body (e.g., an HTML page, JSON data, or an image)
Each request-response exchange is **stateless**, i.e., every message is independent, and **the server retains no session information unless managed via cookies or tokens**

![](image/Pasted%20image%2020260216233843.png)

---

Traditional web communication relies on the **Transmission Control Protocol (TCP)** for reliable delivery and **Transport Layer Security (TLS)** for encryption
Each connection setup involves two separate handshakes before application data can be exchanged
TCP handshake: Client and server synchronize sequence numbers using SYN, SYN-ACK, and ACK messages to establish a reliable byte stream
TLS handshake: Runs on top of TCP to negotiate an encryption key and authenticate the server
Only after both handshakes complete the client sends its HTTP request




---


QUIC (Quick UDP Internet Connection) replaces both TCP and TLS with a single, encrypted transport protocol running over UDP
It integrates reliability, congestion control, and security at one, drastically reducing handshake latency:
The client sends an Initial packet containing a TLS 1.3 ClientHello
The server replies with an Initial + Handshake packet containing the TLS ServerHello and related handshake messages
After this single exchange, encryption keys are ready and HTTP/3 data can flow immediately

## 1.3 The Data tier

As systems grow, it becomes necessary to separate data management from the application logic
In a three-tier architecture, the backend is divided into the web tier, where the web server handles communication and business logic, and the data tier, where a dedicated database manages persistent information
The web server interacts with the database through CRUD operations (Create, Read, Update, Delete) to store and retrieve data
The separation improves scalability, data integrity, and security, allowing each tier to evolve or scale independently

![](image/Pasted%20image%2020260216234748.png)


---

As service systems grow in complexity, it becomes useful to separate business logic from communication and data management
The application or middle tier is responsible for executing business rules, coordinating workflows, and processing requests received from the web server
The web tier focuses on handling incoming HTTP(S) communication and delegating requests, the application tier performs the actual computations and interacts with the database through CRUD operations

Web server 主要任务：处理 HTTP 协议，接收用户的请求，然后返回静态内容（如 HTML 页面、图片、CSS 文件、JavaScript 文件）或进行请求转发。
Application server 主要任务：提供运行环境，执行服务端的业务逻辑，动态生成内容。它不仅能处理 HTTP，还能处理各种协议（如 RPC），并管理事务、数据库连接池、安全认证、消息服务等企业级功能。

![](image/Pasted%20image%2020260216235032.png)

![](image/Pasted%20image%2020260216235116.png)

# 2 SCALING

Scaling refers to the ability of a service system to handle increasing workload or user demand by adjusting its available computing resources
A scalable architecture can maintain acceptable performance even as traffic, data volume, or the number of requests grows
Scaling up or out means adding capacity to handle higher workload, more users, or larger data volumes
Scaling down means reducing capacity when the workload decreases to control cost and energy consumption
Together, scaling up and down enable elastic scaling, where the system dynamically adjusts to demand in real time


## 2.1 Vertical Scaling  and horizontal Scaling

Vertical Scaling (Scale-UP)
Vertical scaling means enhancing the capacity of a single server by adding more powerful hardware, for example, CPU cores, memory (RAM), storage, or network bandwidth
Once a hardware configuration is reached, further scaling requires migrating to a new system
Vertical scaling is suitable for monolithic or stateful applications where distributing workloads across multiple servers is difficult

Horizontal Scaling (Scale-out)
Horizontal scaling increases capacity by adding more servers or instances that share the load
Requests are distributed among these instances, typically through a load balancer
Allows theoretically unlimited growth and improved fault tolerance – if one node fails, others can continue to serve requests
Introduces complexity: data consistency, state synchronization, and network latency must be carefully managed

Limits of vertical Scaling
![](image/Pasted%20image%2020260216235758.png)


## 2.2 load balancer

A load balancer is a network component that distributes incoming client requests across multiple servers in the backend
Its purpose is to increase availability, performance, and scalability by ensuring that no single server becomes overloaded
The domain resolves via DNS to the public IP address of the load balancer, which then forwards requests to one of several backend servers using private IP addresses within the computing center
From the client's perspective, the system appears as a single endpoint – only the load balancer's public address is visible

![](image/Pasted%20image%2020260216235953.png)

Key Functions
Traffic distribution: Spreads requests evenly across backend servers using algorithms such as round robin, least connections, or weighted load
Health checking: Continuously monitors server availability and routes traffic only to healthy instances
Failover: Automatically redirects traffic if one server fails, improving fault tolerance
Session management: Optionally maintains session persistence so that a user's requests are consistently routed to the same server if required


# 3 Database Scaling

While web and application tiers are typically scaled horizontally through load balancing, database systems require a different approach because they manage stateful data
A common method is the primary-replica (or master-slave) architecture, in which one primary database handles all write/update/delete operations and propagates changes asynchronously or synchronously to one or more replica databases  
Replica databases process read-only queries, reducing the load on the primary and improving performance for read-intensive applications
In this setup, no dedicated load balancer is needed – applications are configured to send writes/updates/deletes to the primary and reads to the replicas

# 4 LOAD BALANCING


## 4.1 Layer-4 Load Balancing 
- Layer-4 Load Balancing operates at the transport layer of the OSI model (Layer 4), where protocols such as TCP and UDP define how data flows between endpoints
- The load balancer does not inspect the application payload (e.g., HTTP headers or URLs), but only routes traffic based on network and transport metadata such as:
    - Source and destination IP address
    - TCP/UDP port numbers
    - Protocol type (e.g. TCP, UDP, QUIC)
- The load balancer acts like a smart router that equally distributes loads among servers providing the same functionality

![](image/Pasted%20image%2020260217000901.png)

---

The Layer-4 load balancer must ensure that TCP connections remain intact and are not broken during forwarding
Server selection occurs during the TCP connection setup phase, which the load balancer recognizes by the SYN flag in the TCP header
Upon receiving such a segment, the load balancer selects an appropriate server and forwards the segment to the chosen server

==The load balancer also maintains a mapping table that associates each TCP connection – identified by its source and destination IP addresses and port numbers – with the selected server==
This step ensures that all subsequent segments of the same connection are directed to the same server, thereby preserving end-to-end connection integrity

Two modes of operation:
NAT mode
DSR mode
NAT 模式（网络地址转换模式）
DSR 模式（直接服务器返回模式）

---
NAT mode 
![](image/Pasted%20image%2020260217001450.png)


In NAT mode (Network Address Translation), the load balancer modifies both the source and destination IP addresses in the IP header
For inbound traffic, the source IP address is replaced with the load balancer's internal (private) IP address, and the destination IP address is replaced with the internal IP address of the selected backend server

For outbound traffic, the source IP address is replaced with the load balancer's public IP address, while the destination IP address is restored to the original client's IP address

Advantages: Backend servers can operate within a private network hidden from clients, and the load balancer can actively monitor their availability through health checks.

Drawbacks: The approach introduces additional processing overhead and latency, and backend servers lose direct visibility of client IP addresses. 


---

(DSR Mode)
![](image/Pasted%20image%2020260217001517.png)


In DSR mode (Direct Server Return), the load balancer forwards incoming packets to the selected server without modifying the IP addresses in the packet headers
The load balancer forwards packets by rewriting only the destination MAC address at Layer 2, ensuring they reach the selected server while keeping the IP header unchanged
The destination IP address remains the shared virtual IP configured on both the load balancer and all backend servers, while the source IP address remains the client's public IP
Servers process the request and send responses directly back to the client using the same VIP as the source IP, bypassing the load balancer on the return path
Advantages: This approach achieves high throughput and low latency, since the load balancer handles only inbound traffic
Disadvantages: It requires more complex network configuration and depends on all servers sharing the same subnet


## 4.2 Layer-7 Load Balancing

![](image/Pasted%20image%2020260217002358.png)



In Layer-7 (or application-level) load balancing, the load balancer acts as a reverse proxy that terminates the client's TCP connection, inspects the application data (such as HTTP headers, URLs, or cookies), and then establishes a new connection to a selected backend server
Operating at the application layer, it can decrypt and re-encrypt HTTPS traffic, modify requests, or route them based on content or path information
As a reverse proxy, it maintains two independent TCP connections – one with the client and one with the backend server

Advantages: Enables intelligent routing, SSL termination, caching, and traffic shaping, while providing full visibility and control over client requests
Drawbacks: Causes higher processing overhead and latency, and the separation of connections reduces end-to-end transparency 


## 4.3 Reverse Proxy

A **reverse proxy** is a network component that sits between clients and backend services, forwarding incoming requests to one or more internal servers and returning their responses to the client
Unlike a forward proxy, which acts on behalf of a client, a reverse proxy acts on behalf of the server side and hides the internal architecture from external consumers
It is typically the entry point of a service architecture or microservice system, often implemented using tools like NGINX, HAProxy, or Envoy

![](image/Pasted%20image%2020260217002841.png)

![](image/Pasted%20image%2020260217002850.png)



## 4.4 ##

![](image/Pasted%20image%2020260217002653.png)

![](image/Pasted%20image%2020260217002709.png)



## 4.5 CLUSTERING

- A cluster is a group of servers or service instances that work together and are managed as a single logical system
- Each node in the cluster runs the same service, allowing the system to handle more requests, provide redundancy, and improve fault tolerance
- Clustering is the technique of connecting multiple nodes so that they operate in coordination as a unified system
- It enables:
    - Scalability – distributing workload across multiple nodes
    - High availability – continuing operation even if one node fails
    - Manageability – centralized configuration, deployment, and monitoring
- Clustering is commonly used for application servers, database systems, container orchestration platforms

---
![](image/Pasted%20image%2020260217003350.png)
The reverse proxy acts as an HTTP router that forwards incoming requests based on URL paths to dedicated Layer-4 load balancers, each responsible for distributing traffic to the nodes of its respective service cluster

Pros
Clear separation of concerns: reverse proxy handles routing, Layer-4 balancer handles load distribution
Easier to scale Layer-4 and Layer-7 independently
Suitable for high-traffic, multi-service backends with multiple reverse proxy instances

Cons
Requires additional infrastructure
Increased network hops –> slightly higher latency
More complex configuration and failure handling

---


The reverse proxy performs both HTTP routing and load balancing in one step
Requests are distributed directly from the reverse proxy to backend service clusters with intermediate Layer-4 balancers

Pros
Simplified architecture – fewer components, easier to manage and to deploy
Lower latency due to fewer network hops
Ideal for medium-scale microservice systems where simplicity and agility matter

Cons
Tighter coupling between routing and load balancing logic, making horizontal scaling of proxies more challenging
Additional configuration overhead in case of changes in the cluster configuration if reverse proxy tier has multiple nodes

![](image/Pasted%20image%2020260217003437.png)

![](image/Pasted%20image%2020260217003758.png)


# 5 CACHING


In modern backend systems, caching is used to reduce latency and offload the database by storing frequently accessed data in a fast, in-memory layer called cache tier
This tier sits between the application logic and the persistent data store and serves as a temporary buffer for recently or frequently requested information
When an application reads data, it first queries the cache tier
If the requested data is present (**cache hit**), it is returned immediately
If the cache does not contain the data (**cache miss**), the application retrieves it from the database and populates the cache for subsequent requests
This strategy is known as **read-through cache (or cache-aside pattern),** where the cache is read first and automatically refreshed on misses

![](image/Pasted%20image%2020260217003828.png)

![](image/Pasted%20image%2020260217004145.png)


# 6 CONTENT DELIVERY NETWORKS

A Content Delivery Network (CDN) is a distributed infrastructure that improves the performance, availability, and scalability of web content delivery
Instead of serving all requests from a single backend, ==CDNs replicate static (and sometimes dynamic) resources across multiple edge nodes deployed worldwide.==
Each node caches frequently accessed content (similar to a read-through cache) and serves it directly to nearby clients, reducing latency and offloading backend traffic
When a client requests content, the request is automatically routed to the nearest CDN node, which is typically determined through anycast, geocast, or a combination of both

![](image/Pasted%20image%2020260217004227.png)


![](image/Pasted%20image%2020260217010238.png)


---

## 6.1 The Domain Name System (DNS) is the Internet's directory service

It translates human-readable domain names into machine readable IP addresses needed for network communication
The client asks the local name server to resolve a domain
If the name is not cached, the local server performs an reverse lookup:
It starts with a root name server, which replies with a referral to the responsible top-level domain (TLD) server
The TLD server, in turn, sends a referral to the authoritative name server for the specific subdomain
The authoritative server finally returns the A or AAAA record containing the IP address
The result is cached locally for faster future lookups
Alternatively, the client can run an iterative lookup

![](image/Pasted%20image%2020260217005007.png)

![](image/Pasted%20image%2020260217004958.png)


## 6.2 GEODNS


GeoDNS (Geographic Domain Name System) enables location-aware resolution of domain names, allowing clients to connect to the nearest or most suitable service endpoint
==When a user accesses cdn.yaos.shop, the authoritative name server does not return a single global IP address but instead selects one based on the client's geographic location==
To achieve this, GeoDNS relies on a GeoIP database, which maps IP address ranges to approximate regions or countries
During the DNS lookup, the server determines the location of the requesting client (usually inferred from the resolver's IP address) and returns the corresponding CDN node.

![](image/Pasted%20image%2020260217005955.png)

## 6.3 Unicast, Broadcast, Multicast, and Anycast

![](image/Pasted%20image%2020260217010030.png)

Unicast – One-to-one
The sender transmits data to a single, uniquely addressed receiver
Default mode for most Internet communication (e.g., web requests, API calls)

Broadcast – One-to-all
The sender transmits data to all hosts within the same network segment
Useful for local discovery protocols like the Dynamic Host Configuration Protocol (DHCP) or Address Resolution Protocol (ARP)

Multicast – One-to-many (selected group)
The sender delivers data to a group of interested receivers that have joined a multicast group
Efficient for applications like video streaming, conferencing, or live data feeds

Anycast – One-to-one-of-many
==Multiple receivers share the same IP address, and packets sent to that address are delivered to exactly one of them according to current network policies and path information==
Commonly used for CDNs, DNS root servers, and distributed APIs


### 6.3.1 Combining GeoDNS and Anycast


![](image/Pasted%20image%2020260217011914.png)

In CDNs, Anycast is used to map a single global IP address to multiple CDN nodes distributed across different regions
All these nodes advertise the same IP address (e.g., 1.2.3.4) via the Border Gateway Protocol (BGP)
The Internet's routing system then decides based on network topology, path policies, and routing metrics which node will receive a given client's packets
This approach allows CDNs to
- present a single, uniform service address (such as cdn.yaos.shop) to all users,
- distribute traffic across several operationally equivalent sites, and
- automatically reroute clients when a node becomes unavailable 


- An edge server is the individual machine or virtual instance inside a PoP that actually handles user requests
- It caches web content, performs TLS termination, enforces caching and security policies, and communicates with upstream origins if content is missing
- A Point of Presence (PoP) is a physical data center, sometimes called CDN site or CDN node, where edge servers store and deliver cached content
- Each PoP is connected to regional Internet backbones and local ISPs to ensure fast access for users nearby
- A region is a broad geographical or network area grouping several PoPs
- Regions help structure DNS-based routing (GeoDNS) by mapping users to the closest regional cluster based on their location from a GeoIP database

- Modern CDNs use a hybrid approach that combines GeoDNS and Anycast because each mechanism alone solves only a part of the delivery challenge
- Anycast ensures that requests are automatically routed to one operational PoP that shares a global IP address
- This provides fast routing, automatic failover, and low latency, but gives little control to the CDN operator – routing decisions depend entirely on the network topology, not on business rules, legal boundaries, or server load
- GeoDNS allows the CDN to make policy-based decisions
- ==When a user resolves a hostname such as cdn.yaos.shop, the authoritative DNS server determines the user's approximate location using a GeoIP database and returns the regional IP address that reflects the CDN's own mapping strategy==
    - Then - Anycast ensures that requests are automatically routed to one operational PoP that shares a global IP address (regional IP address ) 
    - edge server ( in that pop )s tore and deleiver cached content 
- GeoDNS decisions are made at DNS resolution time and adapt only slowly to network changes 


## 6.4 Example: Netflix

![](image/Pasted%20image%2020260217103313.png)


- Netflix's backend services — including user authentication, catalog metadata, recommendations, and the master copies of all video assets — run in the Netflix cloud infrastructure on AWS.
- Origin servers in this cloud act as the source of truth for all content, generating video manifests (lists of segment URLs, bitrates, and encodings) and supplying the master video files distributed to the CDN.
- **Open Connect Appliances (OCAs)** form the content-delivery layer, caching popular titles close to users, and are fed by the origin servers through scheduled content updates.
- OCAs are deployed at Internet Exchange Points (IXPs), where multiple Internet Service Providers (ISPs) interconnect, and in many cases, embedded directly within ISP networks.
- IX-site OCAs serve as regional caches for several ISPs and act as the upstream source for embedded OCAs.
- Embedded ISP OCAs reside inside access networks and deliver video streams directly to subscribers, minimizing latency and backbone traffic
# 7 Multi-regional Service DEployment


![](image/Pasted%20image%2020260217103720.png)


# 8 MESSAGE QUEUING


![](image/Pasted%20image%2020260217103748.png)

Message queues enable **asynchronous** communication between services by buffering messages between consumers and producers
A producer publishes messages to the queue, where they remain stored until a consumer retrieves and processes them
Decoupling increases resilience and scalability: producers do not wait for immediate response, and consumers can process messages at their own pace
If a consumer fails, messages persist in the queue and are delivered once it becomes available again
Common technologies include RabbitMQ, Kafka, ActiveMQ, and AWS SQS
Example: when a customer places an order, an event is queued for inventory updates, payment confirmation, and email notification

# 9 CONTROL PLANE


Logging, Monitoring, Metrics, and Automation

![](image/Pasted%20image%2020260217104255.png)


The control plane provides cross-cutting capabilities that ensure transparency, stability, and automation across the entire service infrastructure
It operates above the backend tier and collects operational data from all components
Logging records events and system activities in structured form. It enables tracing requests across microservices, detecting errors, and performing audits
Monitoring continuously observes system health by collecting status signals and alerting operators when thresholds are exceeded
Metrics quantify performance indicators such as latency, throughput, and resource usage. They are used for dashboards, trend analysis, and capacity planning
Automation executes operational tasks such as deployments, scaling, backups, or recovery without human intervention




# 10 AI INFRASTRUCTURES

## 10.1 CPU VS GPU VS TPU

CPUs
- Central Processing Units (CPUs) are general purpose processors used in all computers and servers
- Optimized for sequential, general purpose tasks such as executing program logic, managing I/O, and running operating systems
- CPUs have a few powerful cores with large caches and flexible control units, which is applicable to workloads but **limited in parallel throughput**
- In AI contexts, CPUs orchestrate data movement, pre- and post-processing, and light inference at the edge
- CPUs operate primarily on **scalars**, i.e., single values processed sequentially by a few cores
- Each red square represents a single arithmetic unit operating on one number at a time
- High flexibility, but limited **parallelism** 

![](image/Pasted%20image%2020260217104646.png)

---

GPUs
- Graphics Processing Units (GPUs) were originally built for rendering images and evolved into massively parallel compute engines
- They consist of thousands of smaller cores that execute the same instruction on many data elements simultaneously
- This makes them ideal for training and inference in neural networks, where millions of matrix multiplications dominate
- In AI infrastructures, GPUs enable large-scale model training and fast parallel inference in data centers and high-performance clusters
- **GPUs operate on vectors, i.e., one-dimensional arrays of numbers processed in parallel**
- Many red squares acting simultaneously along a row each performing the same instruction on different data (Single Instruction, Multiple Data)
- **Massive parallelism** for matrix and vector math


![](image/Pasted%20image%2020260217104715.png)

---

TPUs
- Tensor Processing Units (TPUs) are custom-built by Google for tensor operations used in deep learning
- They replace general flexibility with domain-specific efficiency and are optimized for dense matrix multiplies and low-precision arithmetic
- They deliver higher throughput and lower energy use per AI operation compared to GPUs but support fewer model types
- TPUs are commonly deployed in hyperscale clouds or integrated into on-device SoC for inference
- **TPUs directly operate on tensors,** i.e., multidimensional arrays
- Matrix blocks are processed as a unit, i.e., instead of looping through scalar or vector operations, **TPUs perform matrix multiplications in one cycle**
- Specialized silicon for deep learning – extremely high throughput, low flexibility

![](image/Pasted%20image%2020260217105203.png)


## 10.2 The Inference Server


![](image/Pasted%20image%2020260217105401.png)


- An LLM Inference Server executes a trained large language model (LLM) to generate responses from incoming prompts
- It is exposed through a standard API (REST, gRPC, WebSocket) and backed by specialized compute (GPU/TPU) and orchestration layers
- Each inference follows a highly optimized sequence
    - Prefill phase: The user's prompt is tokenized, embedded, and fed into the transformer layers to compute internal key-value states
    - Decode phase: Tokens are generated iteratively, one after another, re-using cached activations for efficiency
    - Post-processing: Decoded tokens are converted back to text, filtered and returned to the client
- The pipeline runs entirely on GPU or TPU clusters optimized for matrix multiplication and memory bandwidth

## 10.3 Forms of Parallelism for AI Inference



![](image/Pasted%20image%2020260217105552.png)

Data Parallelism
In data parallelism, each GPU or TPU holds **a full copy of the model weights**
**Different input batches are processed independently** and in parallel on each accelerator
During inference, **no synchronization** between accelerators is required – results are simply merged afterward
Best for serving many requests at once (throughput scaling)
Each accelerator must fit the entire model into its memory


Tensor Parallelism
When **the model is too large for one accelerator**, tensors (e.g., weight matrices) are shared across accelerators in tensor parallelism
**Each GPU computes a portion of the same matrix multiplication at every layer** – e.g. one handles columns 0-2047, another 2048-4095
Intermediate results are then aggregated via high-speed interconnects
Enables very large models by distributing layer computations
Required frequent communication between GPUs, sensitive to interconnect bandwidth and latency

Pipeline Parallelism
In pipeline parallelism, **the model's layers are divided into stages, each running on a different GPU**
**Tokens (or micro-batches) flow through the pipeline while one accelerator processes the prefill of batch A, another decodes batch B**
This overlapping keeps all accelerators busy and minimizes idle times
Scales models that exceed a single accelerator's memory and improves utilization
Introduces pipeline latency, and balancing that stage is complex


## 10.4 Scaling Architectures for LLM Inference

![](image/Pasted%20image%2020260217110438.png)


Single Node – One GPU/TPU
The simplest setup: one physical or virtual machine hosts both the model and the runtime
Al computation – tokenization, prefill, and decode – runs on a single accelerator, such as GPU or TPU
An accelerator is specialized hardware designed to speed up mathematical operations, particularly matrix multiplications, that dominate neural network workloads
Ideal for small or quantized models, where model weights are stored using lower numerical precision (e.g., 8-bit instead of 16- or 32-bit) 


Single Node – Multiple GPUs/TPUs
A single server node contains several accelerators – GPUs or TPUs – connected inside the same machine via high-speed connectors
**These internal links enable fast data exchange between accelerators and shared access to model parameters, enabling the node to act as a tightly coupled compute unit**
Within this node, parallelism techniques are applied to scale inference across accelerators efficiently
Inside one node, data, tensor, and pipeline parallelism works hand-in-hand, combining throughput, memory capacity, and utilization efficiency 


Multi-Node Cluster
A cluster consists of several server nodes, each equipped with one or more GPUs or TPUs, connected through a high-bandwidth data center network
Together, the nodes form a single logical inference system capable of running models that exceed the capacity of any single machine
Within this cluster, parallelism extends beyond node boundaries to scale throughput, memory, and performance
Across nodes, data parallelism scales throughput easily, pipeline parallelism scales depth, and tensor parallelism scales model size – at rising coordination costs 



Cross-Site or Geo-Distributed Deployment
In a geo-distributed setup, inference capacity is deployed across multiple data centers or cloud regions, each hosting one or more local clusters
A global load balancer or Anycast routing mechanism directs user requests to the nearest or least-loaded site, minimizing latency and ensuring high availability
Data parallelism: Each site maintains a replica of the model and independently handles requests from its geographic region
This minimizes cross-site data transfer, satisfies data sovereignty constraints, and allows near-linear scaling of throughput with global demand
Synchronization between sites occurs only at the model or configuration level, e.g., when updating weights or deploying a new model version
Pipeline and tensor parallelism: Stages or layers of a model could be distributed across sites, but the high latency and network variability of intercontinental links make this impractical

## 10.5 Overview of LLM models and Hardware Planning Levels*


![](image/Pasted%20image%2020260217110053.png)


## 10.6 Retrieval-Augmented Generation (RAG)

- Retrieval-Augmented Generation (RAG) combines information retrieval with text generation to produce context-aware and fact-grounded responses
- Instead of relying solely on a model's internal parameters, a RAG pipeline retrieves relevant documents or data from external sources (e.g., databases, APIs, files,...) and injects them into the model's prompt at runtime
- The **knowledge store** serves as the source of truth containing the original content and provides up-to-date information that can be indexed and referenced during generation
- The **vector database** stores embeddings, i.e., high-dimensional vector representations of the knowledge content and acts as a semantic index enabling efficient similarity search
- The **retriever** converts the user query into an embedding and returns the matching text snippets
- The **orchestrator** combines the retrieved context with the original query 

![](image/Pasted%20image%2020260217110323.png)
