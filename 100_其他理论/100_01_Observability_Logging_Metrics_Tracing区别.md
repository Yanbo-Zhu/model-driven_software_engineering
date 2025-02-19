
所谓“日志”分为三种：
- logging，通常意义上的日志，比如程序打印到文件或stdout的字符串行。记录了程序运行过程中发生的事件。
    - Statements von den Entwicklern im Quellcode.
    - Beinhalten Status oder -Fehlermeldungen
- metrics，应用程序运行指标，用于测量程序的运行情况。
    - System measurements (cpu, mem, users)
    - Alles, was gemonitored werden kann und als Zahlenreihe ausgedrückt werden kann
- tracing，在微服务架构中应用程序/组件之间的调用链。
    - Beinhalten Informationen über die Route/Ausführung von bestimmten Anfragen
    - Wird in Microservice-Umgebungen häufig genutzt

以error为例：

- logging 可以告诉你error什么时候发生，以及细节信息
- metrics 可以告诉你error发生了多少次
- tracing 可以告诉你这个error的影响面有多大

三者提供的数据不同：

- logging可以 1）提供事件细节；2）部分日志可以被聚合
- metrics可以 1）提供数字，可被聚合；2）告诉数据趋势
- tracing可以 1）提供调用span；2）提供部分事件的部分细节

三者存储的也不同：

- logging，时间戳 + 格式良好的非结构化文本/结构化日志（json）
- metrics，时间戳 + 数字
- tracing，时间戳 + span

三者都是对事件的不同角度的描述，三者互补形成完整的监控系统。


# 1 Which should you choose?

Choosing between logging, tracing, and metrics involves understanding the strengths and limitations of each method and considering the specific needs of your system or application. Here's a framework for making that decision:

- Metrics are ideal for recording event occurrences, item counts, action durations, or reporting the status of resources like CPU and memory.
- Logs are more suited for capturing detailed narratives of events, especially for documenting errors, warnings, or unusual occurrences that may also be highlighted by metrics.
- Traces offer a window into the journey of a request as it moves through various services in a microservices setup, requiring a unique identifier for each trace to ensure traceability.

In most cases, a combination of logging, tracing, and metrics is the best approach, leveraging the strengths of each to provide a comprehensive view of your system's health and performance.

For example, you can detect a problem through metrics, diagnose it with logs and use tracing to understand the flow of requests that led to the issue.



https://betterstack.com/community/guides/observability/logging-metrics-tracing/
# 2 What are events?

Events are anything that is deemed important for business, security, or troubleshooting reasons and can range from user actions, system errors, configuration changes, security breaches, network traffic, performance metrics, to software updates and service statuses.

Some common examples of events include:
- Completing a batch processing job.
- Triggering scheduled tasks or [cron jobs](https://betterstack.com/community/guides/linux/cron-jobs-getting-started/).
- Changes in user permissions or roles.
- Receiving an incoming HTTP request.
- Database transactions like creating, updating, or deleting records.
- Executing a function.
- Registering or authenticating a user.
- Initiating or terminating a background process.
- Writing data to a file.
- Making an HTTP request to some API.

Every event carries its own set of contextual information. For instance, a database query may include details like the query statement, the execution time, the transaction that initiated the query, and the number of affected rows.

Similarly, a user login attempt might capture the user's ID, the time of attempt, the success or failure status, and the device used for the attempt. When a service interacts with an external API, the context might consist of the endpoint URL, the payload sent, the response received, and the duration of the call.

To manage and make sense of this information, three primary strategies are employed: tracing to track the flow of requests through the system; logging to record discrete events or states; and metrics to aggregate numerical data points over time for monitoring and analysis.

These techniques help in condensing the data into more manageable forms, facilitating more effective system oversight and decision-making.

Let's talk about logging first.

# 3 What are logs?

==Logs are an historical record of the various events that occur within a software application, system, or network.== They are chronologically ordered so that they can provide a comprehensive timeline of activities, errors, and incidents, enabling you to understand what happened, when it happened, and, often, why it happened.

They also provide the highest amount of detail regarding individual events in the system because you can selectively record as much context as you want, but this can also be a downside as excessive logging can consume too much resources and hamper the performance of the application.

Historically, logs were unstructured which made them a pain to work with as you'll typically have to write custom parsing logic to extract the information you're interesting in.

```text
127.0.0.1 alice Alice [06/May/2021:11:26:42 +0200] "GET / HTTP/1.1" 200 3477
```

Nowadays, though, logging has become more sophisticated with structured formats like JSON at its fore front which trades of some verboseness and storage space for consistent and unambiguous parsing that works everywhere JSON is supported.

```json
{
  "ip_address": "127.0.0.1",
  "user_identifier": "alice",
  "user_authentication": "Alice",
  "timestamp": "06/May/2021:11:26:42 +0200",
  "request_method": "GET",
  "request_url": "/",
  "protocol": "HTTP/1.1",
  "status_code": 200,
  "response_size": 3477
}
```

Applications generate a variety of logs, each serving a distinct purpose, including but not limited to:

- **Debug logs**: Designed specifically for troubleshooting, these logs provide extensive detail to facilitate quick resolution of issues. Given their voluminous nature, debug logs are typically generated in limited scenarios, such as during a debugging session, rather than continuously, to avoid overwhelming storage and processing capacities.
    
- **Error logs**: These logs document the occurrence of unexpected errors within the application's operations. They capture critical information about the error condition, such as the type of error, the location within the application where it occurred, and a timestamp. This helps in diagnosing issues and preventing future occurrences.
    
- **Audit logs**: These logs track and record sequential actions or events, offering a transparent trail of activities including user actions, system changes, or data access, making them invaluable for compliance and security.
    
- **Application logs**: These are records of significant events at the application level, encompassing business transactions, state changes, and other events critical to the business logic of the application. They're pivotal for understanding the application's behavior in the context of business operations.
    
- **Security logs**: Focused on security-related events, these logs are essential for detecting, understanding, and responding to security incidents. They record login attempts, access control activities, and any other actions that could affect the application's security posture.
    

Note that all logs should not be treated the same way. For example, audit and security logs may be subject to regulatory requirements dictating how long they must be retained. In contrast, debug logs are purged more frequently to conserve storage resources.


# 4 What are metrics?

==Metrics focus on aggregating numerical data over time from various events, intentionally omitting detailed context to maintain manageable levels of resource consumption. ==

For instance, metrics can include the following:
- Number of HTTP requests processed.
- The total time spent processing requests.
- The number of requests being handled concurrently.
- The number of errors encountered.
- CPU, memory, and disk usage.

Context is not entirely disregarded in metrics as its often useful to differentiate metrics by some property, although this requires careful consideration to avoid high cardinality.

High-cardinality data, like email addresses or transaction IDs, are typically avoided in metrics due to their vast and unpredictable range but low-cardinality data such as Boolean values, response time buckets are much more manageable.

Metrics are particularly useful for pinpointing performance bottlenecks within an application's subsystems by tracking latency and throughput, which logs, given their detailed and context-rich nature, might struggle to do efficiently.

Once a potential issue is identified through metrics, logs can then provide the detailed context needed to drill down into specific problematic requests or events.

This illustrates the complementary roles of logs and metrics: metrics offer a high-level overview with limited context, ideal for monitoring and performance analysis across broad system components, while logs provide deep, contextual details for thorough investigation of specific events.

Balancing the use of both allows for effective system monitoring and debugging while keeping data processing demands in check.


# 5 What is tracing?

In tracing, not every single event is scrutinized; instead, a selective approach is employed, focusing on specific events or transactions, such as every hundredth one that traverses designated functions. This selective observation records the execution path and duration of these functions, offering insights into the program's operational efficiency and identifying latency sources.

Some tracing methodologies extend beyond capturing mere snapshots at critical junctures to meticulously tracking and timing every subordinate function call emanating from a targeted function.

For instance, by sampling a fraction of user HTTP requests, it becomes possible to analyze the time allocations for interactions with underlying systems like databases and caches, highlighting the impact of various scenarios like cache hits and misses on performance.

Distributed tracing advances this concept by facilitating traceability across multiple processes and services. It employs unique identifiers for requests that traverse through remote procedure calls (RPCs), enabling the aggregation of trace data from disparate processes and servers.

A distributed trace consists of several spans, where each span acts as the fundamental element, capturing a specific operation within a distributed system. This operation might range from an HTTP request to a database call or the processing of a message from a queue.

Tracing provides insight into the origin and nature of issues by revealing:

- The specific function involved.
- The duration of the function's execution.
- The parameters that were passed to it.
- The extent of the function's execution reached by the user.
