
来自 MODEL-DRIVEN SOFTWARE ENGINEERING 这门课 chapter12 

# 1 SYSTEM ARCHITECTURE ARCHITECTURAL CONCEPTS

System Architecture:
− A structured and abstract description of a system, its elements and their relations and interactions
− Exact contents of a system architecture depends on the context/project
− Describes the elements and their properties
− Describes also the context of a system
− E.g. interaction with external systems
− Other software requirements
− Hardware requirements
− Sometimes additional information regarding aspects like security, scalability, operations, maintenance

# 2 MODULARITY

− Decomposition based on function (functional decomposition)
	− Discrete, disjunctive functions
	− Interaction via interface (black box model)
− Advantages:
	− Cope with complexity
	− Restriction of impact of failures
	− Flexibility and (re-)use
− Different levels of granularity
	− Objects
	− Source Code
	− System vs. Sub systems

# 3 DISTRIBUTION

− Spatial partitioning
	− Location based distribution of modularized functionality
	− Needs communication mechanisms between those modules
− Advantages
	− Allows to build different kinds of applications
	− Flexible localisation of computing resources
	− Performance improvement is possible
		− Use of more computing resources (Scalability)
		− Trade-off between local computation and communication overhead
	− Access Control
		− Data may stay in local database but computation results may be shared

# 4 ARCHITECTURAL CONCEPT

## 4.1 MONOLITHIC

− „Oldest“ architectural concept
− No modularity and no distribution
− Self contained
	− Contained user interface and data access
− “No” external dependencies
	− Usually executed on top of a operating system
− A binary executable

## 4.2 PIPELINE

− Sequential execution of processing steps
	− Order of steps needs to be defined
− Data will be handed over from one process to the next process
− Buffers between the processes
− Targets on data stream processing
	− Continuous throughput, variable latency

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200828.png]]


## 4.3 EVENT-DRIVEN

− Communication between components via „events“
	− Events do not have a dedicated receiver
− Components register at event bus as receiver or emitter
− One single event can be received by multiple components
− Event Bus can implement specific event handling paradigms
− Loose coupling between components
− Can be used for user interface


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200847.png]]


## 4.4 ARCHITECTURAL CONCEPTS: CLIENT-SERVER


10
− A.k.a Remote Procedure Calls (RPC)
− Client
	− Initiates a connection
	− Requires a service
− Server
	− Receives services requests
	− Provides the service
	− Sends a reply
− A server can serve multiple clients
	− Robust
	− May in particular improve performance
	− Server can be a bottleneck

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200916.png]]

## 4.5 PEER-TO-PEER

− Decentralized: No dedicated server
	− Peers provide services
	− Peers use services
	− Robustness via redundancy
− Supports flexibility but harder to control
	− Prediction
	− Scheduling
	− Load distribution

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201015.png]]

## 4.6 BLACKBOARD


− Metaphor: Experts in front of a black board
	− Problem is written on the blackboard
	− A single instance can not solve the problem
	− But a single instance can solve a part of the problem
− Blackboard used as centralised data structure
	− Contains problem and partial solutions
	− Access control/coordination needed
	− Changes are propagated to the experts
− This is used in the context of artificial intelligence systems




## 4.7 ARCHITECTURAL CONCEPTS: MOBILE AGENTS


− Mobile agents are processes
	− Data (state)
	− Behaviour (executable code)
− Move (migrate) from one computer to another
	− Mobile agent selects target
	− Collects and processes data
− Try to solve a problem
− Inverse blackboard approach
− Needs a specific agent platform
	− Multi Agent System


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201136.png]]

## 4.8 TIER

− A tier may use the functionality of a neighbour tier
− Separation of general responsibility / functionality
	− Easier to distribute
− Typical incarnation: 3 tier
	− Presentation tier
	− Application tier
	− Database tier
− Very common for web based enterprise systems


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201206.png]]

# 5 MICRO SERVICES


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201239.png]]



MICRO SERVICES
Source: https://microservices.io/
− Single self contained services
− Smaller code base
	− Good separation of concerns
	− Focus on scalability
	− Quick and easy to deploy
	− Isolation of faults
− But:
	− Complexity can’t be reduced
	− Complex communication
	− Unit testing is easier integration testing might not


# 6 HEXAGONAL ARCHITECTURE

− Used as natural choice in Domain-Driven Design approach
− Separates
	− Domain model
	− Application
	− External systems (active or passive)
− Uses ports and adapters

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201406.png]]


