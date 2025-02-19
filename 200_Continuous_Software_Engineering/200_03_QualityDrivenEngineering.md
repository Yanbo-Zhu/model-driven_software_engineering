
# 1 microservice architectural style 

The microservice architectural style is an approach to developing a single application as a suite of small services,

§ “Each running in its own process and
§ Each communicating with lightweight mechanisms, often an HTTP resource API.
§ These services are built around business capabilities and
§ Are independently deployable by fully automated deployment machinery.
§ There is a bare minimum of centralized management of these services, which
§ May be written in different programming languages and use different data storage technologies.”


Microservices connect with each other over networks and make use of “external” datastores

![](image/Pasted%20image%2020241109114421.png)



# 2 Testing 

## 2.1 Unit Testing Microservices: Sociable and Solitary

![](image/Pasted%20image%2020241109114602.png)

Fowler advocates a distinction based on whether or not the unit under test is isolated from its collaborators.

§ Sociable unit testing focusses on testing the behaviour of modules by observing changes in their state. This treats the unit under test as a black box tested entirely through its interface. 

§ Solitary unit testing looks at the interactions and collaborations between an object and its dependencies, which are replaced by test doubles.


## 2.2 Integration Testing: between layers and external components


![](image/Pasted%20image%2020241109114732.png)


## 2.3 Component testing 

A component is any well-encapsulated, coherent and independently replaceable part of a larger system
§ Testing such components in isolation has benefits:
- Thorough acceptance test of the behaviour encapsulated by that component
- Tests execute more quickly than broad stack equivalents
§ Using test doubles avoids any complex behaviour of peers, helps to create a controlled testing environment


In a microservice architecture, components are the services themselves
§ By writing tests at this granularity, the contract of the API is driven through tests from the perspective of a consumer
§ Isolation of the service is achieved by replacing external collaborators with test doubles and by using internal API endpoints to probe or configure the service


### 2.3.1 In-process component testing

不需要动用 network, 
通过 Dependency injection framrwork 实现  模块之间测试依赖的构成 

By instantiating the full microservice in memory using in memory test doubles and datastores, it is possible to write component tests that do not touch the network whatsoever.

This can lead to faster test execution times and minimizes the number of moving parts reducing build complexity.

However, it also means that the artifact being tested has to be altered for testing purposes to allow it to start up in a 'test' mode. Dependency injection frameworks can help to achieve this by wiring the application differently based on configuration provided at start-up time.

![](image/Pasted%20image%2020241109115559.png)




### 2.3.2 Out of process component testing

Executing component tests against a microservice deployed as a separate process allows more layers and integration points to be exercised. Since all interactions make use of real network calls, the deployment artifact can remain unchanged with no need for any test specific logic.

With this approach, the complexity is pushed into the test harness which is responsible for starting and stopping external stubs and coordinating network ports and configuration.

As a result of the network interactions and use of a real datastore, test execution time is likely to increase. However, if the microservice has complex integration, persistence or startup logic, the out-of-process approach may be more appropriate.

![](image/Pasted%20image%2020241109120154.png)


## 2.4 Contract and end-to-end testing

By combining unit, integration and component testing, we achieve high coverage of the modules that make up a microservice

Intention of end-to-end tests is to verify that the system as a whole meets business goals, irrespective of the component architecture in use
- The system is treated as a black box
- Tests should exercise as much of the fully deployed system as possible
- Manipulating of the system should happen through public interfaces such as GUIs and service APIs 

Since end-to-end tests are more business-facing, they often utilize business readable DSLs, which express test cases in the language of the domain.


## 2.5 Summary 


![](image/Pasted%20image%2020241109143732.png)




![](image/Pasted%20image%2020241109143856.png)


# 3 Monitoring 


Account for available and used resources at OS-level
§ CPU, RAM, network bandwidth, disk capacity, …
§ Different granularity
- Measurement interval: 1m, 1s, 1ms, …
- Moving average, last/current value, …

# 4 Benchmarking  stress test 

By repeatedly measuring quality, we can establish a reference or baseline

Not only measure qualities, ==but specifically provoke stress situations for these qualities==

Every benchmark needs a design: the definition of benchmark requirements (relevance, target audience, implementation reqs, workload reqs, etc) and objectives (specific (small set of) qualities and corresponding quality metrics)

Design Criteria:
− Relevance
− Reproducibility
− Fairness
− Portability
− Understandability


## 4.1 benchmarking system



benchmarking system is needed, that is, a set of components available to perform concrete measurements in support of the benchmark design.

There are at least the following main components:
- A workload generator to generate stress on the SUT
- An experiment controller managing experiments
- A measurement database and component(s) for measuring, collecting, and storing measurement results, and to allow queries against the dataset
- An (optional) monitoring component to avoid bottlenecks in the measurement system

![](image/Pasted%20image%2020241109152343.png)


# 5 Benchmarking vs. Monitoring vs Testing 

- Testing (check functionality during development) vs. Monitoring (observe at runtime)
- Benchmarking vs. Testing: 
    - Testing is done before deployment
    - Unit and Integration Tests ensure functional correctness and compatibility of internals and API
    - “Performance Testing” focuses on performance of individual function calls or components
        - How fast are calls of a specific method across different versions?
    - Benchmarks explore limits or steady-state behavior
-  Monitoring vs. Benchmarking
    - Monitoring: passive and non-intrusive observation of a system to detect undesired states
    - Benchmarking: Driven by the desire to answer a specific question, put systems under stress
    - Both benchmarking and monitoring relate to the process of measuring quality and collecting information on system states; both activities are closely related and inherently interested in the same kind of data

![](image/Pasted%20image%2020241109152533.png)



