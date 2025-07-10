
# 1 Composition: Orchestration und Choreography


miscellaneous ( (人，物）混杂的，各种各样的；多方面的，多才多艺的 )
Both approaches have their pros and cons and are used in practice. 
Orchestration is (usually) the basis of SOA (=> next topic) and is popular in business process management. 
Choreography is (usually) the basis of microservices (=> topic after SOA) and is typically used in modern cloud-native applications.



To increase flexibility, maintainability, and some QoS aspects, software systems have over the years evolved towards increasingly large numbers of components.
=> Today‘s applications are always compositionsof components.
=> Two basic styles of composition: orchestrationand choreography
=> Both have advantages and disadvantages.


Orchestration
![[image/Pasted image 20250708125108.png]]

Central orchestrator has global composition knowledge and calls individual components.

Advantages:
Centralized process knowledge
Easy to conceptualize

Disadvantages:
•Double billing problem
•Single point of failure ( wenn orchestrator fails, system fails)
•Performance and scalability bottleneck
•Extra network hops/calls (including data shipping!)
•Orchestrator usually enforces particular communication style and technology: wenn orchestrator benutzt grpc, dann alle components muss grpc verwenden 

---

Choreography
Decentralized composition knowledge, components know how to interact.
Advantages:
• No extra hops/calls/data shipping => low latency
• Components can decide on communication and interfaces stack individually
• No centralized bottleneck/single point of failure

Disadvantages:
• Hard to keep overview of process
• Can appear chaotic, may need tracing

![[image/Pasted image 20250708125416.png]]


# 2 Service-oriented architecture (SOA)


In SOC, software components are implemented as services.
Services have a number of characteristics (which ones depends on the definition). Generally, the following can be agreed on:
• Services are software components implemented as “black box“
• Services are often compositions of other services
• Service invocations have a clear outcome (“get weather data“, “send email“, “book flight“)

A service-oriented architecture is based on service-oriented computing, traditionally using SOAP/WSDL web services, and often uses orchestration.

Orchestration was often based on BPEL (as the technical solution below all BPM approaches).


A SOA is hard to realize, often businesses simply wrapped their monoliths and said “it‘s a service now“.
Hence, some people argue that SOA is dead.
Discontinued ideas: service registries, semantic web services
Focus of SOA:
• Reusability
• Encapsulation, abstraction

Disadvantages:
• Often high latency
• Complexity

![[image/Pasted image 20250708160655.png]]



![[image/Pasted image 20250708161401.png]]



“So one day Jeff Bezos issued a mandate. He's doing that all the time, of course, and people scramble like ants being pounded with a rubber mallet whenever it happens. But on one occasion --back around 2002 I think, plus or minus a year --he issued a mandate that was so out there, so huge and eye-bulgingly ponderous, that it made all of his other mandates look like unsolicited peer bonuses.“ 翻译

“His Big Mandate went something along these lines:
1.
All teams will henceforth expose their data and functionality through service interfaces.
2.
Teams must communicate with each other through these interfaces.
3.
There will be no other form of interprocesscommunication allowed: no direct linking, no direct reads of another team's data store, no shared-memory model, no back-doors whatsoever. The only communication allowed is via service interface calls over the network.
4.
It doesn't matter what technology they use. HTTP, Corba, Pubsub, custom protocols --doesn't matter. Bezos doesn't care.
5.
All service interfaces, without exception, must be designed from the ground up to be externalizable. That is to say, the team must plan and design to be able to expose the interface to developers in the outside world. No exceptions.
6.
Anyone who doesn't do this will be fired.
7.
Thank you; have a nice day!”


# 3 Microservices 


Disclaimer: The more concepts are current trends or hypes, the more definitions will be vague. And microservices (in some flavor) are the style that current applications tend to follow.
Microservice-based architectures are similar to SOA and can be considered a variant. The terms are often used interchangeably (or rather: we now often say “microservices“ to what was referred to as “SOA“ ten years ago and we often say “microservice” instead of “service”).
In general, microservices aim to be more lightweight and flexible.


Individual tech stacks and polyglot persistence
Often good for scalability since microservices can be scaled individually
Microservices are responsible for their own capabilities AND their data, i.e., access to data only through the interfaces of the microservice, no direct access do a database.
Today, the original microservice style is often adapted to run in Kubernetes clusters, to run as event-driven microservices on serverless cloud platforms, or even to be implemented on top of stream processing platforms.


![[image/Pasted image 20250708161739.png]]


## 3.1 SOA 和 mircoservices 的不同 

![[image/Pasted image 20250708161547.png]]


涉及架构粒度、技术独立性、通信方式和部署方式等方面。以下是它们的主要区别：



| 特性            | **SOA (Service-Oriented Architecture)** | **Microservices Architecture**        |
| ------------- | --------------------------------------- | ------------------------------------- |
| **服务粒度**      | 较粗，大服务可包含多个子功能                          | 较细，每个服务只完成单一职责（单一功能）                  |
| **通信方式**      | 通常使用**企业服务总线（ESB）**，基于 SOAP、XML、WS-* 等  | 轻量级通信，通常使用 **RESTful API** 或 **消息队列** |
| **技术依赖**      | 往往使用特定中间件或平台（如 Java EE）                 | 技术异构，每个服务可以用不同语言或数据库                  |
| **部署方式**      | 多服务部署在同一容器或应用服务器中                       | 每个服务独立部署（如 Docker 容器），更灵活             |
| **数据库**       | 通常多个服务共享同一个数据库                          | 每个微服务拥有**独立的数据库**（去耦合）                |
| **可维护性/可扩展性** | 更适合大型企业应用，但维护难度较大                       | 易于持续交付和快速迭代，更适合敏捷开发                   |
| **耦合性**       | 通常耦合较高（集中式 ESB 是单点）                     | 更松耦合，服务之间相互独立                         |
| **重用性**       | 服务可以重用，适合**企业内部集成**                     | 关注业务领域划分，不强调重用                        |


比如一个电商系统：

### 3.1.1 SOA：

- 有一个 "订单管理服务"，负责所有与订单相关的事情：下单、取消、支付、物流。
    
- 所有模块通过一个 **ESB 中央总线** 通信。
    

### 3.1.2 Microservices：

- 把系统拆成多个小服务：
    
    - `OrderService`
        
    - `PaymentService`
        
    - `ShippingService`
        
    - `InventoryService`
        
- 每个服务单独部署、独立扩展、可独立重启。


# 4 Virtualization

Virtualization is a key technology for isolating different applications running on the same machine –from virtual machines to containers.
Today, applications are almost always Dockerized(also as a deployment vehicle) and deployed on virtual machines.
We rarely use a single isolated container in practice –instead we manage fleets of containers with (typically) Kubernetes, possibly using side-car proxies for communication.




Generally: Virtualization is the process of providing a virtual resource on the same level of abstraction 抽象层级 as a physical one.
Virtualization（虚拟化） 是一种技术，它的核心目标是在与物理资源相同的抽象层级上，提供一种“虚拟”的资源形式。


More specifically (in the context of machines): Instead of using the hardware of one machine to run one OS, we use a hypervisor to divide the resources and expose them as several virtual machines each running their own OS.

![[400_Anwendungssystem/image/Pasted image 20250710144049.png]]


Virtualization started in the 1960s as a way of isolating applications on mainframes –now, it is the basis of cloud computing. Today, server applications usually run in VMs and not directly on physical machines.
Some further reading on virtualization in general:
https://www.ibm.com/topics/virtualization
https://opensource.com/resources/virtualization


hypervisor isoliert hardware resoource und build eine new virtual machine instance darauf 
snapshot of virturalle machine 


---

Remember: “All problems in computer science can be solved by another level of indirection…

...except for the problem of too many layers of indirection.” [1]
(and the resulting performance implications)
=> Can we remove Guest OS and Hypervisor while retaining the benefits of virtualization?

![[400_Anwendungssystem/image/Pasted image 20250710145435.png]]

![[400_Anwendungssystem/image/Pasted image 20250710145533.png]]


## 4.1 Container 

Virtualize the “OS“: App believes to be alone on an OS
=> namespaces for isolation and separation of processes
=> cgroupsfor enforcing resource limits
=> chroot, ….
Host OS is shared by all containers (=> host OS == “guest“ OS). Processes in all containers are visible to Host OS.
Low performance overhead, but higher security risk


![[400_Anwendungssystem/image/Pasted image 20250710150016.png]]


Today, Docker is the de-facto standard for rolling out software:
• Write code
• Write Dockerfile that describes how to turn code into an image, e.g. by compiling it
• Build Docker Image
• Upload image to container runtime
• Create Docker Container from Image whenever a new instance needs to be started
The underlying container storage format has been standardized by the Open Container Initiative, so that the same Image can be run on many platforms.



![[400_Anwendungssystem/image/Pasted image 20250710151019.png]]


start container image 很快 , 比起 start a vm 

![[400_Anwendungssystem/image/Pasted image 20250710151201.png]]



## 4.2 Kubenetes 

Managing Docker containers individually is hard (there are simply too many in practice).
=> We need a container orchestrator to manage lifecycle, deployment, failures, scaling, etc. of our fleet of containers
Today, this is usually done with Kubernetes (K8s).



![[400_Anwendungssystem/image/Pasted image 20250710151601.png]]


In Kubernetes, the fundamental unit of deployment is a container. K8s takes care of networking, storage, availability, ….
Declarative configuration: describe the final state (in a .yamlfile), K8s controllers ensure that the actual state matches the described state.
Key Components
• Cluster: A group of servers (called nodes) that run the applications
• Pod: One or more containers that share networking and storage
• Deployment: Configuration that describes how pods should be created and managed
• Service: A consistent method of accessing applications (regardless of which pods they run in)
• Storage: Persistent storage for applications



---


Users send requests to a RESTful API (running on the Master Node). Inside the master node, controllers and schedulers constantly compare the current and the desired state and act to reconcile the state.
Examples for resources created via the API:

![[400_Anwendungssystem/image/Pasted image 20250710152347.png]]


## 4.3 unikernels and Mircovm

Old ideagettingrenewedattention: unikernels
=> Roughlypackageonlythosepartsof theOS thatareneededtogetherwiththeapplicationcode and rundirectlyon top of hypervisor. E.g.: Don‘tneeddiskaccess? Then,don‘tincludeOS modulesfordiskaccess.
microVMs:
=> unbloatedOS optimizedforserver-basedexecution, e.g., nosupport forUSB sticks
Key difference: microVMsarestill generalpurposeVMs whichcanbeusedtorunmultiple applicationsinside; unikernelsaremorelightweightand targeta single, particularapplications.
=> Of course, it‘sa continuumhowmuchisincludedand howitiscalled.

![[400_Anwendungssystem/image/Pasted image 20250710152658.png]]

**Unikernels**

- 是一种将**应用程序代码和操作系统内核的必要部分打包成一个单一镜像**的技术。
- 它运行**直接在 hypervisor（如 Xen、KVM）上**，而不是传统的操作系统之上。
- 优点是极致轻量，因为：
    - 只包含应用所需的功能模块。
    - 不需要的 OS 组件（如磁盘访问、USB 等）不会包含。
- 🎯 适合**单一用途、单一应用的场景**，如边缘计算、函数计算（FaaS）、IoT。
    

**例子：**  
如果你的服务只用网络和内存，不访问硬盘，就不会包含硬盘相关的 OS 模块 → 更快启动、更小体积、更安全。

**MicroVMs**
- 是一种**瘦身的虚拟机（VM）**，通常运行在像 **Firecracker（AWS Lambda/ Fargate 使用）** 这样的轻量 hypervisor 上。
- 使用的是**最小化的、优化过的 OS（如 Alpine、OSv）**，专为服务器端或容器场景而设计。
- 虽然比标准 VM 小，但仍然是**通用型的虚拟机**，可以跑多个进程。

| 特性       | **Unikernel**                             | **MicroVM**                                 |
| -------- | ----------------------------------------- | ------------------------------------------- |
| 是否为通用 OS | ❌ 不是，专为单一应用打造                             | ✅ 是，只是精简了                                   |
| 启动速度     | 🟢 极快（毫秒级）                                | 🟡 快，但不如 unikernel                          |
| 应用支持     | ❗ 只能运行一个特定编译进的应用                          | ✅ 可运行多个程序或服务                                |
| 体积       | 🟢 极小（几百 KB 到几 MB）                        | 🟡 小于普通 VM，但通常大于 unikernel                  |
| 安全性      | 🟢 高：攻击面极小                                | 🟡 安全但更复杂                                   |
| 使用方式     | 需要将 app 和 kernel 链接生成一个 image             | 使用标准 VM 启动流程                                |
| 示例技术     | MirageOS, IncludeOS, OSv (也被称为 unikernel) | Firecracker, Kata Containers, gVisor (部分类似) |




# 5 Cloud Computing 


NIST: "Cloud computing is a model for enabling
ubiquitous, convenient, on-demand network accessto a
shared pool of configurable computing resources
(e.g., networks, servers, storage, applications, and services) that can be
rapidly provisioned and releasedwith
minimal management effort or service provider interaction.”


## 5.1 Function-as-a-Service

![[400_Anwendungssystem/image/Pasted image 20250710154801.png]]


FaaS is about running backend code without managing your own server systems or your own long-lived server applications.”

Functionsarestatelessand event-driven.

Characteristicsofserverlessapplications
• LEGO-style application engineering
• Heavily use other cloud platform services
• Functions as glue code for other services
• Often event-driven
• FaaS as the core runtime platform
• Sometimes, people also say “FaaS microservices”


![[400_Anwendungssystem/image/Pasted image 20250710154832.png]]



# 6 软件研发进程

Evolution of architectural styles
• Application servers: the systems of the n-tier (and sometimes beyond)
• Composition: two principles of building applications from components
• SOA: components as services, focus on reuse
• Microservices: business domains define components, focus on organizational needs

Execution platforms and technologies
• Virtualization: VMs, Docker, Kubernetes
• Cloud computing: an on-demand infrastructure for applications
• Serverless computing: 3rd gen cloud computing, outsourcing the application ops/lifecycle Summary




1  The Monolith
Failed to address technical needs for
-Interoperability
-Reusability
-Extensibility
-…


---


2 
Interconnectedmonoliths(2-tier/3-tier systems)

Example technology: CORBA
Problems:
-Lack of reusability and extensibility
-Not ready for the web
-…
![[400_Anwendungssystem/image/Pasted image 20250710155354.png]]


---

3 Applicationservers(n-tier systems)

Example technologies: J2EE/Java EE/Jakarta EE, .NET
Problems:
- Lack of reusability
- Often complex to maintain/develop
- Hard to scale
- Can still be monolith-like depending on coupling between components
-
…

![[400_Anwendungssystem/image/Pasted image 20250710155423.png]]

---


4 Service-oriented computing/SOA

Example technologies:
- SOAP/WSDL, WS-*
- BPEL (, BPMN)

Problems:
- Too formalized, too heavyweight
- Lack of flexibility
- Often too slow
- Hard to get it “right“
- Organizational needs
![[400_Anwendungssystem/image/Pasted image 20250710155521.png]]


---


Modern enterprisesystems1/3: classic microservices

Characteristics:
- Vertical cuts through the stack
- Driven by organizational needs
- DevOps
- Various programming platforms
- Polyglot persistence
- Often cloud-based

![[400_Anwendungssystem/image/Pasted image 20250710160116.png]]


---

Modern enterprisesystems2/3: cloud-native

![[400_Anwendungssystem/image/Pasted image 20250710160129.png]]


Characteristics:
- Applications fully containerized
- Inherently cloud-based
- Integrated with cloud platform services
- Roughly: Microservices 2.0




Modern enterprisesystems3/3: Serverless

Characteristics:
- LEGO-style application engineering
- Heavily uses cloud platform services
- Applications are often only glue code
- Often event-driven
- Function-as-a-Service as the core runtime platform

![[400_Anwendungssystem/image/Pasted image 20250710160215.png]]




