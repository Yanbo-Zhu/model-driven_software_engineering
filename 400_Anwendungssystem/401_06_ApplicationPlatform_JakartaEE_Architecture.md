

 Evolution of architectural styles: Application servers

Enterprise n-tier applications often run on top of application servers which provide the middleware runtime features such as dependency injection, concurrency control, security, event-driven programming, persistence, transactions, etc. in so-called containers. => Application server = middleware
Jakarta EE (formerly: Java EE (formerly J2EE) ) is a group of specifications implemented by, e.g.,
Glassfish, WebSphere, WildFly/JBoss.
Jakarta EE alternatives include Spring or .NET.



# 1 Jakarta EE Overview 

Jakarta EE
├── Web Container
│   ├── Servlet（后端逻辑处理）
│   └── JSP（前端页面生成）
├── JPA（数据库访问）
├── EJB（企业业务逻辑）
├── CDI（依赖注入）
└── JAX-RS（REST 服务）



![[400_Anwendungssystem/image/Pasted image 20250703152018.png]]



# 2 fat and thin Client 

fat client 
thin client 

---

Thin Client（瘦客户端）

定义：
瘦客户端是一个**功能较少**的客户端程序，**几乎不处理业务逻辑**，只负责展示用户界面和与服务器交互。

特点：
- 所有的逻辑都在服务器端处理（如 Java EE 的 EJB、Servlet、JSP 等）
- 客户端通常是浏览器
- 易于部署和维护
- 更依赖网络连接
- 安全性更高（逻辑在服务器）

在 Java EE 中：
- 客户端：HTML + JavaScript（通过 HTTP 与 Servlet/JSP 通信）
- 服务端：JSP/Servlet + EJB（业务逻辑 + 数据访问）

---
Fat Client（胖客户端）

has application layer komponent in client 

定义：
胖客户端拥有**较多功能**，包括部分或全部**业务逻辑**。它不是仅仅作为展示界面，还可以处理数据、进行计算、验证等。

特点：
- 客户端包含业务逻辑（如 Java Swing、JavaFX 应用）
- 对服务器的依赖较小（只用于数据存取）
- 响应快（部分逻辑本地处理）
- 更新麻烦（每个客户端都需要升级）
- 安全风险高（逻辑暴露在用户端）

在 Java EE 中：
- 客户端：Java Swing、JavaFX 应用，使用 RMI、CORBA 或 Web 服务连接到服务器
- 服务端：提供数据服务和部分逻辑（EJB、REST API 等）



|项目|Thin Client|Fat Client|
|---|---|---|
|业务逻辑处理位置|服务器端|客户端和服务器端共享|
|部署/更新|简单（只更新服务器）|复杂（需要更新客户端）|
|响应速度|相对慢（依赖网络）|相对快（本地处理）|
|示例|浏览器访问 Web 应用|Java Swing 桌面程序|
|与 Java EE 结合|Servlet/JSP, JSF|Swing/JavaFX + EJB/RMI|


- **Thin Client**：网页系统、企业后台、政府门户等
- **Fat Client**：银行终端系统、工程软件、本地化企业工具等
# 3 Web Container 

nur Web Container  braucht,  dann use tomcat 

- 是 Jakarta EE 的一个**运行环境**，专门负责执行：
    - **Servlets**
    - **JSP (Java Server Pages)**
- 主要功能：
    - 管理生命周期（初始化、销毁）
    - 接收和处理 HTTP 请求
    - 负责 Web 应用部署

**典型 Web Container 实现**：
- Apache Tomcat（只支持 Web 部分，不支持全部 Jakarta EE 规范）
- Jetty
- Undertow

## 3.1 Servlets 


Provides entry point for requests from the web.
Most basic specification: Servlets – provide endpoints for HTTP call, 
然后通过 定义 这个 java class 去 确定 response 什么信息 

![[400_Anwendungssystem/image/Pasted image 20250703152257.png]]

- 是 Java 写的一个**服务器端程序组件**，用于处理 HTTP 请求和响应。
- 类似于控制器（Controller）的角色。    
- 编写方式是继承 `javax.servlet.http.HttpServlet` 并重写 `doGet()` / `doPost()` 方法。


```java
public class HelloWorld extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<h1>Hello world!</h1>");  // 这里发回的 信息很丑 用 Jakarta Server Pages 来构造 返回页  
	}
}
```




## 3.2 Jakarta Server Pages   JSP 
• Write HTML sites with special tags for embedding Java code
• JSPs are compiled into servlets for execution

- 全称是 **JavaServer Pages**。
- 是一种**动态 HTML 页面**，嵌入 Java 代码来生成内容。
- JSP 被 Web 容器转译成 Servlet 后运行。    
- 用于创建前端视图页面（HTML + 动态内容）。

```html
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=US-ASCII">
		<title>First JSP</title>
	</head>
	<%@ page import="java.util.Date" %>
	<body>
		<h1>Hello World!</h1><br>
		Current Time on the server is: <%=new Date() %>
	</body>
</html>

<html>
  <body>
    <h1>Hello, <%= request.getParameter("name") %></h1>
  </body>
</html>


```





### 3.2.1 JSP 如何被转译成 Servlet 运行？

- JSP 是把 Java 代码和 HTML 混合写的一种方便方式。
- 但 Web 容器只能运行 Java 类（Servlet），不能直接运行 JSP 文件。    
- 所以容器需要先把 JSP 转成 Servlet，再编译运行。


JSP 代码：
`Hello, <%= request.getParameter("name") %>!`
转换后对应的 Servlet 代码大致是：
```
out.print("Hello, ");
out.print(request.getParameter("name"));
out.print("!");

```


|步骤|说明|
|---|---|
|请求 JSP|浏览器请求 `.jsp` 页面|
|JSP 转 Servlet|容器将 JSP 文件转换为 Java Servlet 源代码|
|编译 Servlet|编译生成 `.class` 字节码文件|
|加载 Servlet|加载并初始化 Servlet|
|执行 Servlet|运行 Servlet，生成响应内容|
|返回响应|把生成的 HTML 等内容返回给客户端|

- **客户端请求 JSP 页面**  
    浏览器访问一个 `.jsp` 页面，比如 `http://example.com/index.jsp`，这个请求会被 Web 容器（比如 Tomcat）接收。
    
- **检查 JSP 编译状态**  
    容器会检查：
    - 这个 JSP 是否之前已经被请求过？
    - JSP 文件有没有发生修改？
    - 是否存在已经编译好的对应 Servlet 类？
- **如果 JSP 是第一次请求或者文件更新了，容器会将 JSP 文件转成 Java Servlet 源代码**
    - 容器会解析 JSP 文件内容。
    - 把 JSP 中的 HTML 代码转换成 `out.print()` 语句。
    - 把 JSP 中的脚本代码（比如 `<% ... %>`）转换成 Java 代码，放进 Servlet 的相应方法里（通常是 `service()` 或者 `doGet()`、`doPost()`）。
- **编译生成的 Servlet Java 源码**
    - 容器调用 Java 编译器将转换后的 Java 源代码编译成字节码（`.class` 文件）。
- **加载并初始化 Servlet 类**
    - 容器将编译好的 Servlet 类加载进 JVM。
    - 调用 Servlet 的 `init()` 方法进行初始化。
- **执行 Servlet 的服务方法**
    - 处理 HTTP 请求，执行 `service()`（或者 `doGet()`、`doPost()`）方法。
    - 将响应的内容（HTML 等）通过 `out.print()` 输出，发送回客户端。
- **后续请求直接调用已编译的 Servlet**
    - 只要 JSP 文件没有变更，容器后续会直接用已经编译好的 Servlet，避免重复转换和编译，提高效率。


### 3.2.2 jsp 对应的 servlet 的 class


JSP 对应的 Servlet 的 `.class` 文件，一般由 Web 容器自动生成并存放在服务器的工作目录中。具体位置和文件名取决于你使用的 Web 容器（比如 Tomcat）和它的配置。


以 Tomcat 为例，JSP 对应的 Servlet `.class` 文件存放路径
- Tomcat 会把 JSP 转换成 Java Servlet 源文件（`.java`），然后编译成 `.class` 文件。    
- 这些生成的文件默认保存在 Tomcat 的工作目录中，路径类似于：

```
<TOMCAT_HOME>/work/Catalina/localhost/<webapp-name>/org/apache/jsp/
```

例如，你有一个 JSP 文件叫 index.jsp，对应生成的 Servlet 类文件一般叫： index_jsp.class

- `<TOMCAT_HOME>` 是你的 Tomcat 安装目录。
- `Catalina` 是 Tomcat 的默认服务器名称。
- `localhost` 是虚拟主机名称（通常是 localhost）。
- `<webapp-name>` 是你部署的 web 应用名（比如 ROOT 或者 myapp）。
- `org/apache/jsp/` 是 Tomcat 生成的 JSP Servlet 包路径。
- `.java` 文件是 JSP 转换后的 Java 源文件（可以查看，但会被定期清理）。
- `.class` 文件是编译后的字节码，是实际运行的 Servlet。

---

假设：
- 你的 Tomcat 安装在 `/usr/local/tomcat`
- Web 应用名是 `myapp`
- JSP 文件是 `hello.jsp`
那么生成的 Servlet 类文件路径大致为

`/usr/local/tomcat/work/Catalina/localhost/myapp/org/apache/jsp/hello_jsp.class`


---

- JSP 文件被容器转成 Servlet `.java` 文件并编译成 `.class` 文件。
- 这个 `.class` 文件就是对应的 Servlet，容器通过它来处理 JSP 请求。
- 位置在 Tomcat 的 `work` 目录下，对应应用和包结构。
- 你可以去这个目录查看编译后的 Servlet 类文件。


## 3.3 JAX-RS（REST 服务）

Others: JAX-WS and JAX-RS to provide SOAP and REST endpoints by annotating a method

- **JAX-RS** 是 Java 平台上的一个规范，用于简化开发基于 REST（Representational State Transfer）架构风格的 Web 服务。
- 它定义了一组注解和 API，帮助开发者快速创建符合 REST 原则的服务端接口。
- JAX-RS 是 Java EE（Jakarta EE）标准的一部分，很多 Java 应用服务器（如 WildFly、GlassFish、Tomcat+Jersey）都支持它。

|内容|说明|
|---|---|
|JAX-RS|Java 标准的 RESTful Web 服务 API|
|作用|简化 REST 服务开发|
|核心注解|`@Path`, `@GET`, `@POST`, `@Produces` 等|
|典型返回格式|JSON、XML|
|适用场景|构建轻量级、可扩展的 Web API|


|注解|作用|
|---|---|
|`@Path`|定义资源路径|
|`@GET`|处理 HTTP GET 请求|
|`@POST`|处理 HTTP POST 请求|
|`@PUT`|处理 HTTP PUT 请求|
|`@DELETE`|处理 HTTP DELETE 请求|
|`@Produces`|指定返回数据格式（如 JSON、XML）|
|`@Consumes`|指定接收数据格式（如 JSON、XML）|
|`@PathParam`|从 URI 中获取参数|

---

 2.4 运行环境
- 你需要一个支持 JAX-RS 的应用服务器（如 WildFly、GlassFish），或者用 Jersey、RESTEasy 这类 JAX-RS 实现库 部署到普通 Servlet 容器（如 Tomcat）。
- 在部署时，通常会有一个 `Application` 类用来配置 JAX-RS：

```java
import jakarta.ws.rs.ApplicationPath;
import jakarta.ws.rs.core.Application;

@ApplicationPath("/api")
public class RestApplication extends Application {
    // 可以用来配置资源和提供者
}

```

这表示所有 JAX-RS 路径前缀为 `/api`，所以上面用户服务的完整路径是 `/api/users/{id}`。



### 3.3.1 例子


```java
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.PathParam;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;
import jakarta.ws.rs.core.Response;

@Path("/users")  // 资源根路径
public class UserService {

    // 模拟数据库
    private static Map<String, String> users = Map.of(
        "1", "Alice",
        "2", "Bob",
        "3", "Charlie"
    );

    // GET /users/{id} 返回指定用户信息
    @GET
    @Path("/{id}")
    @Produces(MediaType.APPLICATION_JSON)  // 返回 JSON 格式
    public Response getUser(@PathParam("id") String id) {
        String name = users.get(id);
        if (name == null) {
            return Response.status(Response.Status.NOT_FOUND).build();
        }
        return Response.ok("{\"id\":\"" + id + "\", \"name\":\"" + name + "\"}").build();
    }
}

```

- 类上用 `@Path("/users")` 定义根路径，表示该类处理 `/users` 相关请求。
- 方法 `getUser` 通过 `@GET` 标识响应 HTTP GET 请求。
- `@Path("/{id}")` 表示路径中有一个参数 `id`，用 `@PathParam("id")` 注入方法参数。
- `@Produces(MediaType.APPLICATION_JSON)` 表示返回 JSON 格式。    
- 如果用户存在，返回 HTTP 200 状态和用户 JSON；否则返回 404 状态。



## 3.4 **Servlets** 和 **JAX-RS** 之间关系 

**Servlet 是底层的 HTTP 处理机制，JAX-RS 是建立在 Servlet 之上，用注解简化 RESTful Web 服务开发的框架。**

Servlet 是什么？
- **Servlet** 是 Java EE 的基础 Web 组件，是服务器端 Java 程序，专门用来处理 HTTP 请求和响应。    
- 它们运行在 Servlet 容器（比如 Tomcat、Jetty）中，通过覆盖 `doGet()`, `doPost()` 等方法来处理 HTTP 请求。
- Servlet 负责接收请求、处理业务逻辑、生成响应（HTML、JSON、XML 等）。

JAX-RS 是什么
- **JAX-RS** 是 Java 生态中专门用来开发 RESTful Web 服务的 API 规范。
- 它建立在 Servlet 之上，封装了 HTTP 请求的处理，让开发者用注解（`@Path`、`@GET`、`@POST` 等）来声明资源和操作，简化了编写 REST 服务的复杂度。
- JAX-RS 运行在支持 Servlet 规范的容器中。


|方面|Servlet|JAX-RS|
|---|---|---|
|层级关系|底层技术，提供基础的 HTTP 请求/响应处理|基于 Servlet 的高级抽象，用于开发 RESTful API|
|编程模型|需要手动管理请求参数、响应内容，代码较底层|使用注解和 POJO，声明式编程，代码简洁|
|使用方式|需要继承 `HttpServlet` 并重写方法|开发资源类，使用注解声明路径和方法|
|运行环境|Servlet 容器（Tomcat、Jetty、WildFly 等）|依赖 Servlet 容器，通过实现（如 Jersey、RESTEasy）实现运行|
|主要用途|构建任何基于 HTTP 的服务，通常是传统 Web 应用|专注构建 RESTful Web 服务|
|代码复杂度|相对较高，需要自己处理大量细节|简单易用，自动管理请求绑定、序列化等细节|

实际工作流程
- **Servlet**：容器收到 HTTP 请求 → 分发给对应的 Servlet → 调用 `doGet()`, `doPost()` 等 → 处理请求 → 生成响应 → 返回给客户端。
- **JAX-RS**：容器收到 HTTP 请求 → 分发给 JAX-RS 运行时（比如 Jersey）内部的 Servlet → 根据注解映射找到对应资源方法 → 执行方法 → 返回响应。
简单说，JAX-RS 本质上就是一个更高层次的框架，基于 Servlet 实现，帮你省去手写大量 Servlet 代码，专注于写 REST API。

### 3.4.1 例子 


Servlet 示例
```
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        resp.setContentType("text/plain");
        resp.getWriter().write("Hello World from Servlet");
    }
}

```

- 你需要在 `web.xml` 或使用注解配置这个 Servlet。    
- 需要手动管理请求和响应，写很多样板代码。

JAX-RS 示例
```
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class HelloResource {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String sayHello() {
        return "Hello World from JAX-RS";
    }
}

```

- 这里用 `@Path` 指定路径，`@GET` 表示处理 GET 请求。
- `@Produces` 指定响应的媒体类型。    
- 代码更简洁，专注业务逻辑，无需处理请求和响应细节。




运行环境

- Servlet 需要部署到支持 Servlet 规范的容器（如 Tomcat）
- JAX-RS 资源类通常通过一个 JAX-RS 实现（如 Jersey、RESTEasy）部署在 Servlet 容器里。JAX-RS 实现会有一个 Servlet（例如 `ServletContainer`）来接收请求，调用对应的资源方法。


# 4 EJB Container  **Enterprise JavaBeans**

实现 dependecy injection and refection 
EJB，全称是 **Enterprise JavaBeans**，中文一般叫 **企业级 Java Bean**，它是 Java EE（现为 Jakarta EE）平台中的一个重要组件规范，主要用于实现企业级应用中的**业务逻辑层**。bussines logic layer 

什么是 EJB？
- EJB 是一种服务器端组件模型，专门设计用来封装企业应用中的业务逻辑。
- 它运行在支持 Java EE 的应用服务器（比如 WildFly、GlassFish、WebLogic）上。
- 通过 EJB，开发者可以专注于业务功能的实现，而不用关心底层事务管理、安全、并发控制等复杂细节。


EJB 的主要作用
- **封装业务逻辑**：处理业务规则和计算，比如订单处理、客户管理、支付结算等。
- **事务管理**：自动处理数据库事务，保证数据的一致性和完整性。
- **安全控制**：支持声明式安全，控制访问权限。
- **并发管理**：自动处理多用户并发访问的问题。
- **远程访问**：支持远程调用，分布式系统中的业务组件可以被远程客户端调用。
- **生命周期管理**：应用服务器管理 EJB 的创建、销毁和状态维护。


EJB 的几种类型
1. **Session Beans（会话 Bean）**
    - 处理客户端的请求和业务逻辑。
    - 分为三种：
        - **Stateless（无状态）**：不保存客户端状态，适合处理独立请求。
        - **Stateful（有状态）**：保存客户端会话状态，适合会话关联操作。
        - **Singleton（单例）**：应用中只有一个实例，适合共享资源管理。
2. **Message-Driven Beans（消息驱动 Bean）**
    - 用于异步处理消息，常与 JMS（Java 消息服务）结合，用来处理消息队列和主题。
3. **Entity Beans（实体 Bean）** — 现在基本被 JPA（Java Persistence API）替代，不推荐使用。

EJB 的优势
- **标准化**：由 Java EE 标准定义，保证可移植性和互操作性。
- **简化开发**：自动管理事务、安全、并发等复杂功能，减少样板代码。
- **高扩展性和可靠性**：适合大型、复杂的分布式企业应用。



|关键词|说明|
|---|---|
|EJB|企业级 Java 业务组件，负责业务逻辑实现|
|运行环境|需要支持 Java EE 的应用服务器|
|功能|事务、安全、并发、远程访问自动管理|
|类型|Stateless、Stateful、Singleton、Message-Driven|
|适用场景|大型分布式企业应用，复杂业务逻辑层|

## 4.1 EJB instance create 

EJB Is called from web container and directly from thick clients or remote Jakarta EE applications.
EJBs are usually not instantiated by directly calling a constructor but rather from dependency injection or as a result of a remote request. The EJB container manages the instance lifecycle and can pool objects.

EJB 是由容器管理的组件，开发者不直接用 `new` 创建它，而是通过注入或远程请求使用。容器会自动管理它的生命周期，并通过对象池优化性能。

1 EJBs 通常不是通过 new 创建的
```java
// ❌ 不推荐这样创建 EJB：
MyServiceBean service = new MyServiceBean();

```
- 在使用 **EJB（Enterprise Java Beans）** 时，你通常不会直接使用 `new` 来创建它的实例。    
- EJB 实例是由 **EJB 容器（如在应用服务器中）自动管理** 的。

2 EJB 实例是由容器通过依赖注入 (Dependency Injection) 或远程调用来提供的
```
@EJB
private MyServiceBean myService;
```

- `@EJB` 注解告诉容器：请注入一个管理好的 EJB 实例。
- 这叫做 **依赖注入（DI）**，由容器来完成对象的创建、初始化与生命周期管理。
- 如果是远程调用（例如从另一个 JVM 调用），容器也会根据请求创建或提供实例。

3  **EJB 容器负责管理 EJB 实例的生命周期**

容器会处理：
- 创建
- 初始化
- 销毁
- 方法调用前后的拦截（AOP）
- 安全检查
- 事务管理等


4 **容器可以使用对象池（Object Pooling）来提高性能**

- 为了避免频繁创建和销毁对象，容器可以维护一个 **对象池**。
- 当客户端请求一个 EJB 时，容器可以从池中取一个已有实例复用，而不是每次都 `new` 一个。
- 这样可以 **提高系统性能和可伸缩性**。


## 4.2 Session Beans
rufe asynchronous 

- 处理客户端的请求和业务逻辑。
- 分为三种：
	- **Stateless（无状态）**：不保存客户端状态，适合处理独立请求。
	- **Stateful（有状态）**：保存客户端会话状态，适合会话关联操作。
	- **Singleton（单例）**：应用中只有一个实例，适合共享资源管理。

Class is annotated with @Stateless, @Stafeful or @Singleton:
• Stateless session beans do not hold any session state and can be pooled and reused as needed by the container
• Stateful session beans can have instance variables to hold session state (and are not pooled)
• Singleton session beans exist only once

Additional annotations, e.g., to differentiate between local and remote invocations can be added.

这里，`OrderServiceBean` 是一个无状态会话 Bean，应用服务器会自动管理它的生命周期和事务。
```java
import javax.ejb.Stateless;

@Stateless
public class OrderServiceBean implements OrderService {
    public void placeOrder(Order order) {
        // 处理下订单的业务逻辑
    }
}

```



### 4.2.1 **Stateless EJB 生命周期流程图**

- **Stateless EJB 是无状态的**，意味着容器可以复用同一个实例处理多个客户端请求。
- 生命周期由 **容器托管**，开发者只需关注业务逻辑。
- 使用 `@PostConstruct` 和 `@PreDestroy` 可以在初始化和销毁阶段执行额外操作。
- 容器可能会提前创建一批实例（对象池）以提高性能。


```
┌────────────────────┐
│     EJB 容器启动     │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ 实例化 (new Bean)   │ ←─── 由容器创建 Bean 实例
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│   依赖注入 (@EJB)   │ ←─── 注入其他资源，如数据库、服务等
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ @PostConstruct 方法 │ ←─── 初始化，例如打开连接
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│     就绪状态        │ ←─── 可以响应客户端请求
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│  方法调用 (业务逻辑)│ ←─── 客户端调用业务方法
└────────┬───────────┘
         │
         ▼
(重复使用，无状态 Bean 可被多个请求复用)
         │
         ▼
┌────────────────────┐
│ @PreDestroy 方法    │ ←─── 容器关闭或释放 Bean 时调用
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│    Bean 实例销毁    │
└────────────────────┘

```



```java
import javax.ejb.Stateless;
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

@Stateless
public class MyStatelessBean {

    @PostConstruct
    public void init() {
        System.out.println("Bean initialized");
    }

    public void doWork() {
        System.out.println("Executing business logic");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Bean is about to be destroyed");
    }
}

```

## 4.3 Message-driven beans
用于异步处理消息，常与 JMS（Java 消息服务）结合，用来处理消息队列和主题。

• Event-driven programming, called from Java‘s proprietary messaging system JMS
• Application code is written in onMessage(Message msg) method
• Calls can be part of a transaction
• Check-out example here  https://www.baeldung.com/ejb-message-driven-beans

在基于 JMS 的事件驱动编程中，你的应用代码写在 `onMessage()` 方法里，由容器在消息到达时自动调用，并且这个方法可以参与事务管理，从而保证消息的可靠处理。

---

1 Event-driven programming, called from Java's proprietary messaging system JMS

**事件驱动编程** 是一种编程模式：程序的执行是由事件触发的，而不是按顺序执行。
在这里的事件来源是 **JMS（Java Message Service）**，它是 Java 提供的一套官方消息队列系统，用于让应用程序通过异步消息进行通信。
- 应用可以订阅队列（Queue）或主题（Topic），当有消息发送到这些通道时，就会触发程序去处理。
- JMS 是 Java EE（现在是 Jakarta EE）规范的一部分。



---

2 Application code is written in onMessage(Message msg) method

你写的业务逻辑代码会放在一个名为 `onMessage(Message msg)` 的方法中，这个方法会在有消息到达时由容器自动调用。

这是典型的 **Message-Driven Bean（MDB）** 的写法：
```
@MessageDriven
public class MyMessageBean implements MessageListener {
  
    @Override
    public void onMessage(Message msg) {
        // 这里写你的业务逻辑，比如处理订单、日志等
        System.out.println("Received message: " + msg);
    }
}

```

容器在检测到有消息进来时，会自动调用这个方法来处理消息。


---

3 **Calls can be part of a transaction**

消息的处理过程是可以参与事务的，例如：
- 如果你在 `onMessage()` 方法中访问数据库，并抛出异常，**事务会回滚**，消息也会重新排队（redelivery）。
- 这保证了**消息处理的原子性**（要么成功处理，要么全部回滚）。
- 
容器通过声明式事务管理来支持这一点，无需你手动管理事务。
```
@TransactionAttribute(TransactionAttributeType.REQUIRED)
public void onMessage(Message msg) {
    // 访问数据库
    // 如果出错，事务自动回滚
}

```

### 4.3.1 例子 


好的，下面我将给你一个 **完整的 JMS 示例**，结合：
- 消息类型（TextMessage）
- 如何配置队列和连接工厂
- 如何用 **Message-Driven Bean (MDB)** 来接收消息
- 如何用一个简单的 Java 程序来发送消息

JNDI（**Java Naming and Directory Interface**，Java 命名与目录接口）是 Java 提供的一套 **API，用于访问命名和目录服务**。它的主要作用是：**通过名字查找资源对象**，比如数据库连接池、EJB、JMS 队列等。


`[Java SE Sender] ---> [JMS Queue: TestQueue] ---> [MessageDrivenBean MDB] ---> onMessage()`

- 发送方将消息发送到 JMS 队列
- 容器自动调用 MDB 的 `onMessage()` 方法    
- 可在其中进行数据库写入等操作，并支持事务

----

1 配置连接工厂和队列（以 JBoss/WildFly 为例）

在 Java EE 容器（如 WildFly）中，连接工厂和队列通常通过 JNDI 绑定：

```java
<!-- standalone-full.xml 中的配置 -->
<subsystem xmlns="urn:jboss:domain:messaging-activemq:5.0">
  <server name="default">
    <jms-destinations>
      <jms-queue name="TestQueue">
        <entry name="java:/jms/queue/TestQueue"/>
      </jms-queue>
    </jms-destinations>
  </server>
</subsystem>

```

---

2 接收方：Message-Driven Bean (MDB)

这个类被容器管理，一旦有消息进入队列 `TestQueue`，容器就会自动调用 `onMessage()`。

```java
import javax.ejb.ActivationConfigProperty;
import javax.ejb.MessageDriven;
import javax.jms.*;

@MessageDriven(
    activationConfig = {
        @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue"),
        @ActivationConfigProperty(propertyName = "destinationLookup", propertyValue = "java:/jms/queue/TestQueue")
    }
)

public class MyMessageBean implements MessageListener {

    @Override
    public void onMessage(Message message) {
        try {
            if (message instanceof TextMessage) {
                String text = ((TextMessage) message).getText();
                System.out.println("✅ Received: " + text);
            }
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}

```

---


3 发送方：Java 程序发送消息到 JMS 队列

这个发送程序可以是一个普通的 Java SE 程序，只要配置好 JNDI 和 JMS：

```java
import javax.jms.*;
import javax.naming.InitialContext;
import javax.naming.NamingException;

public class JmsSender {
    public static void main(String[] args) throws Exception {
        InitialContext ctx = new InitialContext(); // 从 jndi.properties 读取连接配置

        Queue queue = (Queue) ctx.lookup("java:/jms/queue/TestQueue");
        ConnectionFactory cf = (ConnectionFactory) ctx.lookup("ConnectionFactory");

        try (Connection conn = cf.createConnection();
             Session session = conn.createSession(false, Session.AUTO_ACKNOWLEDGE);
             MessageProducer producer = session.createProducer(queue)) {

            TextMessage message = session.createTextMessage("Hello from the sender!");
            producer.send(message);
            System.out.println("✅ Message sent.");
        }
    }
}

```


确保你有一个 `jndi.properties` 文件包含类似：
```
java.naming.factory.initial=org.wildfly.naming.client.WildFlyInitialContextFactory
java.naming.provider.url=http-remoting://localhost:8080
```




# 5 Application Client Container

standalone ohne IGB Container, mit Client Container kann ich direkt mit methold 

EJB lookups, dependency injection, and access to JMS are features of the application server.
Application client components ( which is not running on an application server  )do not have direct access and need to install a dedicated application client container (essentially, a number of libs for the build path)

**EJB、依赖注入和 JMS 等 Jakarta EE 功能依赖容器（服务器）提供的环境。如果客户端程序不在容器中运行，就要用 Application Client Container 来“模拟”容器功能。**

1 EJB 查找、依赖注入、JMS 访问是应用服务器的功能

像下面这些功能：

    通过 JNDI 查找 EJB（InitialContext.lookup(...)）

    使用 @EJB 或 @Inject 自动注入 EJB 或 JMS 队列

    向 JMS 队列发送/接收消息（如 @MessageDriven Bean）

👉 这些功能都不是 Java SE 自带的，它们是 应用服务器（Application Server）（如 WildFly, GlassFish, Payara）提供的，运行在 Jakarta EE 容器中。



2 如果你写的客户端程序不是运行在应用服务器中，就无法直接使用这些功能
例如你写了一个普通的 Java SE 应用（比如用 main() 启动的 CLI 客户端）：
```
public class Client {
    public static void main(String[] args) {
        // 想访问一个远程的 EJB 或 JMS 队列
    }
}

```

⚠️ 你无法直接使用 @EJB 或 @Inject，也不能通过 JNDI 查找 EJB 或 JMS 队列。因为这些功能需要应用服务器支持，而你的客户端不在容器中运行。



3 解决方法：安装一个 Application Client Container（ACC）
你需要使用 Jakarta EE 提供的 Application Client Container（ACC），它本质上是一个客户端运行时，包含一堆需要的 Jakarta EE 的 jar 包（比如 jboss-client.jar、jakarta.ejb-api.jar 等），使得：
    你的客户端可以通过 JNDI 查找远程 EJB
    使用容器提供的类加载器、事务管理器等功能

这就像是在本地给你的客户端程序“模拟一个微型的 Jakarta 容器环境”。


## 5.1 例子 


### 5.1.1 

假设你有一个远程部署在服务器上的 EJB：

```
@Stateless
public class CalculatorService {
    public int add(int a, int b) {
        return a + b;
    }
}

```

你想在本地 Java SE 程序中使用它：
```
public class RemoteClient {
    public static void main(String[] args) throws Exception {
        Properties props = new Properties();
        props.put(Context.INITIAL_CONTEXT_FACTORY, "org.wildfly.naming.client.WildFlyInitialContextFactory");
        props.put(Context.PROVIDER_URL, "http-remoting://localhost:8080");

        InitialContext ctx = new InitialContext(props);
        CalculatorService service = (CalculatorService) ctx.lookup("ejb:/MyApp/CalculatorService!com.example.CalculatorService");
        System.out.println(service.add(3, 4));
    }
}

```

这时你必须在 classpath 中添加 **应用服务器提供的客户端库**（比如 WildFly 的 `jboss-client.jar`），否则你连 `InitialContext` 都无法正确使用。


### 5.1.2 



好的，我们继续详细解释并给出一个**完整的示例**，帮助你理解 **在不运行于 Jakarta EE 应用服务器中的客户端程序，如何通过 JNDI 远程访问 EJB** —— 即使用 **Application Client Container（ACC）**。


| 要点       | 内容                                                   |
| -------- | ---------------------------------------------------- |
| EJB 调用方式 | 客户端不能直接构造 EJB，要用 JNDI 或依赖注入                          |
| 客户端访问    | 需要使用 Application Client Container，添加相应 jar 包         |
| 关键 jar   | `jboss-client.jar`（WildFly），或其他 Jakarta EE 实现提供的 jar |
| 用途       | 在不运行于应用服务器的客户端中访问远程 EJB、JMS 等服务                      |


---

你有一个部署在应用服务器（比如 WildFly）上的远程 EJB：
```java
// 服务端代码（部署在 WildFly 上）
package com.example;

import jakarta.ejb.Stateless;

@Stateless
public class CalculatorBean implements CalculatorRemote {
    public int add(int a, int b) {
        return a + b;
    }
}

```

接口（必须是 remote 接口）：
```java
package com.example;

import jakarta.ejb.Remote;

@Remote
public interface CalculatorRemote {
    int add(int a, int b);
}

```

---

客户端调用这个 EJB（不是在服务器中运行）
你想写一个普通 Java SE 程序调用它：
```java
// 客户端代码
package client;

import com.example.CalculatorRemote;

import javax.naming.Context;
import javax.naming.InitialContext;
import java.util.Properties;

public class RemoteClient {
    public static void main(String[] args) throws Exception {
        Properties props = new Properties();
        props.put(Context.INITIAL_CONTEXT_FACTORY, "org.wildfly.naming.client.WildFlyInitialContextFactory");
        props.put(Context.PROVIDER_URL, "http-remoting://localhost:8080");  // 替换为你的 WildFly 地址

        Context context = new InitialContext(props);

        CalculatorRemote calculator = (CalculatorRemote) context.lookup(
            "ejb:/myapp/CalculatorBean!com.example.CalculatorRemote"
        );

        System.out.println("3 + 4 = " + calculator.add(3, 4));
    }
}

```

 JNDI 查找路径格式（WildFly 示例）：
```
ejb:/<deployment-name>/<bean-name>!<fully-qualified-remote-interface>
```

```
`ejb:/myapp/CalculatorBean!com.example.CalculatorRemote`
```

- `myapp` 是你部署的 `.jar` 或 `.ear` 文件名（不带扩展名）
- `CalculatorBean` 是你的 EJB 类名
- `com.example.CalculatorRemote` 是接口全名


---

你需要添加客户端依赖（Application Client Container）

最重要的一步：你的客户端不能直接使用 JNDI，需要引入 **Application Client Container 的依赖 jar 包**。

对于 **WildFly**，你只需在 classpath 中加入：
```
jboss-client.jar

```

这个 jar 位于：

```
<wildfly-home>/bin/client/jboss-client.jar
```

如果用 Maven，也可配置：
```
<dependency>
    <groupId>org.wildfly</groupId>
    <artifactId>wildfly-ejb-client-bom</artifactId>
    <version>30.0.0.Final</version> <!-- 你的 WildFly 版本 -->
    <type>pom</type>
    <scope>import</scope>
</dependency>

```


---

最终结果

运行客户端后会输出：
```
最终结果

运行客户端后会输出：`3 + 4 = 7` 
```

表示客户端成功通过 JNDI 远程调用了部署在 WildFly 上的 EJB。


# 6 JNDI **Java Naming and Directory Interface**

**Java Naming and Directory Interface**，Java 命名与目录接口）是 Java 提供的一套 **API，用于访问命名和目录服务**。它的主要作用是：**通过名字查找资源对象**，比如数据库连接池、EJB、JMS 队列等。

**JNDI 是 Java 中用来“通过名字找资源”的机制**，你可以用它来找数据库、EJB、消息队列等资源，实际操作中常配合 `@Resource` 或 `InitialContext.lookup()` 使用。

JNDI 就像一个“电话簿”或“资源目录”，你可以通过名称（例如 `"java:/MyDataSource"`）查找到对应的对象（例如一个数据库连接池）。

|场景|使用 JNDI 查找的资源|
|---|---|
|数据库|DataSource（连接池）|
|消息服务|JMS 队列 / Topic|
|远程服务|EJB 远程接口|
|配置项|环境变量 / 资源引用|

- **应用服务器**（如 WildFly, TomEE）在启动时会将资源注册到 JNDI 名字空间中。
- 应用程序只需要知道资源的名字，通过名字获取引用。    
- JNDI 统一了不同目录服务的访问方式，支持：
    - 本地命名空间
    - LDAP
    - RMI Registry
    - DNS 等

| 名字空间前缀           | 含义           |
| ---------------- | ------------ |
| `java:comp/env/` | 应用内资源（最常用）   |
| `java:/`         | 全局资源         |
| `java:global/`   | 应用服务器全局范围的资源 |


---

1 示例 1：查找数据库连接池
```java
import javax.naming.InitialContext;
import javax.sql.DataSource;
import java.sql.Connection;

InitialContext ctx = new InitialContext();
DataSource ds = (DataSource) ctx.lookup("java:/MyDataSource");
Connection conn = ds.getConnection();

```

这段代码表示：从 JNDI 目录中查找名为 `"java:/MyDataSource"` 的数据库连接池，并获取一个连接。


---


示例 2：在 Jakarta EE 应用中注入 JNDI 对象 到一个 object 中 

你通常不需要手动用 `lookup()`，只需要使用注解：

```
@Resource(lookup = "java:/MyDataSource")
private DataSource dataSource;

```
容器会在部署时，从 JNDI 中自动注入这个对象。



## 6.1 通过 JNDI 使用数据库连接池来访问 MySQL


| 步骤  | 说明                           |
| --- | ---------------------------- |
| 1   | 在应用服务器中配置 JNDI 数据源           |
| 2   | 在 Java 代码中通过 `@Resource` 注入  |
| 3   | 使用 `getConnection()` 获取数据库连接 |
| 4   | 无需管理连接池，容器全自动处理              |


1  在应用服务器中配置 JNDI 数据源
示例：在 **WildFly** 中配置 JNDI 数据源 `java:/MyDS`
你可以在 `standalone.xml` 中添加如下内容（或用管理界面添加）：

```
<datasource jndi-name="java:/MyDS" pool-name="MyDS" enabled="true" use-java-context="true">
  <connection-url>jdbc:mysql://localhost:3306/mydb</connection-url>
  <driver>mysql</driver>
  <security>
    <user-name>dbuser</user-name>
    <password>dbpassword</password>
  </security>
</datasource>

```

🔧 确保你已将 MySQL JDBC 驱动部署到服务器上。



2 在 Java EE 应用中使用这个 JNDI 数据源

你可以用注解 @Resource 注入 JNDI 数据源。
- 使用 `@Resource(lookup = "java:/MyDS")` 注解注入 JNDI 数据源。
- 不需要手动创建连接池或管理连接。
- 容器（如 WildFly）负责提供并管理这个数据源。
  
```java
import javax.annotation.Resource;
import javax.enterprise.context.RequestScoped;
import javax.inject.Named;
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

@Named
@RequestScoped
public class DbTestBean {

    @Resource(lookup = "java:/MyDS")
    private DataSource dataSource;

    public String getUserCount() {
        try (Connection conn = dataSource.getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT COUNT(*) FROM users")) {

            if (rs.next()) {
                return "User count: " + rs.getInt(1);
            }
        } catch (Exception e) {
            e.printStackTrace();
            return "Error accessing DB.";
        }
        return "No result.";
    }
}

```


3 配合 JSF 页面测试显示结果（可选）

```
<h:form>
  <h:outputText value="#{dbTestBean.userCount}" />
</h:form>
```


## 6.2 通过 JNDI 使用 **JMS 队列**


| 类型     | 使用方式                           | JNDI 名称示例                           | 容器管理 |
| ------ | ------------------------------ | ----------------------------------- | ---- |
| JMS 队列 | `@MessageDriven` 或 `@Resource` | `java:/jms/queue/QueueTest`         | ✅ 是  |
| EJB 服务 | `@EJB` 或显式 JNDI 查找             | `java:global/myapp/GreetingService` | ✅ 是  |

JMS（Java Message Service）是 Java EE 提供的异步消息通信机制，用于构建**事件驱动架构**。

监听一个名为 `jms/QueueTest` 的消息队列，并处理消息。


1 在应用服务器中配置 JMS 队列（以 WildFly 为例）

在 `standalone.xml` 或管理控制台中添加：

```
<jms-queue name="QueueTest" entries="java:/jms/queue/QueueTest"/>
```



2   创建一个 Message-Driven Bean (MDB) 监听队列
☑️ 这是 **事件驱动编程** 的典型示例。容器通过 JNDI 自动将队列注入并绑定消息监听器。
```java
import javax.ejb.ActivationConfigProperty;
import javax.ejb.MessageDriven;
import javax.jms.Message;
import javax.jms.MessageListener;
import javax.jms.TextMessage;

@MessageDriven(
  activationConfig = {
    @ActivationConfigProperty(propertyName = "destinationLookup", propertyValue = "java:/jms/queue/QueueTest"),
    @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue")
  }
)
public class MyQueueListener implements MessageListener {

    @Override
    public void onMessage(Message message) {
        try {
            if (message instanceof TextMessage) {
                String content = ((TextMessage) message).getText();
                System.out.println("Received message: " + content);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```



3 发送消息到这个队列（可选）
```
import javax.annotation.Resource;
import javax.inject.Inject;
import javax.jms.*;

public class SenderBean {

    @Inject
    private JMSContext context;

    @Resource(lookup = "java:/jms/queue/QueueTest")
    private Queue queue;

    public void send(String text) {
        context.createProducer().send(queue, text);
    }
}

```


## 6.3 通过 JNDI 使用 EJB 服务

Enterprise JavaBeans（EJB）是 Java EE 的核心之一，用于处理事务、安全性、并发等。

调用一个远程 EJB 服务或注入本地无状态 EJB。

| 类型     | 使用方式                           | JNDI 名称示例                           | 容器管理 |
| ------ | ------------------------------ | ----------------------------------- | ---- |
| JMS 队列 | `@MessageDriven` 或 `@Resource` | `java:/jms/queue/QueueTest`         | ✅ 是  |
| EJB 服务 | `@EJB` 或显式 JNDI 查找             | `java:global/myapp/GreetingService` | ✅ 是  |


1  创建一个无状态 EJB
```java
import javax.ejb.Stateless;

@Stateless
public class GreetingService {

    public String greet(String name) {
        return "Hello, " + name + "!";
    }
}

```


2 注入并调用 EJB
在同一个模块中使用本地调用：

```java
import javax.ejb.EJB;
import javax.enterprise.context.RequestScoped;
import javax.inject.Named;

@Named
@RequestScoped
public class GreetingClient {

    @EJB
    private GreetingService greetingService;

    public String getMessage() {
        return greetingService.greet("Alice");
    }
}

```


可选：显式 JNDI 查找远程 EJB（例如跨服务器）
```java
InitialContext ctx = new InitialContext();
GreetingService remote = (GreetingService) ctx.lookup("java:global/myapp/GreetingService");
String result = remote.greet("RemoteUser");

```



# 7 大例子 


## 7.1 

整合 **EJB（企业级 Java Bean）** 与 **JAX-RS（Java RESTful 服务）
我们将创建一个简单的示例，演示：
- 使用 JAX-RS 构建 REST API
- 在 REST 服务中注入并调用 EJB
- EJB 执行实际业务逻辑或数据库操作


`[ 客户端 ] → [ JAX-RS REST API ] → [ EJB 业务逻辑 ] → [ 数据库 / 外部系统 ]`




src/
 └── com.example
     ├── rest/
     │   └── MessageResource.java        // JAX-RS 入口
     └── service/
         └── MessageServiceBean.java     // Stateless EJB


1 EJB 业务逻辑组件（Stateless Bean）

```java
package com.example.service;

import javax.ejb.Stateless;

@Stateless
public class MessageServiceBean {
    public String getGreeting(String name) {
        return "Hello, " + name + "! (from EJB)";
    }
}

```


2 JAX-RS 资源类调用 EJB

- `@Inject`：CDI（上下文与依赖注入）注入 Stateless EJB
- EJB 也可以通过 `@EJB` 注入，推荐使用 `@Inject` 以支持更现代的 CDI 语义

```java
package com.example.rest;

import com.example.service.MessageServiceBean;

import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;

@Path("/greet")
@Produces(MediaType.TEXT_PLAIN)
public class MessageResource {

    @Inject
    private MessageServiceBean messageService;

    @GET
    @Path("/{name}")
    public String greet(@PathParam("name") String name) {
        return messageService.getGreeting(name);
    }
    
}

```



3 启用 JAX-RS 应用

```java
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;

@ApplicationPath("/api")
public class RestApp extends Application {
    // Empty – just activates JAX-RS
}

```


调用示例

启动你的 Java EE 容器后，例如部署到 WildFly，访问：
```
http://localhost:8080/your-app-name/api/greet/Alice

```

响应：
```
Hello, Alice! (from EJB)

```


## 7.2 

**JAX-RS + EJB 示例基础上，加入数据库访问（JPA）与事务管理**



构建一个简单的 REST API，用于创建和查询用户：

- 使用 JAX-RS 提供 HTTP 接口
- 使用 EJB 编写业务逻辑并启用事务管理
- 使用 JPA 连接数据库并保存用户数据（持久化）    
- 所有组件都由 Java EE 容器管理

|层级|技术|
|---|---|
|REST API|JAX-RS|
|业务逻辑|Stateless EJB|
|数据访问|JPA + Entity|
|数据库事务|自动（通过 EJB）|
|依赖注入|CDI (`@Inject`)|
|运行环境|WildFly / Payara|

|技术|用途|
|---|---|
|JAX-RS|暴露 REST 接口|
|EJB|编写业务逻辑，自动管理事务|
|JPA|持久化实体类|
|EntityManager|对数据库执行 CRUD 操作|
|`@Inject`|自动注入依赖组件|
|`persistence.xml`|配置数据库连接|

src/
└── com.example
    ├── entity/
    │   └── User.java
    ├── service/
    │   └── UserServiceBean.java
    ├── rest/
    │   └── UserResource.java
    └── RestApp.java


---

1 实体类：User.java

```java
package com.example.entity;

import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Constructors
    public User() {}
    public User(String name) {
        this.name = name;
    }

    // Getters
    public Long getId() { return id; }
    public String getName() { return name; }

    // Setters
    public void setName(String name) { this.name = name; }
}

```


2 业务逻辑类（EJB）：UserServiceBean.java

```java
package com.example.service;

import com.example.entity.User;

import javax.ejb.Stateless;
import javax.persistence.*;
import java.util.List;

@Stateless
public class UserServiceBean {

    @PersistenceContext(unitName = "myPU")
    private EntityManager em;

    public void createUser(String name) {
        User user = new User(name);
        em.persist(user);  // 自动开启事务（由容器控制）
    }

    public List<User> getAllUsers() {
        return em.createQuery("SELECT u FROM User u", User.class).getResultList();
    }
}

```

3 REST API 接口：UserResource.java
```java
package com.example.rest;

import com.example.service.UserServiceBean;
import com.example.entity.User;

import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import java.util.List;

@Path("/users")
@Produces(MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
public class UserResource {

    @Inject
    private UserServiceBean userService;

    @POST
    public void createUser(User user) {
        userService.createUser(user.getName());
    }

    @GET
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}

```



4 启用 REST：RestApp.java
```
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;

@ApplicationPath("/api")
public class RestApp extends Application {
    // nothing needed here
}

```


5 配置 persistence.xml
在 `src/main/resources/META-INF/persistence.xml` 中配置：

```xml
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
                                 http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">

    <persistence-unit name="myPU" transaction-type="JTA">
        <class>com.example.entity.User</class>
        <properties>
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:mem:test;DB_CLOSE_DELAY=-1"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="hibernate.hbm2ddl.auto" value="update"/>
            <property name="hibernate.show_sql" value="true"/>
        </properties>
    </persistence-unit>
</persistence>

```



6 调用接口测试

添加用户：
```
curl -X POST -H "Content-Type: application/json" \
     -d '{"name": "Alice"}' \
     http://localhost:8080/your-app/api/users
```


获取用户列表：
`curl http://localhost:8080/your-app/api/users`



