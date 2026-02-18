
# 1 DEFINITION & KEY CHARACTERISTICS

Cloud Computing is a model that provides on-demand network access to a shared pool of configurable resources (compute, storage, networking, platforms, apps) that can be rapidly provisioned and released with minimal provider interaction. NIST SP 800-145 


![](image/Pasted%20image%2020260218154729.png)




SELF-SERVICE
A consumer can provision computing capabilities whenever needed, without human interaction with the provider

PAY-AS-YOU-GO
Resource usage is metered (e.g., CPU time, storage volume, ...), monitored, controlled, and reported


ELASTICITY
Capabilities can be elastically provisioned and released, often automatically, to scale with demand


MASSIVE SCALABILITY
The platform supports scaling to very large numbers of instances and high throughput, because capacity is pooled at provider scale

MULTI-TENANCY
Multiple customers (tenants) share underlying infrastructure, while their workload/data remain logically isolated

API-DRIVEN AUTOMATION
Cloud resources are programmable via APIs; infrastructure can be declared, versioned, reviewed, and deployed like code



## 1.1 THE THREE DIMENSIONS OF CLOUD COMPUTING

![](image/Pasted%20image%2020260218154834.png)


Service models
Define how much abstraction the provider offers and how much responsibility remains with the customer

Infrastructure as a Service (IaaS)
Provides raw compute, storage, and networking resources
The customer controls operating systems, middleware, and applications

Platform as a Service (PaaS)
Provides a managed runtime environment for applications
The provider manages infrastructure and platform components

Software as a Service (SaaS)
Provides complete applications as a service
The provider manages the full stack; the customer only consumes functionality 

---

Deployment models
Describe how and by whom the cloud infrastructure is operated
 

Private
Dedicated to a single organization

Community
Shared by multiple organizations with common requirements

Hybrid
Combination of private and public environments

Public
Operated by a provider and shared among many customers 


# 2 IaaS – PaaS – SaaS



![](image/Pasted%20image%2020260218155007.png)


![](image/Pasted%20image%2020260218155206.png)

Software as a Service (SaaS)
- Software services are highly standardized and strongly optimized by the provider
- Customization and extensibility are intentionally limited to ensure efficiency and operational stability

Platform as a Service (PaaS)
- Platform services strike a balance between flexibility and optimization
- They provide managed frameworks with predefined constraints, while still allowing application-level customization and a moderate degree of optimization

Infrastructure as a Service (IaaS)
- Infrastructure services offer maximum flexibility and can host almost any type of application
- However, optimizing the infrastructure for specific application requirements remains largely the customer's responsibility and typically involves higher operational effort


## 2.1 Deployment Models


![](image/Pasted%20image%2020260218155425.png)


# 3 INFRASTRUCTURE AS A SERVICE

Server virtualization abstracts a physical server into multiple isolated Virtual Machines (VMs) on the same hardware
Each VM behaves like a "real" server: it has its own virtual CPU, memory, storage, and network interfaces
==A hypervisor (VM monitor) allocates hardware resources to VMs and enforces isolation between them==
Primary goals: higher utilization, faster provisioning, and work isolation
Typical benefits: elastic capacity, snapshots/rollback, and hardware independence (VMs can be migrated)
Typical trade-offs: performance overhead vs bare metal, plus new operational complexity (VM sprawl, patching images, monitoring)

![](image/Pasted%20image%2020260218160110.png)


Type-1-Hypervisor
Runs directly on the hardware (no host operating system in between)
Each VM contains its own guest operating system plus the hosted services or applications
Typical for data centers and clouds (strong isolation, high performance)
Examples: KVM, Xen, VMware ESXi, Microsoft Hyper-V 

Type-2-Hypervisor
Runs on top of a conventional host operating system (the hypervisor is an application)
Easier to install and use on laptops/desktops, but adds an extra layer (more overhead, host OS becomes part of the Trusted Computing Base) 
Common for development, demos, and testing environments
Examples: VirtualBox, VMware Workstation/Fusion, Parallels


## 3.1 Instances and Instance Types

![](image/Pasted%20image%2020260218160335.png)

- An instance is a provisioned VM: you "rent" a slice of compute capacity on shared physical hardware and get a server you can boot, stop, resize, and terminate
- An instance type (sometimes called a shape) is a predefined hardware profile for the VM
- It determines the resource limits and performance characteristics of the instance  
- Instance types come in families (e.g., general purpose vs compute- or memory-focused) and with sizes within a family (medium, large, xlarge,...) that scale vCPU, RAM, and often network/storage throughput together
- When picking an instance type, you are choosing the VM's compute, memory, and I/O envelope (storage+network) – not just "how many cores", but also how fast it can move data 

Typical configuration dimensions (as in the AWS table) are:
- vCPU: virtual CPU cores available to the VM (compute capacity)
- Memory (GiB): RAM size (working set capacity)
- Instance storage (GB): local disk attached to the host (instance store); if it says EBS-only (Elastic Block Storage), the instance has no local disk and uses network-attached volumes instead
- Network bandwidth (Gbps): maximum network throughput available to the instance
- EBS bandwidth (Gbps): dedicated throughput limit between the instance and its block storage volumes (important for storage-heavy workloads)


## 3.2 Iaas Building Blocks


![](image/Pasted%20image%2020260218160606.png)

- EC2 (Elastic Cloud Compute) is AWS's offering for running VMs – an EC2 instance denotes a concrete, running VM
- A Security Group is accommodated in an AZ and captures network access rules (virtual firewall) that constrain inbound and outbound traffic for instances
- An AMI (Amazon Machine Image) is the bootable template of an EC2 instance, which bundles the operating system, a boot/root disk image, and optionally preinstalled software and baseline configuration
- The same AMI can serve as a reusable golden image to start many identical instances
- Elastic IP denotes an optional static public IP address that can be associated with an instance and reassociated when instances change
- EBS Volumes represent persistent block storage (virtual disks) that can be attached to instances
- Snapshots are point-in-time backups of volumes, used for restore and cloning   

## 3.3 Regions and Availability Zones

- Regions are geographically separated areas (e.g., Europe (Frankfurt)) that contain multiple data centers and offer a full portfolio of cloud services
- Choose a region mainly for latency, data residency/compliance, service availability, and cost
- Availability Zones (AZs) are physically separate groups of data centers within a region (often 3+ per region)
- AZs are connected with high-bandwidth, low-latency links, but are designed to fail independently (power, cooling, networking)
- You use AZs for high-availability: run redundant instances and databases in at least two AZs so a single-AZ outage does not take you down
- Data centers are the individual facilities inside an AZ – customers do not pick a specific data center, instead cloud providers place and move resources within the AZ
- Edge locations are smaller sites close to users to serve latency-sensitive delivery: CDN caching, DNS, and request acceleration
- Edge is not a full region – it is optimized for "close to the user", not for hosting the complete backend stack 

![](image/Pasted%20image%2020260218161528.png)



## 3.4 Image 

![](image/Pasted%20image%2020260218160908.png)

A complete server setup is prepared on a single EC2 prototype instance

The prototype stack typically combines
- Operating system
- Runtime/middleware (e.g., JVM, Node.js, web server)
- One or more microservices (example: Analytics)
- Optional database (often externalized later)
- Configuration (defaults + environment-specific parameters)  
- Monitoring & management agent (health checks, metrics/log shipping, remote management)

The prototype is captured as an image (bootable template of the full baseline)

Multiple target instances are started from the image; each instance begins with the same OS, runtime, services, and operational tooling

Main effect: repeatable deployments and consistent runtime environments across all instances 

## 3.5 OBSERVABILITY

![](image/Pasted%20image%2020260218161034.png)


- Amazon CloudWatch is AWS's central service for observability: collecting metrics, logs, and events from EC2 instances and other AWS resources
- A metric is a named time series, while a measurement is a single value in that series
- Metrics are organized as
    - Namespace (e.g., AWS/EC2, AWS/ApplicationELB, or a custom namespace)
    - Metric name (e.g., CPUUtilization)
    - Dimensions (key/value attribute that identifies which resource)
- A dimension can be related to a single instance or database, a load balancer (representing a cluster of services), or an Auto Scaling Group
- An Auto Scaling Group maintains a desired number of EC2 instances and automatically adds or removes instances based on defined policies (e.g., demand or health)



## 3.6 ELASTIC LOAD BALANCING

![](image/Pasted%20image%2020260218161159.png)

- Elastic Load Balancing (ELB) provides a single, stable entry point for clients and distributes incoming requests across multiple EC2 backend instances
- Traffic is balanced within a region and can span multiple AZs, so a failure of one instance – or even one AZ – does not necessarily take the service down
- ELB performs health checks and forwards requests only to healthy instances; unhealthy instances are removed from request routing automatically
- A separate load balancing service operates the load balancer (based on data provided by CloudWatch), so scaling and high availability of the balancer are handled by the cloud provider
- Main effects: high availability, horizontal scalability, and a clean separation between a public endpoint and an elastically changing set of instances

# 4 Characteristics of Cloud-native Microservices


![](image/Pasted%20image%2020260218161330.png)

# 5 CONTAINERIZATION


![](image/Pasted%20image%2020260218161617.png)

A software container is a lightweight, portable ==package== that ==runs an application as an isolated process== together with its dependencies while sharing the host operating system kernel. 



![](image/Pasted%20image%2020260218161825.png)

- Virtual machines package an application or service together with a complete guest operating system, resulting in strong isolation, but also increases image size, boot time, and patching effort
- Containers package the application or service and its dependencies, while sharing the host operating system kernel, which makes the deployment unit smaller and easier to rebuild, ship, and replace
- In practice, containers are often still hosted on VMs in the cloud, but the VM stays stable while services change frequently
---

![](image/Pasted%20image%2020260218162212.png)

A container bundles everything the application needs at runtime: the application itself, the language runtime with its modules, and the OS user space with binaries and shared libraries
Configuration is supplied separately so that the same image can be executed with different settings across environments
==The container engine starts the process in an isolated environment, while the host operating system provides the shared kernel underneath==
Incoming traffic reaches a host port and is forwarded via port mapping to the container port
The runtime then accepts the connection on the port it listens to inside the container


## 5.1 HYPERVISOR VS CONTAINER ENGINE | VM VS CONTAINER

![](image/Pasted%20image%2020260218162344.png)



## 5.2 CONTAINER WORKFLOW

![](image/Pasted%20image%2020260218162358.png)

A container workflow separates building from running
The build happens on a client machine and produces an immutable image
The image is then distributed via a registry and pulled to the target host
The target host only needs a container engine to turn the image into a running container


On the client, docker build creates an image from the Dockerfile and its build context
The result is stored in a local cache (not shown) and is addressed by name and tag
docker push uploads the tagged image to the registry
The registry stores the image as a set of layers and makes it available to other machines

On the target host, docker pull downloads the image from the registry
The pulled image is placed into the host's local image store
docker run creates a new container from the selected image and starts it
The container engine sets up isolation and runtime configuration and launches the service process inside the container


# 6 PAAS VS FAAS



![](image/Pasted%20image%2020260218162440.png)

In Platform-as-a-Service (PaaS), you deploy and operate a long-running application, and you remain responsible for its code, its configuration, and typically its data model
==Function-as-a-Service (FaaS) pushes this idea further by making the deployable unit a single function that is started on demand and scaled automatically per trigger==
This changes both cost and scalability: instead of paying for always-on capacity, you pay for the number of requests and execution time and benefit from very fine-grained elasticity
The price for this convenience is reduced control over runtime details and a stronger dependency on the provider's execution, event, and integration model 

![](image/Pasted%20image%2020260218162526.png)

## 6.1 
![](image/Pasted%20image%2020260218162536.png)

platform as a Service
In PaaS, the deployment unit is a long-running application service
The cloud provider operates the runtime platform (e.g., Node.js) and the underlying instances (VMs) and hardware, while the application code and configuration is provided by the cloud customer
The service typically exposes an always-on endpoint behind a load balancer, and it scales by automatically adding or removing instances
This makes capacity planning simpler than on IaaS, but the service still exists as a continuously running process
A database can be used alongside the application, yet it remains a separate service with its own scaling and operational characteristics 


Function as a Service
==In FaaS, the deployment unit is a single function==
Functions are invoked on demand, and the platform scales them automatically per trigger, which brings elasticity down to the level of individual operations
This model fits event-driven workflows particularly well, because functions can be triggered by HTTP requests, queues, or domain events
The application becomes a set of independently deployable handlers rather than one continuously running service
State is typically kept outside the function
Persistent state moves to managed databases and storage, and short-lived state is kept in function memory only for the duration of an invocation

## 6.2 Cold Start Latency of FAAS



![](image/Pasted%20image%2020260218162801.png)


Cold starts occur when a function is invoked but no warm execution environment is available yet
In that case, the platform must first allocate a worker, initialize the runtime, fetch the function package, and load the application code before any business logic can run
The additional preparation and loading time is added to the user-visible response time and can dominate latency for short-running functions
Depending on packaging size, dependency loading, and runtime initialization, the preparation and loading phase can easily exceed the actual function execution time 
Warm starts avoid most of these steps because an execution environment already exists and only lightweight scheduling is required

冷启动发生在函数被调用时，但尚未有可用的暖执行环境的情况下。
在这种情况下，平台必须先分配一个工作进程、初始化运行时、获取函数包并加载应用程序代码，之后才能执行业务逻辑。
这些额外的准备和加载时间会增加在用户可见的响应时间中，对于短时运行的函数而言，这部分时间可能成为延迟的主要部分。
根据打包大小、依赖项加载和运行时初始化的不同，准备和加载阶段很容易超过实际的函数执行时间。
暖启动则避免了上述大部分步骤，因为执行环境已经存在，仅需进行轻量级的调度即可。




AWS Lambda's cold-start duration can vary from under 100ms to over 1 second
In practice, it can be higher for heavier runtimes (e.g., JVM), large dependencies, or container-image based functions
There is no guaranteed "keep warm" time, but many practitioners report reuse windows around 5-15 minutes of inactivity before containers are reclaimed 

![](image/Pasted%20image%2020260218163017.png)

# 7 CLOUD ECONOMICS

Capital Expenditure (CAPEX)

In traditional IT environments, organizations must invest upfront in:

servers, storage, and networking equipment
datacenter space, power, and cooling
capacity planning for peak loads
These investments:

are fixed and long-lived
must be made before actual demand is known
often lead to overprovisioning to handle peak usage
Cloud computing significantly reduces CAPEX by:

eliminating the need to purchase and own physical infrastructure
shifting infrastructure ownership to the cloud provider



## 7.1 UTILIZING DEMAND-SIDE ECONOMIES OF SCALE

