
# 1 Composition


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

![[image/Pasted image 20250708161547.png]]


Individual tech stacks and polyglot persistence
Often good for scalability since microservices can be scaled individually
Microservices are responsible for their own capabilities AND their data, i.e., access to data only through the interfaces of the microservice, no direct access do a database.
Today, the original microservice style is often adapted to run in Kubernetes clusters, to run as event-driven microservices on serverless cloud platforms, or even to be implemented on top of stream processing platforms.


![[image/Pasted image 20250708161739.png]]

