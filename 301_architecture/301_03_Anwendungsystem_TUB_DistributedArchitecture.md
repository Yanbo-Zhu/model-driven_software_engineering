

# 1 component und Layer 


component? 
Components are logical units of abstraction: • Unit of composition • Contractually specified interfaces • Explicit dependencies • No specific assumptions on execution environment • Different possible realizations (programming languages, code structure) • Composable from other software components • Component boundaries are usually derived from logical functionalit



Components and layers as abstractions
Componentization hides complexity
Teams can develop components (mostly) independently
Abstraction allows local changes
Layering = Repeated abstractions
Optimizing lower layers while ignoring upper layers can be expensive
Beware of leaky abstractions
Errors break abstractions


--- 

Lines and Boxes 

![[image/Pasted image 20250506132540.png]]![[image/Pasted image 20250506132540 1.png]]
# 2 Layersand architecture design


![[image/Pasted image 20250506130109.png]]


Client: any user or program that wants to perform an operation over the system.
Clients interact with the system through a presentation layer.
The application logic determines what the system actually does. Application logic can take many forms: programs, constraints, business rules, business processes.
The resource manager deals with the organization (storage, indexing, retrieval) of the data necessary to support the application logic. This is typicallya database, but it can also be a text retrieval system or any other data management system providing querying capabilities and persistence.


Top-down and Buttom-up design

![[image/Pasted image 20250506130231.png]]

![[image/Pasted image 20250506130252.png]]




# 3 From logical layers to physical tiers


## 3.1 1-tier: Fully centralized architecture

![[image/Pasted image 20250506130507.png]]

==The presentation layer, the application logic, and the resource manager are built as one monolithic entity.==

Users/programs access the system through display terminals but what is displayed and how it appears is controlled by the server. (These are “dumb” terminals).

This was the typical architecture of mainframes, offering several advantages:
No forced context switches in the control flow (everything happens within the system)
All is centralized, managing and controlling resources is easier
The design can be highly optimized by blurring the separation between layers


---


## 3.2 2-tier systems / client-server systems

![[image/Pasted image 20250506130528.png]]

==As computers became more powerful, it was possible to move the presentation layer to the client.==

Advantages of the 2-tier architecture
Clients are independent of each other: one could have several presentation layers depending on what each client wants to do.
One can take advantage of the computing power at the client machine to have more sophisticated presentation layers. This also saves computer resources at the server machine.
It introduces the concept of API (Application Programming Interface):an interface to invoke the system from the outside. It also allows designers to think about federating the systems into a single system.
The resource manager still only sees one “client”: the application logic. This greatly helps with performance since there are no client connections/sessions to maintain within the resource layer.

The server has to deal with all possible client connections:
• The maximum number of clients is given by the number of connections supported by the server.
• The load created by a client will directly affect the work of others since they are all competing for the same resources.

![[image/Pasted image 20250506130605.png]]

If clients want to access two or more servers, a 2-tier architecture causes several problems:
The underlying systems don’t know about each other
There is no common business logic
The client is the point of integration (increasingly fat clients)
The responsibility of dealing with heterogeneous systems is shifted to the client.
The client becomes responsible for knowing where things are, how to get to them, and how to ensure consistency

This is tremendously inefficient from all points of view (software design, portability, code reuse, performance since the client capacity is limited, etc.).
There is very little that can be done to solve this problems if staying within the 2 tier model.



---

## 3.3 3-tier: Middleware-based architecture

In a 3-tier system, the three layers are fully separated.
The layers are also typically distributedtaking advantage of the complete modularity of the design (in two tier systems, the server is typically centralized)
3-tier makes only sense in the context of middleware systems (otherwise the client has the same problems as in a 2 tier system)


![[image/Pasted image 20250506131334.png]]

Middleware

Middleware is a level of indirection between clients and other layers of the system, introducing an additional layer encompassing all underlying systems

By doing this, a middleware system:
• Simplifies the design of the clients by reducing the number of interfaces
• Provides transparent access to the underlying systems
• Acts as the platform for inter-system functionality and high level application logic
• Takes care of locating resources, accessing them, and gathering results

Middleware has two roles:
• Programming abstraction: Hide lower-level details   . user only implements thos abstraction which are already definded in middleware 
• Infrastructure: Add functionality, so lower-level details still work


 Integration of systems with different architectures


![[image/Pasted image 20250506131510.png]]


What we have already seen in the few examples so far:
- Middleware can mean many different things
- Middleware can provide a range of different features (from wrapper and communication facilitator to runtime environment)- Theonecommonality is that
- middleware acts as some kind of intermediary layer (sometimes even as an intermediary tier) and- middleware provides programming abstraction and infrastructure implementation

## 3.4 N-tier

N-tier: Extendinga 3-tier systembyaddinga server-sideWeb layer

![[image/Pasted image 20250506131836.png]]




N-tier architectures (multi-tier architectures) result from connecting several 3-tier systemsto each other and/or by adding a Web server as an additional layer to allow clients to access the system
The Web layer was initially external to the system (a true additional layer); today, it is being incorporated into a presentation layer that resides on the server side (part of the middleware infrastructure in a three tier system, or part of the server directly in a two tier system)
The addition of the Web layer led to the notion of application servers, which is used to refer to middleware platforms supporting access through the Web
Today, “traditional” web-based applications usually have n-tier architectures.
Examples: online banking (look for .jspin the URL!) or travel booking sites, …



# 4 Quality of Service


## 4.1 Functionalvs. non-functionalrequirements

**Functional Requirements**
Describe **capabilities** — _“the what”_  
Examples:

- Users can log in via OAuth or using username/password.
    
- Users can browse through the product catalog.
    
- Users can add items to their shopping cart by clicking on a button.


**Non-Functional Requirements**

Describe the **quality** of these capabilities — _“the how good”_  
Examples:

- Log-in shall not take more than 200 ms.
    
- The number of in-stock items may not lag more than 5 seconds behind.
    
- Adding items shall always be possible; annual downtime may not exceed 1 minute.


## 4.2 Quality ofService

QoS goalsareoftendescribedasservicelevelobjectives(SLOs) and listedin servicelevelagreements(SLAs).

![[image/Pasted image 20250506132355.png]]


## 4.3 Performance


**Performance** has two dimensions: **Latency** and **Throughput**

 **Latency**
- Time needed to serve a request, i.e., **response time**
- Usually measured in **milliseconds (ms)**
- Aggregation typically uses **95th or 99th percentiles**  
    → _Why not average?_
    - The average can hide **outliers** and **spikes**, which may seriously affect user experience        
    - Percentiles give a **more realistic picture** of worst-case performance
        

 **Throughput**
- Number of requests that can be served **per unit of time**
- Usually measured in **requests per second (RPS)**
- Often aggregated as **maximum throughput**  
    → _Meaning:_ the **maximum sustainable load** under **latency constraints**


## 4.4 Availability

Describes whether a system is **able to respond to requests**.  
👉 _“Able to respond”_ means the system is **reachable and functioning correctly**, i.e., it can process requests and return valid responses within acceptable timeframes.

---
**Availability is usually quantified** in terms of how periods of **non-availability** are distributed:

- **Mean Time to Repair (MTTR)**  
    → Average time it takes to **restore** the system after a failure
    
- **Mean Time Between Failures (MTBF)**  
    → Average time between two **consecutive failures**
    
- **Uptime**  
    → Percentage of total time that the system is **available**  
    Example:
    
    - 99.9% uptime ≈ 8.76 hours of downtime per year
        
    - 99.99% uptime ≈ 52.6 minutes of downtime per year


![[image/Pasted image 20250506132439.png]]


# 5 Elastic scalability

Describes how well a given system can **adapt to changes in load and resources**.

---

**Key Concepts:**

- Ideally, a system can **serve twice the number of requests** without degradation in other **Quality of Service (QoS)** dimensions when **doubling the amount of resources**.
    
- In practice, **most systems have scalability limits** due to bottlenecks, software constraints, or architectural choices.
    

---

**Scaling Types:**

- **Scaling out / in** (horizontal scaling):  
    → Add or remove instances (e.g., more servers or containers)
    
- **Scaling up / down** (vertical scaling):  
    → Increase or decrease the capacity of a single instance (e.g., more CPU or memory)


![[image/Pasted image 20250506132631.png]]

Elasticity:
•
Describes what happens while scaling
•
Scaling time and scaling impact can be significant–especially when state needs to be migrated betweenmachines.



![[image/Pasted image 20250506132639.png]]



## 5.1 Security 

Has multiple dimensions (their number depends on who you ask).
Authentication: The identity of a party can be confirmed.
Authorization: The rights of a party can be confirmed (and enforced).
Confidentiality: Information should only be able accessible to authorized parties.
Integrity: Information should be safe from unauthorized modification, deletion, creation etc. and should be able to prove it.
Non-repudiation: Parties cannot deny their actions.


## 5.2 Replication

![[image/Pasted image 20250506132758.png]]


![[image/Pasted image 20250506132805.png]]

