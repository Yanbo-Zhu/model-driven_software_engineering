
# 1 service and  API

A service is a self-contained unit of functionality that performs a specific business or technical task and exposes it through a well-defined interface
It is designed to be independent, reusable, and composable, meaning that it can be combined with other services to form a larger system
Services encapsulate their internal logic and data, providing access only via the agreed-upon interface, typically over a network

An Application Programming Interface (API) defines how clients can access and interact with a service
It specifies the operations, data formats, and protocols used for communication, and exposes them through one or more endpoints, i.e., concrete network locations where the service can be invoked



## 1.1 SERVICE COMPONENTS – THE DETAILED VIEW

![](image/Pasted%20image%2020260216215415.png)


The **service implementation** is the internal realization of a service's functionality
- It contains the business logic, data management, and integration code that execute the operations defined by the API
- It can be written in any language, use any database, or call other services
The **service specification** separates the implementation from the outer world and consists of
- a **syntactic specification**, which comprises the service's API
- a **semantic specification**, which defines what the service does, often described using domain vocabulary or ontologies, and
- an **operational specification**, which defines policies for service usage, service level agreements in terms of quality of service parameters, a cost model, and other operational parameters
The **endpoint** is the technical address where the service can be reached, including protocol, host, and path


## 1.2 SERVICE INTERACTIONS

A **consumer** is any entity that uses a service's API to invoke operations
It can be another service, an application, or even a script – anything that sends requests to the service's endpoint according to the defined API contract
Consumers are decoupled from the service's internal implementation and rely only on the exposed interface
 A **message exchange pattern** defines the interaction style between a service and its consumer, i.e., how messages are sent, received, and correlated
It determines whether communication is one-way, two-way, or asynchronous, and how responses or acknowledgements are handled

![](image/Pasted%20image%2020260216215818.png)


![](image/Pasted%20image%2020260216215928.png)


### 1.2.1 UNIFIED BACKEND – MANY FACES AT THE FRONT

- A single backend service can serve multiple types of clients
- All clients communicate through a common API, independent of their implementation technology
- The presentation and interaction logic is implemented on the client side, using device-specific languages and frameworks
- Sharing one service backend reduces development, maintenance, and operational costs, while ensuring consistent business logic across channels

![](image/Pasted%20image%2020260216221605.png)

![](image/Pasted%20image%2020260216221758.png)




### 1.2.2 MONOLITH VS MICROSERVICES


![](image/Pasted%20image%2020260216221843.png)

正向代理 (Forward Proxy)
概念：客户端主动通过代理服务器访问互联网，服务器只知道代理地址，不知道真实客户端。
反向代理 (Reverse Proxy)
概念：客户端请求服务器，不知道真实的服务器是哪一台，请求被反向代理服务器根据负载情况转发到后端的某一台服务器。


A **monolithic** architecture is a single unified application that contains all business logic, data access, and APIs within one deployable unit
All clients connect to the same monolithic API
Easy to develop and deploy initially – one codebase, one runtime, one database
Scalability and agility are limited: even small changes require redeploying the whole system
Failures or performance issues in one part can affect the entire application

A **microservice** architecture is a system that is composed of multiple small, autonomous services, each responsible for a specific business capability
A **reverse proxy** or API gateway routes client requests to the appropriate microservice
Each service has its own API, logic, and often its own database, enabling independent deployment and scaling
Increases flexibility and resilience – one service can evolve or scale without impacting others
Comes with added operational flexibility: inter-service communication, monitoring, deployment, and data consistency become harder


---

Services are organized into tiers that separate concerns: reverse proxy –> Business logic –> Auxiliary services –> Database services
Dependencies flow downwards: each service relies only on the next lower tier
This structure prevents tight coupling and makes individual tiers easy to evolve
Shared functionality (e.g., caching, notifications, analytics) is implemented as auxiliary services, promoting reuse
Database services abstract storage technologies, allowing business services to remain independent of specific databases

![](image/Pasted%20image%2020260216222242.png)



# 2 AI AGENTS

- An AI agent is an autonomous, goal-oriented system that perceives its environment, makes decisions, performs actions, and learns from experience in order to accomplish a task independently
- The lifecycle of an AI agent follows an iterative loop of perception, planning, decision, and action within a dynamic environment
- **Environment**: The external world in which the agent operates. It provides input signals, data, or events that the agent must interpret
- **Perception**: The agent observes and processes information from the environment through sensors, data streams, or APIs to build an internal representation of the current state
- **Planning**: Based on its goals and perceived state, the agent generates possible strategies or sequences of actions to reach a desired outcome
- **Decision-Making**: The agent evaluates alternative plans or actions according to its objectives, constraints, and learned policies, and selects the most suitable option
- **Action**: The chosen behavior is executed, influencing the environment through actuators, service calls, or other interfaces 


---


- Traditional systems focus on data and service layers atop a general IT infrastructure
- Future systems add AI agents and autonomous processes that act on goals, not single requests
- They depend on context and session state, as well as models and algorithms that continuously evolve
- Infrastructure adapts to host both:
    - For services: scalable containers, service meshes, transactional databases, API gateways
    - For agents: GPU/TPU-based compute for inference and training, vector databases, and model stores
- Orchestration layers coordinate mixed workloads and ensure reliable co-existence
The result is a hybrid runtime where deterministic services and adaptive agents interact

![](image/Pasted%20image%2020260216223532.png)

![](image/Pasted%20image%2020260216223603.png)



---


![](image/Pasted%20image%2020260216223632.png)

Multi-Agent Layer
Multiple agents collaborate or negotiate using the Agent-to-Agent protocol (A2A)
It is used to coordinate distributed tasks, exchange goals and context, and realize autonomous process orchestration 
Agent/Component Layer

AI Agents form the operational core
They interact with services and via the Model Context Protocol (MCP), enabling structured, machine-readable exchanges between models, APIs and data sources
Agents may act in the role of service consumers, sometimes as services or service providers
Model Layer

Various AI paradigms – Discriminative, Generative, Reinforcement, and Symbolic AI – provide the computational intelligence
An individual agent may rely on a combination of these models to perceive, reason, and act

# 3 SERVICE AND API TAXONOMIES

分类法；分类学

FUNCTIONAL TAXONOMY – WHAT THE SERVICE DOES
![](image/Pasted%20image%2020260216223813.png)

Architectural Taxonomy – How services are structured
![](image/Pasted%20image%2020260216224653.png)


API Style Taxonomy – How they expose functionality
![](image/Pasted%20image%2020260216224719.png)

![](image/Pasted%20image%2020260216225408.png)

---

Lifecycle/Governance Taxonomy – How APIs are Managed
![](image/Pasted%20image%2020260216225437.png)




# 4 API ECOSYSTEMS

Business Model Taxonomy – Why the API exists economically
![](image/Pasted%20image%2020260216225515.png)

A business model describes how an organization creates, delivers, and captures value in economic, social, or other forms
It explains the logic of the firm: how key resources and activities are combined to produce offerings that serve specific customer segments through defined channels and relationships, while generating revenue and managing costs
The Business Model Canvas, developed by Alexander Osterwalder and Yves Pigneur (2010), is a visual framework for designing, analyzing, and communicating business models
It subdivides the logic of a business model into nine interconnected building blocks, grouped into four areas: infrastructure, offering, customers, and finance


![](image/Pasted%20image%2020260216225712.png)

![](image/Pasted%20image%2020260216225725.png)

---

Digital Value Networks

- Modern API ecosystems form Digital Value Networks, where multiple actors co-create and exchange value through both business relationships and API integrations
- The diagram shows two complementary layers:
    - Black arrows show the value proposition flow: who provides value to whom (e.g., AWS enables YAOS operations, PayPal offers secure payments, ...)
    - Red arrows depict API integrations, i.e., the concrete digital interfaces through which these interactions are implemented
- Within such networks, participants assume different roles and follow distinct interaction patterns:
    - Service providers expose APIs as products
    - Service consumers integrate APIs to enrich their offerings
    - Multi-sided platforms orchestrate interactions between independent groups
- This layered view shows how business value and technical integration reinforce one another, transforming isolated services into interoperable ecosystems where collaboration is API-driven 
![](image/Pasted%20image%2020260216230217.png)
