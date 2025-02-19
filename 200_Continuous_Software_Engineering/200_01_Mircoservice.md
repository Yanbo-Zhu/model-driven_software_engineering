

# 1 Design Principles and Architectural Patterns

Design principles are high-level, abstract guidelines that can help give structure to software.

They provide general rules and best practices, e.g.:
▪ Separation of Concerns
▪ Encapsulation
▪ Explicit Dependencies
▪ Loose Coupling
▪ Persistence Ignorance
▪ Bounded contexts; Programmatic Interfaces


An architectural pattern is a set of architectural design decisions that are applicable to a recurring design problem, and parameterized to account for different software development contexts in which that problem appears.

Specific, reusable solutions or proven ”templates”
e.g.:
• Model-view-controller (MVC) Pattern
• Command Query Responsibility Segregation (CQRS) Pattern
• Circuit Breaker Pattern

# 2 Microservice Architecture

In short, the microservice architectural style is an approach to developing a single application as a suite of small [focused] services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API.

These services are built around business capabilities and are independently deployable by fully automated deployment machinery. 

There is a bare minimum of centralized management of these services, which may be written in different programming languages and use different data storage technologies.”

Each user request is satisfied by some sequence of services
A service represents a specific business capability
Most services are not externally available
Each service communicates with other services through service interfaces

![](image/Pasted%20image%2020250104105635.png)


# 3 Characteristics of Microservices
1. Componentization via Services
2. Organized around Business Capabilities
3. Products not Projects
4. Smart endpoints and dumb pipes
5. Decentralized Governance
6. Decentralized Data Management
7. Infrastructure Automation
8. Design for failure
9. Evolutionary Design



# 4 Microservice Design Patterns: Data Management Patterns

Source:
https://microservices.io/patterns/


Data Management Patterns
• Shared Database
• Database per Service
• API Composer
• CQRS

![](image/Pasted%20image%2020250104123027.png)
## 4.1 Pattern: Shared Database


to meet the characteristic : Decentralized Data Management 

![](image/Pasted%20image%2020241109144120.png)


(a) Private Tables per microservice, sharing a database server and schema
(b) Schema per microservice, sharing a common database server
(c) Database server per microservice

![](image/Pasted%20image%2020241109144320.png)

## 4.2 Pattern: API Composer

How can we query and retrieve data from across multiple microservices?

![](image/Pasted%20image%2020241109144403.png)



## 4.3 Pattern: Command Query Responsibility Segregation (CQRS)


![](image/Pasted%20image%2020241109144422.png)


若干个 service 总调用同一个 service 

左边的每个小黄blocks 去满足  不同的 query needs , 上面的小球 代表 command interface

邮编的 block 上面的小球 是function in API

# 5 Inter-service Communication Patterns

Inter-service Communication Patterns
• Synchronous Communication
• Asynchronous Communication
• Messaging
• Client-side Service Discovery
• Server-side Service Discovery

![](image/Pasted%20image%2020241109144558.png)


---


![](image/Pasted%20image%2020241109144609.png)



Pattern: Client-side Service Discovery
![](image/Pasted%20image%2020241109144713.png)


Pattern: Server-side Service Discovery
![](image/Pasted%20image%2020241109144724.png)

![](image/Pasted%20image%2020241109144756.png)



# 6 Service API Design 

## 6.1 RESTful Services

![](image/Pasted%20image%2020250104105722.png)


## 6.2 API-driven development
-  “API first” – specify an interface, then implement
- Internal vs. External APIs
- API-level documentation (tools like swagger) →
- API documentation serves as contract
    - between teams
    - for testing
- API versioning makes changes explicit

![](image/Pasted%20image%2020250104105816.png)


## 6.3 OpenAPI Specifications and Generators

API Documentation
• Editor based document generation: e.g. https://editor.swagger.io/
• Generation of OpenAPI documents based on code: e.g. https://springdoc.org/

Generation of Webservices based on API Documentation
• OpenAPI Generator: https://openapi-generator.tech/


