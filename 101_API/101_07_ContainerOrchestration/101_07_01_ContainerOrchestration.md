

# 1 DEFINITION & KEY CHARACTERISTICS

![](image/Pasted%20image%2020260218204827.png)



Declarative State
You declare the target state for your workloads. The orchestrator continuously converges the running system toward that state.

Self-healing
Failed containers and workloads are restarted or replaced. Unhealthy instances are removed from traffic until they recover.

Service Discovery und load balancing
Workloads get stable names and endpoints despite changing instances. Requests are distributed across the currently healthy replicas.

controlled rollouts und rollback 
New versions are deployed gradually to reduce risk. A previous version can be restored quickly when problems occur.

Scheduling und placement 
Workloads are assigned to cluster nodes automatically. Placement considers resource needs and constraints to use the cluster efficiently.

Horizontal Scaling 
The number of workload replicas can be increased or decreased. Autoscaling can react to load or metrics to match demand.


Ingress und External exposure 
External traffic is routed into the cluster in a controlled way. Routing rules map hosts or paths to internal services.

Configuration und Secrets injection 
Configuration is kept separate from container images. Secrets and environment-specific settings are provided at runtime.

Resource isolation und Tenancy 
The orchestrator exposes APIs for automation and integration. Platform capabilities can be extended with additional controllers or plugins.


Extensibility und platform Integration 
Resources can be partitioned across teams or applications. Limits and quotas prvent one workload from starving others.



# 2 KUBERNETES 
– WHAT IT IS AND WHY WE NEED IT

What Kubernetes is
==Kubernetes is an open-source platform for container orchestration. It runs containerized applications across a cluster and automates scheduling, scaling, rollouts, and self-healing.==

What problem it solves
It turns a set of machines into a single operational environment for many services. Applications can be deployed and updated without manually placing containers on specific servers.

Where it comes from
Kubernetes originated at Google and was influenced by Google's internal cluster management experience (often associated with systems like Borg). Google open-sourced Kubernetes in 2014 to make these ideas broadly usable.

Who deploys it today
Kubernetes is developed as a community-driven open-source project. It is hosted under the Cloud Native Computing Foundation (CNCF), with contributions from many companies and individuals.

How and where it is used
Kubernetes is used to run microservices, APIs, batch jobs, and platform components in a uniform way. It is common both on-premises and in the cloud via managed services, and it also appears in edge and hybrid setups.

Why it is called k8s
"k8s" is a numeronym: there are eight letters between the "k" and the "s" in "Kubernetes". It is simply a shorter, community-established abbreviation.

Why it matters in practice
==Kubernetes standardizes deployment and operations through declarative configuration and APIs. ==This enables reproducible environments, automation via CI/CD, and portable runtime behavior across different infrastructures


## 2.1 Problem: Proprietary APIs

Proprietary APIs for configuring cloud-based service landscapes create lock-in effects – if you want to change the cloud provider, you start with the configuration from scratch

![](image/Pasted%20image%2020260218211250.png)


If the application is built on the Kubernetes API, it can be transferred relatively easily to any other provider and environment (IaaS
![](image/Pasted%20image%2020260218211309.png)


## 2.2 SYSTEM ARCHITECTURE
![](image/Pasted%20image%2020260218211325.png)


A Kubernetes cluster is a group of machines that act as a single platform for running containerized workloads
It combines a central management layer with distributed compute capacity, so that applications can be deployed, scaled, and healed across multiple nodes
The cluster is split into a control plane and a set of worker nodes
The control plane coordinates the system and stores its state, while the worker nodes provide the resources for executing the application container

- The CLI (Command Line Interface) and UI (User Interface) talk to the cluster through the API server
- The **API server** exposes the management API, validates requests, and persists the declared desired state in etcd, which servers as the clusters configuration database
- The **scheduler** assigns newly created workloads to worker nodes
- A **workload** is a running application component such as a microservice, a batch job, or a background worker, typically deployed as one or more Pods (often via a Deployment) – Users then send requests to those workloads
- The **controller manager** continuously reconciles desired and actual state, for example by creating replacements when instances fail or by keeping the configured number of replicas running



Each worker node runs multiple pods, and each pod contains one or more containers
- A pod is the smallest deployable unit and defines a shared runtime context, such as networking and local volumes, for its containers
- One every worker node, the container runtime (here: Docker engine) pulls images and executes containers
- The kubelet is the node agent that receives instructions from the control plane and ensures that the required Pods are running on that node
- The kube-proxy implements the cluster's service networking on the node
- It provides stable virtual endpoints and load balancing so that requests can be routed to the currently healthy pod instances, even while pods are created, moved, or replaced





## 2.3 RESTFUL APIs

![](image/Pasted%20image%2020260218211627.png)



Kubernetes follows a standardized approach to interacting with a cluster via its robust RESTful API
This central API acts as the sole entry point for all management tasks and data requests, ensuring a unified and consistent control plane 
All interactions with a cluster are initiated by various frontends, including command line interfaces (kubectl), web-based or app-based user interfaces
==Each interface communicates exclusively with the central Kubernetes API server, rather than talking to individual worker nodes or pods directly==
This consistent interface ensures that security controls, validation checks, and logging are applied uniformly across all management entry points



The API exposes cluster resources (such as deployments) as standard RESTful endpoints
Resources are accessed via structured paths
The API server processes these requests and orchestrates the creation, modification, or retrieval of the desired objects
This separation allows clients to manage the desired state of applications (like the account-service) declaratively without needing direct knowledge of the underlying infrastructure




## 2.4 WHAT IS A MANIFEST?

![](image/Pasted%20image%2020260218211807.png)

In Kubernetes, a manifest is a YAML (or JSON) document that defines resources using a declarative state approach
A manifest tells Kubernetes what should exist and how it should look:
which objects to create (e.g., Service, Deployment, Ingress),
- which container image to run,
- how many replicas to keep,
- which ports to expose,
- which environment variables to set,
- and which checks to use.
The Kubernetes control plane then continuously reconciles the live cluster state with the declared state in the manifest and adjusts the system whenever it drifts
Because manifests are plain text, they can be versioned, reviewed, and deployed like code (Git-friendly), and they make deployments repeatable across dev/test/prod by changing only a few parameters (e.g., image tags, replica counts, config values)


# 3 ABOUT CLUSTERS, NODES, AND PODS


## 3.1 ROUTING INSIDE A CLUSTER

![](image/Pasted%20image%2020260218211927.png)


![](image/Pasted%20image%2020260218214741.png)

Traffic is managed across three main network scopes, moving from public visibility down to isolated application processes
- External IPs: Public, internet-routable addresses provided by the cloud provider (e.g., 52,204.151.20). This is where the public internet entry point is located
- Node IPs (Internal IPs): Private addresses used within the cloud provider's virtual network (e.g., 10.128.0.15). These addresses are used to differentiate individual virtual machines (nodes)
- Pod IPs: Highly isolated, cluster private addresses that are managed by Kubernetes (e.g., 192.168.1.47). These are ephemeral and are used exclusively for internal communication 


Traffic is managed across three main network scopes, moving from public visibility down to isolated application processes
- An incoming request is received on a specific node's external IP (52.204.151.20) using a reserved, high-numbered NodePort (:32035). This port is opened on all nodes in the cluster
- Once inside the node, the linux kernel, which is configured by kube-proxy rules, intercepts the packet, and a network address translation (NAT) is performed
- The destination address is internally rewritten by the kernel from the external Node/NodePort combination to the internal, private Pod IP (192.168.1.47)
- The request is then efficiently routed to the target Pod (#3)
In essence, the node port acts as the crucial gateway through which external requests are translated into internal, private, pod-to-pod communication




## 3.2 LOAD BALANCING


![](image/Pasted%20image%2020260218212654.png)


Horizontal scaling and load balancing occur at two distinct levels within modern Kubernetes ecosystems
This layered approach separates public traffic management from the internal self-healing mechanics of the cluster

Level 1 – Internal horizontal scaling and Load Balancing
- The primary level of scaling and load balancing happens entirely within the Kubernetes cluster itself
- When traffic enters the cluster via the node port (:32035), ==the internal kube-proxy intelligently load balances incoming requests across all available healthy pods – regardless of which node they reside on==
- The routing between worker nodes ensures that even if traffic hits Worker Node #3, it can be seamlessly routed internally to Pod #3 on Worker Node #2 
- This provides the self-healing and elasticity of the application tier

evel 2 – External Load Balancing
The second level of load balancing occurs outside the Kubernetes cluster perimeter
- It manages public internet traffic and distributes it evenly to the available Kubernetes nodes
- An external, cloud-provided Load Balancer receives the initial incoming request
- ==It is configured to distribute these requests evenly across the external IPs of the available worker nodes==
- This ensures that the entire cluster entry point is highly available and that no single node is overwhelmed by public traffic 


## 3.3 CONTAINER VS POD

![](image/Pasted%20image%2020260218213218.png)


A container is an isolated process with its own filesystem, i.e., a single isolated package of software
A pod is a wrapper or sandbox that can contain one or more containers
While most pods in Kubernetes contain only one container, a pod is designed to hold a primary container and optional sidecar containers (like a ogging agent or a local cache)
A pod is a logical host that provides a shared network and shared storage for one or more containers
Kubernetes never deploys containers directly, it always deploys pods
In Docker, the host port is mapped onto the container port
In Kubernetes, the node port of each worker node is mapped to the container port 

![](image/Pasted%20image%2020260218213635.png)

## 3.4 HOW TO BUNDLE SERVICES AND CONTAINERS?

![](image/Pasted%20image%2020260218213646.png)

Multiple Services in one Container
Multiple main services are placed into a single container image, managed by a single process manager within that containers
This violates the core design principle that a container should run only one primary process or concern 
This tightly couples the service, prevents independent scaling, and makes updates complex, as a change to one service requires rebuilding and redeploying the entire monolithic container  


Multiple Main Containers in one Pod
Containers of two main applications are placed in a single
Both applications are forced to scale and fail as a single unit
If you need 50 copies of the frontend but only 5 copies of the backend database, this structure is inefficient and impractical
It removes the flexibility and resilience provided by Kubernetes' orchestration capabilities


One service per Container, One Contaner per Pod
Represents the idiomatic Kubernetes approach
Each logical service is isolated within its own container, which is wrapped in its own pod
The design enables independent scaling, allowing the deployment manager to create many copies of the highly used Frontend pods and fewer copies of the Backend pods as needed
Furthermore, it provides robust failure isolation; if the Backend pods fail, the Frontend pods can remain active



## 3.5 MULTIPLE NODE PORTS

![](image/Pasted%20image%2020260218213732.png)



- ==A node port is a specific port opened on all worker nodes, i.e., it is not a port owned by a single node, but a shared entrance for a specific group of pods==
- A single node port is assigned to multiple pods that perform the same function
- When traffic hits that port on a node, the system knows to pick one of those pods to handle the request
- The cluster might have several different node ports active on the same node at the same time 
- Even though the pods have their own private IP addresses, the node port is the public face they all  share




# 4 ABOUT DEPLOYMENTS AND SERVICES



## 4.1 DEPLYOMENTS

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: account-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: account-service
  template:
    metadata:
      labels:
        app: account-service
    spec:
      containers:
        - name: server
          image: axelkuepper/yaos-account-service:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 3000
          env:
            - name: PORT
              value: "3000"
            - name: MONGO_URL
              value: "mongodb://mongodb:27017"
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
          readinessProbe:
            httpGet:
              path: /healthz
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 5
```


Pods are never manually created for production, but managed by a deployment
A deployment object is derived from the manifest and acts as the manager for an application, ensuring, among other things, that the desired number of pods are running and healthy at all times
A deployment defines
- which container image to use,
- the container ports that should be open,
- the labels that allow services to find these pods,
- environment variables, and
- endpoints for health checks.
If a node crashes, the deployment notices that the actual state of pods no longer matches the desired state and automatically starts a new pod on a healthy node
==The deployment is responsible for the life cycle of the application – if a new version of an application available, it creates a new pod, waits for the pod to become healthy, and terminates the old pod==


![](image/Pasted%20image%2020260218213905.png)

## 4.2 SERVICES
答案是：是的，每个 Service 会在所有 Worker Node 上生效，而不仅仅是在某一个节点上。

![](image/Pasted%20image%2020260218213951.png)

While pods are active workers of the cluster, they are inherently ephemeral; they are frequently created, destroyed, and moved, receiving a new IP address each time
==A service in Kubernetes is a persistent abstraction layer that provides a stable IP address for a dynamic group of pods==
By assigning a constant name and IP address to a functional group, a service ensures that internal and external callers can always reach the application, regardless of the individual pod's lifecycle
A service therefore acts as a single point of entry that abstracts the complexity of the underlying network by automatically discovering healthy pods and balancing incoming requests across them
Services come in three different flavors: ClusterIP, NodePort, and LoadBalancer.



### 4.2.1 Cluster IP 

```
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  type: ClusterIP 
  selector:
    app: mongodb    
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```


![](image/Pasted%20image%2020260218214119.png)

A ClusterIP service is the default Kubernetes service type that ==provides a stable, internal-only IP ==address for a group of pods
It enables secure communication between different components of an application within the cluster while remaining completely invisible to the public internet 
The name field defines the pod groups domain name – once this is applied, the pods can be reached via the hostname (mongodb-service) and do not need to know the IP address
targetPort defined the container ports, while port is the visible port used by other pods to invoke the service


### 4.2.2 NodePort

```
apiVersion: v1
kind: Service
metadata:
  name: account-service
spec:
  type: NodePort
  selector:
    app: account-service
  ports:
    - protocol: TCP
      port: 80          
      targetPort: 3000
      nodePort: 32000   

```

![](image/Pasted%20image%2020260218214259.png)

==A NodePort service extends internal cluster connectivity by exposing an application through a specific, dedicated port on the external IP address of every node==
It serves as static gateway that accepts incoming traffic from outside the cluster and transparently routes it to the corresponding backend pods
The nodePort field specifies the public entrance on the a node's physical IP address
The port field specifies the service port used by other pods inside the cluster
The targetPort field is the actual destination in form of the container port
The service is accessible externally via `http://<node-ip>:3200 `and internally via `http://account-service:80`
Both are mapped onto `http://<pod-ip>:3000`


#### 4.2.2.1 NodePort: service  nodeport, port, target port

在 Kubernetes 中，当我们定义一个 Service（服务）时，`nodePort`、`port` 和 `targetPort` 是三个最容易混淆但又至关重要的端口概念。它们分别代表了**从外部到内部，再到容器**的流量路径。

简单来说：`targetPort` 是**容器**听的，`port` 是**Service** 听的，`nodePort` 是**节点**听的。

-   **`targetPort`**：只管 Pod 里面的事（应用监听的端口）。
-   **`port`**：只管 Service 本身的事（集群内部访问的端口）。
-   **`nodePort`**：只管集群边界的事（外部访问的入口）。


核心定义与流量走向

```mermaid
graph LR
    User(外部用户) -->|访问| NodePort[节点IP：nodePort]
    NodePort -->|转发| Service[ClusterIP：port]
    Service -->|代理| Pod[PodIP：targetPort]
    Pod -->|容器进程监听| Container(容器应用)
```

| 端口类型 | 所属对象 | 监听位置 | 通俗解释 | 取值范围 |
| :--- | :--- | :--- | :--- | :--- |
| **targetPort** | Pod / 容器 | Pod 内部 | **应用真正监听的端口**。比如你的 Nginx 镜像暴露的是 80，Java 进程暴露的是 8080，这个值就是告诉 Kubernetes："流量进来后，请发到 Pod 的这个端口上。" | 1-65535 |
| **port** | Service | Service 的虚拟 IP （ClusterIP） | **Service 的虚拟端口**。这是集群内部其他服务访问该 Service 时使用的端口。你可以把它理解为 Service 这个"反向代理"自己监听的端口。 | 1-65535 |
| **nodePort** | 节点 （Node） | 每个工作节点的物理网卡 IP | **对外暴露的节点端口**。当用户从集群外部访问时，访问任意节点的这个端口，流量就会被转发到对应的 Service 上。 | 30000-32767 |

----

它们之间是如何协作的？

假设有一个 Web 应用，容器里启动了一个服务监听了 `8080` 端口。现在我想让外部网络能访问它。

第一步：容器定义
在 Pod 的 spec 中，容器暴露的端口就是 `targetPort` 的基础。
```yaml
# 这是 Deployment 或 Pod 的一部分
containers:
- name: my-app
  image: my-web-app:v1
  ports:
  - containerPort: 8080  # 这就是 targetPort 的基准
```


 第二步：Service 定义 （NodePort 类型）
我们来定义一个 NodePort 类型的 Service：

```yaml
apiVersion： v1
kind： Service
metadata：
  name： my-service
spec：
  type： NodePort          # 声明类型为 NodePort
  selector：
    app： my-app           # 选择后端 Pod
  ports：
    - protocol： TCP
      port： 80            # 【Service Port】集群内部通过 my-service：80 访问
      targetPort： 8080    # 【Target Port】转发到 Pod 的 8080 端口
      nodePort： 30001     # 【Node Port】（可选）如果不指定，K8s 会在 30000-32767 中随机分配一个
```


第三步：访问流程分解
1.  **外部访问**：用户在浏览器输入 `http：//任意节点IP：30001`。
2.  **节点拦截**：请求到达其中一个 Worker 节点的 `30001` 端口（`nodePort`）。节点的 iptables/IPVS 规则捕获了这个请求。
3.  **Service 转发**：节点根据规则，将请求转发给 `my-service` 这个 Service 的虚拟 IP 地址的 `80` 端口（`port`）。
4.  **负载均衡**：Service 根据负载均衡策略，从标签选择器 `app： my-app` 匹配到的 Pod 列表中选一个。
5.  **到达容器**：请求最终被发送到选中的 Pod IP 的 `8080` 端口（`targetPort`）。
6.  **应用处理**：容器内的应用程序在 `8080` 端口接收到请求，进行处理并返回响应。

---
这种分层设计体现了 Kubernetes 的**解耦**思想：

-   **targetPort 解耦应用**：开发人员可以自由选择应用监听的端口（比如习惯用 8080），而不需要关心外部服务如何访问它。
-   **port 解耦服务发现**：集群内的其他服务只需要固定访问 `my-service：80`，即使后端的 Pod 重启、迁移、扩容导致 IP 和端口变化，调用方也不需要修改配置。
-   **nodePort 解耦外部网络**：Kubernetes 统一了对外暴露的端口范围，配合负载均衡器，可以将流量均匀分发到集群的所有节点上。



1.  **targetPort 可以是字符串**：
    除了数字端口，`targetPort` 还可以是你在 Pod 定义中给端口起的**名字**。这在需要经常更改端口号时非常有用。
    ```yaml
    # Pod 定义
    ports：
    - name： http-port      # 给端口起个名字
      containerPort： 8080

    # Service 定义
    targetPort： http-port  # 引用名字，如果将来 Pod 改成 8081，只需改 Pod，Service 不用动
    ```

2.  **如果不指定 nodePort**：
    如果在 YAML 里只写 `type： NodePort`，不写 `nodePort` 字段，Kubernetes 会自动从 `30000-32767` 范围内分配一个可用的端口。你可以通过 `kubectl get svc` 查看分配结果。

3.  **ClusterIP 省略时的默认值**：
    如果你只定义了 `port` 和 `targetPort`，没有显式定义 `ClusterIP`，Kubernetes 会分配一个虚拟 IP。Service 的 `port` 就是这个虚拟 IP 监听的端口。


### 4.2.3 LoadBalancer


```
apiVersion: v1
kind: Service
metadata:
  name: account-service-public
spec:
  type: LoadBalancer
  selector:
    app: account-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
```

![](image/Pasted%20image%2020260218214458.png)

A LoadBalancer service is the standard method for exposing applications to the internet within a cloud environment
Upon creation, Kubernetes automatically provisions a dedicated, public-facing Load Balancer through the cloud provider
==This service type provides a single, stable public IP address that manages incoming traffic and distributes it across the healthy nodes and pods of the cluster==, ensuring high availability and professional-grade external access
The load balancer exists outside the Kubernetes cluster – it is part of the cloud provider's managed network, not the cluster's internal compute
==When a LoadBalancer service is created, a cloud controller within Kubernetes sends an API call to the cloud provider to ask for the provision of a new load balancer and connecting it with the node ports of the cluster==
The port field defines the external port, i.e., `http://<external-lb-ip>:port,` which is first mapped onto `http://<node-ip>:nodePort`, which is finally mapped onto `http://<pod-ip>:targetPort`



## 4.3 KUBERNETES DEPLYOMENT WORKFLOW (SIMPLIFIED)

![](image/Pasted%20image%2020260218214529.png)
