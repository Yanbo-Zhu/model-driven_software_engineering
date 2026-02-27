
# 1 MONOLITHS VS MICROSERVICES


A monolith is an application that is built and deployed as one single, indivisible unit with the following key characteristics
- Single codebase: All functionality (e.g., product catalog, shopping cart, order management, payment) is contained in one large codebase
- Single deployment: The entire system is built, tested, and deployed together. You cannot scale or update parts of it independently
- Shared resources: Typically, it uses one central database that all components access
- Tight coupling: Modules are interconnected, so a change in one part can easily affect others
- Scaling: Scaling is usually done by replicating the whole application (running multiple identical instances of the monolith)

Patchwork Monolith
- A patchwork monolith grows organically **without clear boundaries** between business areas
- Modules such as Governance, Logistics, Products, and Customers overlap in code and data access
- Components share classes and database tables, which creates tight coupling and hidden dependencies
- Small changes can have unpredictable side effects across the entire application
- Testing and refactoring are difficult, and scaling individual parts is not possible
- The architecture lacks a clear separation of concerns and becomes increasingly brittle over time

modular monolith 
- A modular monolith keeps a single deployment unit but divides the system into well-defined modules with explicit interfaces
- **Each module encapsulates its own business logic** and data access layer and interacts with others only through internal APIs
- This structure enforces clear ownership, improves maintainability, and enables parallel development by different teams
- The modular monolith provides a neutral evolutionary step toward microservices, without the operational overhead of distribution 


![](image/Pasted%20image%2020260217213115.png)




## 1.1 Service-Oriented Architectures

![](image/Pasted%20image%2020260217213247.png)

A Service-oriented Architecture (SOA) is an architectural style where an application **is structured as a set of coarse-grained, reusable services that communicate through a common integration layer**
- Services as building blocks: Each service often represents a business domain (e.g., Governance,...)
- Common communication layer: Often realized through an Enterprise Service Bus (ESB) or as a middleware that handles messaging, routing, and transportation
- Loose coupling: Services expose standardized interfaces (often SOAP, later REST), allowing different technologies to interoperate
- Reusability: Services are designed so they can be reused across multiple applications or processes
- Scaling: Scaling is possible at the service level (but often less fine-grained than microservices)

An **Enterprise Service Bus (ESB)** is a middleware layer in SOA that enables communication, coordination, and integration between services
- Message hub: Acts as a central backbone where services send and receive messages
- Protocol and format transformation: Converts between different data formats and communication protocols (e.g., SOAP <-> REST, XML <-> JSON)
- Routing and orchestration: Determines which service should process a message and can coordinate multiple services in workflows
- Centralized control: Provides features such as logging, monitoring, security, and error handling across all services
- Potential bottleneck: While it simplifies integration, heavy reliance on the ESB can reduce agility and become a single point of failure


## 1.2 Microservices

![](image/Pasted%20image%2020260217213549.png)

A microservice architecture is an approach where an application is built as a collection of small, independently deployable services, each responsible for a fine-grained business capability
- Single responsibility: Each microservice focuses on one small function (e.g., catalog, pricing, cart,...)
- Independent deployment: Services can be updated, deployed, and scaled without affecting others
- Polyglot freedom: Different services can use different programming languages, frameworks, or databases
- Decentralized data management: Each service typically owns its own database to avoid tight coupling
- Lightweight communication: Services communicate via lightweight protocols (often REST/JSON, gRPC, or messaging)

## 1.3 Monolith VS SOA VS MICROSERVICES

![](image/Pasted%20image%2020260217213801.png)

Granularity	Coarse-grained services that often represent complete business domains (e.g., "Order Management")	Fine-grained services, each focusing on a single business capability (e.g., "Order Validation," "Inventory Update")
Communication	Typically enterprise-grade middleware like an Enterprise Service Bus (ESB) for orchestration and message routing	Lightweight protocols such as HTTP/REST, gRPC, or message queues; often avoids a central bus
Integration	Centralized integration via the ESB — enforcing policies, transformations, and security	Decentralized integration — services communicate directly or through an API Gateway or service mesh
Data Management	Shared databases across services are common, emphasizing consistency	Each microservice usually owns its own database, ensuring autonomy and independent scaling.
Deployment	Large, coarse components; changes require coordinated deployments	Fully independent deployment and scaling; continuous delivery is standard
Governance	Heavy governance, often enterprise-wide; focuses on standardization and reusability	Lightweight governance; emphasizes autonomy and agility per service team
Technology Stack	Often homogeneous — enterprise platforms (e.g., SOAP, WSDL, Java EE)	Polyglot — each service can use the most suitable language and framework
Scaling	Vertical or centralized scaling of shared infrastructure	Horizontal scaling per service; enabled by containers, orchestration (Kubernetes), and cloud-native tooling
Typical Use Case	Large enterprises integrating legacy and heterogeneous systems	Cloud-native applications demanding agility, resilience, and continuous evolution
Current relevance	Still relevant in enterprise integration scenarios, especially where legacy systems, ERP suites, or heterogeneous platforms must interoperate (e.g., banking, insurance, government).	The dominant paradigm for new cloud-native backends. Most modern platforms (Netflix, Amazon, etc.) implement SOA principles through microservices — decentralized, containerized, and managed via DevOps pipelines.
Trend	Stable or declining — evolving toward hybrid architectures that combine legacy SOA with newer APIs or microservices	Growing and evolving — forming the basis of serverless, event-driven, and agent-based architectures


---

![](image/Pasted%20image%2020260217214050.png)



Main benefits	
Simple to develop & deploy at the start (one codebase/runtime/db).
Easy local debugging and end-to-end changes.
Strong consistency is straightforward with a single database
Independent deploy & scale per service → flexibility and resilience.
Polyglot freedom (tech per service).
Blast radius of failures is smaller; teams can move faster
Main drawbacks	Any change triggers redeploy of the whole app; partial failures affect the whole system; limited agility & scale	
Higher operational complexity: service discovery, inter-service comms, monitoring, and data consistency are harder.
Requires platform pieces (reverse proxy/API gateway, load balancing, observability)
Scaling	Mostly vertical; horizontal scaling is coarse-grained (scale everything together)	Fine-grained horizontal scaling per service (LBs, clusters)
Data	Often a single shared database; simple transactions	"Database per service" encourages autonomy but complicates cross-service transactions
When to use	Small team, well-understood domain, early product phases, or tight consistency needs	Large/fast-evolving systems, heterogeneous workloads, need for independent scaling and rapid delivery

---

Scalability Dimensions
![](image/Pasted%20image%2020260217214225.png)

Development & DEployment

![](image/Pasted%20image%2020260217214240.png)


## 1.4 Case Study: Shopify

![](image/Pasted%20image%2020260217214328.png)

- Shopify uses a modular monolith, internally partitioned into domain-based modules such as Catalog, Basket, and Identity
- Each module has clear API boundaries, limited dependencies, and well-defined ownership, yet all live within the same runtime and deployment pipeline
- The company started with a classical Ruby on Rails monolith
- As it grew to thousands of developers and millions of merchants, cross-coupling and slows builds became critical pain points
- Moving to microservices was considered too costly and complex for their scale
- Instead, they evolved the monolith into a modular monolith with strong internal boundaries
- This allowed hundreds of teams to work in parallel while keeping a single, unified deployment pipeline

![](image/Pasted%20image%2020260217214839.png)

## 1.5 Case Study: Netflix

![](image/Pasted%20image%2020260217214855.png)

Netflix originally operated a monolithic architecture for its rental DVD business, and later for its streaming business
Over time, the growth in subscribers, devices, global scale, and developer teams created bottlenecks
To scale globally, support many client devices, enable rapid deployment of features, and ensure resilience, Netflix moved to a microservice architecture, mainly residing in AWS compute centers
A single client request (say "play movie") may traverse multiple microservices (catalogue, user profile, streaming authorization, device compatibility, recommendation engine) 
They emphasize bounded contexts, loose coupling, and service-data autonomy: each service ideally has its own datastore and runtime

![](image/Pasted%20image%2020260217215051.png)



# 2 SERVICE ARCHITECTURES

## 2.1 Inside the service



![](image/Pasted%20image%2020260217215107.png)

At first glance, both architectures appear structurally similar: each separates functionality into three tiers: an interface tier, a logic tier, and a data tier
This tiered decomposition reflects the Model-View-Controller (MVC) principle, where presentation corresponds to the "view", the logic layer to the "controller", and the data layer to the "model"

monolithic architecture
- In a monolithic architecture, the three tiers – **presentation, logic, data** – coexist within a single, unified deployment
- The presentation tier provides the external interface and frontend to clients and hides the system's internal logic and data
- All components share the same runtime and database, which simplifies communication but tightly couples the entire system
- logic： In the context of your definition, logic refers specifically to the business logic (also known as the application tier or domain logic). It is the "brain" of the application—the core set of rules that dictates how data is created, stored, and modified based on specific business requirements.

microservice architecture
- In a microservice architecture, the internal structure of the service follows a comparable pattern, but the presentation tier is replaced by the API tier, defining the service's external contract and hiding its internal implementation from other services
- A microservice can act as a provider (for upstream consumers) and as a consumer (of downstream dependencies), forming part of a larger distributed system
- This modularization preserves the clarity of the layered design inside each service while enabling independent evolution, deployment, and scaling across the system


## 2.2 Around the service



![](image/Pasted%20image%2020260217215853.png)


Just as every service has an internal architecture, it also requires an external architecture that defines the environment in which it operates, especially in a microservice setting, where each service is an independent, deployable unit
This environment can be understood as four conceptual layers:
- Client – User interfaces, devices, or third-party systems that access the service to invoke its functionality
- Boundary – The entry, exit, or internal points that handle cross-cutting concerns such as authentication, authorization, routing, or logging, and expose the service's API to other actors
- Service – The functional core that implements business logic, orchestration, and inter-service communication
- Platform – The infrastructure and deployment layer providing compute, storage, networking, and automation that the service runs on
Together, these layers describe the operational ecosystem of a microservice 


# 3 THE PLATFORM LAYER


![](image/Pasted%20image%2020260217220235.png)

A microservice is supported by infrastructure, i.e., a deployment target like AWS and Heroku where services are run, including infrastructure primitives, such as load balancers and virtual machines
They support secure operation, such as network controls, secret management, and application hardening
Communication channels and service discovery support service interaction
Observability tools collect and correlate data from services and underlying infrastructure
Deployment pipelines manage upgrades (rollbacks) of this stack


Application hardening
Application hardening means securing an application by reducing its attack surface, i.e., removing or disabling everything that an attacker could potentially exploit
It includes:
Removing unnecessary features, services, or APIs
Configuring security settings properly, such as enforcing TLS, secure cookies, CORS rules, and strict HTTP headers
Using strong authentication and authorization mechanisms
Protecting secrets (API keys, tokens, credentials) and ensuring secure storage
Keeping dependencies up to dat


# 4 THE SERVICE LAYER

![](image/Pasted%20image%2020260217220745.png)

**Business services** implement business capabilities – they realize the company's value propositions and reflect domain-specific functionality:
- Contain core business logic (e.g., Order, Inventory, Payment)
- Operate on domain data and enforce business rules
- Directly contribute to user-facing features or processes
- Evolve with products, markets, and business models

Utility services implement technical capabilities that are application-related but domain independent:
- Provide reusable supporting functions used by several business services
- Encapsulate specialized technical know-how or algorithms
- Promote consistency, reuse, and maintainability across domains

Both types of services are typically stateless, focusing on computation or transformation rather than keeping the state between different requests


## 4.1 Aggregator vs Coordinator

![](image/Pasted%20image%2020260217220956.png)

Application services build on top of business and utility services
- They combine, coordinate, or mediate their functionality to realize complete end-to-end processes or composite views for clients
- Two complementary patterns describe how this integration is achieved – aggregation and orchestration


Aggregation
Collects data from multiple underlying services into a single, unified response
Typical for query scenarios such as ProductView that aggregates product details, pricing, recommendations, and stock information
Involves minimal control logic – services are queried independently, and results are composed
Often stateless, focusing on read-only data retrieval



Orchestrator
Coordinates multiple service invocations to execute a sequence of dependent actions
Typical for command scenarios such as Checkout, which invokes Cart, Order, Payment, and Shipping services in a controlled order
Requires process logic, transaction handling, and error recovery
Frequently stateful, maintaining the context of the ongoing process or transaction during a session

## 4.2 Chains of services


- Services are connected in chains, where each service depends on others to fulfill a user request or business process
- Upstream and downstream dependencies
    - A failure in a downstream service immediately affects all upstream consumers.
    - The longer the service chain, the higher the probability of disruption
- Business criticality
    - Some services are business critical – if ProductView fails, customers cannot browse or purchase products, directly impacting revenue.
    - Others, like Analytics are less critical; their temporary unavailability affects insights, not operations
- Reliability propagation
    - The overall system reliability depends on the weakest link in the chain.
    - Designing for resilience – redundancy, caching, fallbacks, and graceful degradation – is essential to maintain service continuity  

## 4.3 Stateful Services 

![](image/Pasted%20image%2020260217223820.png)



## 4.4 Stateless Services 

Stateless Services – State maintained in Database
![](image/Pasted%20image%2020260217223831.png)

Stateless Services – State maintained by Client
![](image/Pasted%20image%2020260217223932.png)

![](image/Pasted%20image%2020260217224019.png)



# 5 BOUNDARIES

Boundary – The entry, exit, or internal points that handle cross-cutting concerns such as authentication, authorization, routing, or logging, and expose the service's API to other actors


![](image/Pasted%20image%2020260217224045.png)



Modern service systems expose functionality across multiple administrative domains – to clients, to internal peers, and to external providers
To control these interactions, they must establish explicit boundaries, each responsible for enforcing policies and managing cross-cutting concerns such as security, logging, or monitoring
Boundaries are implemented by dedicated **proxy** components, which realize different architectural patterns – for example, API or consumer-driven gateways – depending on their role and communication direction
This design clearly **separates business logic** from non-functional responsibilities and increases security, scalability, and maintainability

---

Inbound boundary
Controls all inbound traffic from external clients into the backend
Typically implemented by a **reverse proxy** applying patterns like API gateway or load balancing
It terminates connections, authenticates clients, and routes requests to internal services

Internal boundary
Mediates service-to-service communication inside the backend
Often realized by an **internal proxy implementing cross-cutting** concerns such as observability, retries, and load balancing
It ensures stable and **policy-compliant interaction** between independently evolving services

Outbound boundary
Regulates all outbound traffic from the backend to external systems
Implemented by a **forward proxy** enforcing organizational policies, security checks, and compliance for downstream integrations
It prevents uncontrolled data flows and maintains isolation from external dependencies

## 5.1 The Backends-for-Frontends Pattern

The Backends for Frontends (BFF) pattern introduces dedicated backend APIs for different client types, for example, the public website, the mobile app, the admin console, or third party integrations, instead of exposing a single, one-size-fits-all backend
Each BFF serves as an intermediary between its client and the shared business service, providing a client-optimized interface in terms of data aggregation, payload structure, performance, and security 
**BFF ensures that each frontend receives exactly what it needs, without compromising the modularity of the overall backend**

![](image/Pasted%20image%2020260217225112.png)

![](image/Pasted%20image%2020260217225127.png)


---

API Gateway
![](image/Pasted%20image%2020260217225153.png)

An API Gateway provides an entry point for client requests to a microservice system
It intercepts each request before it reaches the backend service and applies a sequence of cross-cutting concerns such as logging, authentication, and transformation
It then routes the transformed request to the appropriate service, potentially using a different protocol
The same sequence, in reverse, is applied to the outgoing response to ensure consistent processing, auditing, and adaptation across the entire communication path
The gateway functions are non-functional in nature – they apply system-wide and are not tied to any specific business service

## 5.2 Consumer-driven Gateway pattern


A consumer-driven gateway is an evolution of the API Gateway and BFF patterns
While traditional gateways define what data and operations clients can access, a consumer-driven gateway lets the client itself shape the response according to its own needs
Instead of providing fixed endpoints such as /products, /products/:id, and /reviews, t**he gateway exposes a flexible query interface – most commonly implemented using GraphQL**
Clients specify exactly which data fields they need and how deeply related resources should be traversed
The gateway then orchestrates the required backend calls (to REST, gRPC, or database services) and composes a single, optimized response 

![](image/Pasted%20image%2020260217225359.png)

![](image/Pasted%20image%2020260217225410.png)



# 6 CLIENTS

![](image/Pasted%20image%2020260217225427.png)

frontend monolith 
A frontend monolith is given if one app or Single Page Application (SPA) owns routing, state, user interface, and all features in a single codebase/build/deploy unit

Pros
Simple, local development, one toolchain, one design system
Easy cross-feature refactors and share state
Best out-of-the-box performance (no duplicated runtimes)

Cons
Slows down with team/feature scale
One broken feature blocks all
Harder to experiment with divergent tech stacks


---
microfrontend
A microfrontend is given if multiple independent built and deployed slices compose into one user experiences (by route, layout slot, or widget)
Each slice is owned by a team

Pros
- Team autonomy: isolated repos, continuous integration, and deploy cadence
- Rollback one slice without touching others
- Tech evolution per slice (React/Vue/Web Components side-by-side if needed)

Cons
- Composition complexity
- Performance tax: Multiple bundles, potential duplicate frameworks
- Shared UX governance required (design system, accessibility, analytics)

# 7 MICROSERVICE SCOPING

## 7.1 domain

A domain is the coherent== **area of business capability** ==in which a system creates values, including its actors, rules, processes, and vocabulary
**It is defined by a stable purpose** (what it is to achieve), by a consistent ubiquitous language, and by ownership of certain business facts (its authoritative records)

A subdomain is a smaller, tightly cohesive part of a domain that has its own purpose and internal consistency but still depends on the parent domain's language and goals
Subdomains exist to keep models small and focused, not to mirror technical layers
If the parent answers "What capability do we provide?", a subdomain answers "Which part of that capability do we provide"? 

![](image/Pasted%20image%2020260217225820.png)


## 7.2 Criteria for defining Domains


![](image/Pasted%20image%2020260217231005.png)

Cohesion of Purpose
Meaning: A domain exists to achieve one clear business outcome and shares a stable vocabulary.
Apply: If you can write a single mission sentence that all its entities/behaviors support, it's cohesive.
YAOS example: Order Processing -> "Turn a customer cart into a fulfilled order" (terms: Order, OrderLine, Status, ShipmentId).
Pitfall: Mixing unrelated outcomes (e.g., putting Recommendations into Order Processing because both touch products). 

Volatility
Meaning: Group things that change for the same reason and at the same rhythm; split things that change for different reasons.
Apply: Compare change triggers and release cadence. If feature requests/bugs arrive from different stakeholders at different times, split.
YAOS example: Pricing & Promotions changes with marketing campaigns; Inventory changes with warehouse operations. Keep them separate.
Pitfall: Binding your domain model directly to a payment-provider SDK. Each provider SDK change triggers a service release, even though your own business rules didn't change

Authority
Meaning: The single source of truth for a record type defines a domain boundary.
Apply: Ask "Who publishes the canonical record?" Others subscribe/replicate, but do not own.
YAOS example: Customer Accounts owns Customer Profiles and Addresses; Catalog owns Product and SKU; Inventory owns StockLevel; Orders owns Order and OrderLine.
Pitfall: Dual-write of the same entity across services (e.g., both Catalog and Recommendations writing Product attributes. 

Policy
Meaning: Different legal or policy regimes imply boundaries (GDPR, tax, payment rules).
Apply: Whenever consent handling, retention, or audit differ, isolate into its own context.
YAOS example: Customer Accounts handles GDPR consent, data deletion, and audit logs; Billing handles VAT rules per jurisdiction.
Pitfall: Exposing raw Personally Identifiable Information (PII) – any data that can identify a specific person, directly or indirectly – to services that don't enforce the same policies. Use policy-enforced boundaries (filtered views, minimized datasets) instead  


Integration
Meaning: Put a boundary around capabilities you expect many others to call or that you may want to swap out.
Apply: Count external consumers; if high, make a stable, well-owned API. If vendor lock-in is a risk, wrap the vendor behind your domain API.
YAOS example: Payments sits behind a Payment API so you can switch Payment Service Providers without touching Order Processing.
Pitfall: Letting upstreams call vendor SDK directly -> replacement becomes a big-bang refactor. 

Consistency
Meaning: Keep code together when it must enforce strong invariants atomically; split where eventual consistency is acceptable.
Apply: List invariants that must never be observed broken. If enforcing them needs ACID across components, collocate within one domain.
YAOS example: Strong consistency is needed for status=Paid, which is valid only if a Payment Authorization Id is present.
YAOS example: Eventual consistency is sufficient after an order is paid and the inventory needs to be updated.
Pitfall: Distributed transactions across many domains -> fragility and tight coupling. 



Scalability
Meaning: Separate parts that have very different read/write patterns, latency requirements, or traffic spikes.
Apply: Profile: high-read vs. heavy-write, sync vs batch, hot vs cold data. Split to scale independently.
YAOS example: Product Search & Browse (high-read, cache/CDN, search index) vs Catalog Authoring (low volume writes, strong integrity).
Pitfall: Running heavy analytics on the live checkout database, slowing down customer transactions. 


## 7.3 domain model
A domain model is an abstraction of a domain that captures core concepts, their relationships, behaviors, and invariants using the domain's own language
It is not merely a data schema, but also encodes business rules and operations so that the software reflects how the domain actually works


### 7.3.1 Ubiquitous Language

![](image/Pasted%20image%2020260217231745.png)


A ubiquitous language is a shared precise vocabulary that is developed and continually refined by domain experts and engineers together, and it is used consistently in conversations, documentation, code, tests, and models within a bounded context
Its purpose is to eliminate ambiguity and translation gaps: the same terms carry the same meaning everywhere they appear in that context, and change in understanding immediately updates the language and the software that embodies it

Lifecycle
Entity Relationships
Invariant & Policies

Ingredients 成分 of a Domain Model (Excerpt)
![](image/Pasted%20image%2020260217231835.png)

## 7.4 What is a bounded context?


![](image/Pasted%20image%2020260217232851.png)

==**A bounded context is an explicit boundary within which a particular domain model and its Ubiquitous Language are defined,** ==valid, and consistent
Inside the boundary, terms have precise meanings and the model is coherent
Outside it, other contexts may use the same words differently, so integration occurs through well-defined contracts (APIs, events) and, when needed, translation and anti-corruption layers

---

![](image/Pasted%20image%2020260217233126.png)

Mapping services to bounded contexts
- A bounded context should map to a single microservice that owns the public contract and private datastore
- If you need more than one service, you are likely discovering subdomains: split the context along business seams and create subdomains with their own bounded contexts
- Technical utility services (file storage, notification, CDN,...) sit outside bounded contexts and contain no business rules
- Cross-cutting business-logic utilities (tax calculation, fraud scoring,...) deserve their own bounded context 
- Avoid several contexts in one service except as a temporary step during early development

## 7.5 Upstream and Downstream services


![](image/Pasted%20image%2020260217233209.png)

Synchronous messaging means that the consumer sends a request and waits for the immediate response, typically over HTTP or gRPC
It fits user-facing reads and simple commands where the result must be known right now, but it couples the consumer to the service's latency and availability
An upstream service is one sending a request to another service, i.e., it sits closer to the client or frontend in the request path than the service invoked
A downstream service is the service receiving a request from another service, i.e., it sits behind the calling service in the dependency chain
Note: In asynchronous message patterns the notion of upstream and dowstream services vary


## 7.6 Context map

![](image/Pasted%20image%2020260217233257.png)

==A context map shows bounded contexts and the model-dependency between them, plus (independently) the direction of message flows== (requests, commands, events)
An upstream context is the one that own and defines the domain model on a given relationship
It sets the language and structure; others must conform
This status says nothing about who calls whom or who publishes events or sends commands
A downstream context is the one that conforms to the upstream model to interoperate
It can still send requests, commands, or events in either direction – message direction is a separate concern from model ownership
Important: Upstream/downstream service ≠ upstream/downstream context 


## 7.7 Relational Patterns for Context Maps

![](image/Pasted%20image%2020260217233541.png)


1 Conformist
![](image/Pasted%20image%2020260217233559.png)
The upstream context owns the model and evolves it primarily for its own needs
The downstream context fully conforms to that model with little or no influence
Integration is simple – use the upstream's schemas, types, and semantics – so delivery is fast and coordination overhead is low
The trade-off is tight coupling: downstream inherits upstream quirks, release cadence, and breaking changes
Use this when the upstream is dominant/authoritative and the downstream's differentiation does not depend on model autonomy

上游上下文拥有模型的所有权，并主要根据自身需求对其进行演进。
下游上下文完全遵循该模型，几乎没有或根本没有影响力。
这种方式的集成很简单——直接使用上游的模式、类型和语义即可——因此交付速度快，协调成本低。
其代价是紧密耦合：下游将继承上游的特性、发布节奏以及破坏性变更。
请在下游的差异化不依赖于模型自主性，且上游处于主导/权威地位时，采用此模式。


----

2 Published Language

The upstream context provides a well-documented, versioned document model, independent of a single customer
Downstreams can conform directly or integrate via an ACL, but the published model stabilizes meaning, enables tooling (validation, codegen), and supports parallel development
Governance matters: versioning rules, deprecation windows, and examples keep the contract reliable as it evolves
Adopt this when multiple downstreams must integrate consistently and you want a durable, explicit contract around the upstream's mode

上游上下文提供一个文档完善、具备版本管理的文档模型，该模型独立于单一客户。
下游可以直接遵循该模型，或通过防腐层（ACL）进行集成，但已发布的模型能稳定语义、支持工具链（如验证、代码生成），并促进并行开发。
治理至关重要：版本管理规则、弃用周期以及示例代码，能在模型演进过程中保持契约的可靠性。
当存在多个下游需要以一致的方式进行集成，并且希望围绕上游模型建立持久、明确的契约时，请采用此模式。

---
3 Anti-Corruption Layer (ACL)

![](image/Pasted%20image%2020260217233830.png)

The downstream preserves its own model and protects it from upstream influence by translating at the boundary
The ACL boundary may be implemented as a software library residing inside the bounded context (or service) or as a boundary implemented by a reverse proxy if multiple downstream contexts exist
Mappers, adapters, and policies convert terms, fix inconsistencies, and enforce downstream invariants so the core stays clean
This adds code, latency, and operational complexity, but it localizes change impact and enables independent evolution
Use ACLs when upstream quality is uneven, semantics don't fit, or the downstream's model is strategically important

下游上下文维护自身的模型，并通过在边界进行转换来保护该模型免受上游的影响。
防腐层（ACL）边界可以作为一个软件库，存在于限界上下文（或服务）内部；如果存在多个下游上下文，它也可以作为由反向代理实现的边界。
通过映射器、适配器和策略来转换术语、修复不一致性并强制保障下游的不变量，从而保持核心域的整洁。
这种方法会增加代码量、延迟和运维复杂性，但它能将变更影响局部化，并支持各上下文独立演进。
当上游质量参差不齐、语义不匹配，或下游的模型具有战略重要性时，请使用防腐层（ACL）。

---

4 Customer/Supplier
![](image/Pasted%20image%2020260217233906.png)
The upstream still owns the model, but the downstream is a named customer whose needs are actively prioritized
There is a clear feedback channel, shared backlogs, or SLAs, and planned delivery windows so the downstream can rely on changes arriving in time
This reduced the pressure to build transaction layers while preserving a single authoritative model
Use it when one upstream serves many consumers but agrees to productize features for important downstreams

上游仍然拥有模型的所有权，但下游是被明确指定的客户，其需求会被优先考虑。
双方存在清晰的反馈渠道、共享的产品待办事项列表或服务水平协议（SLA），以及规划好的交付周期，从而确保下游能够依赖上游及时交付的变更。
这种方式在保留单一权威模型的同时，降低了构建转换层的压力。
当一个上游服务于众多消费者，但同意为重要的下游将特定功能产品化时，请采用此模式。

---

5 Partnership
![](image/Pasted%20image%2020260217233923.png)

Two contexts co-evolve a shared model with joined planning and synchronized releases
There is no fixed upstream/downstream within that scope
Teams collaborate on definitions, testing, and versioning, often with shared ownership of artifacts (schemas, contracts, examples)
The benefit is a model tailored to both parties with fewer translations
The cost is coordination overhead and reduced autonomy
Choose this when both sides have high stakes in a common capability and are willing to invest in collaboration

两个上下文共同演进一个共享模型，并采用联合规划与同步发布的方式。
在此范围内，不存在固定的上游/下游关系。
团队协作进行定义、测试和版本管理，通常对产出物（如模式、契约、示例）拥有共享所有权。
其优势在于，模型能为双方量身定制，减少了转换工作。
代价则是协调成本的增加和自主性的降低。
当双方对某项共同能力都拥有重大利益，并愿意投入精力进行协作时，请选择此模式。

