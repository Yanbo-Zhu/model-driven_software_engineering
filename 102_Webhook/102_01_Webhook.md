
https://www.redhat.com/zh-cn/topics/automation/what-is-a-webhook


Webhook 是一种事件驱动的轻量级通信，可通过 HTTP 在应用之间自动发送数据。Webhook 由特定事件触发，可自动实现 [应用编程接口（API）](https://www.redhat.com/zh-cn/topics/api/what-are-application-programming-interfaces)之间的通信，并可用于激活工作流，例如在  [GitOps](https://www.redhat.com/zh-cn/topics/devops/what-is-gitops)  环境中。

Webhook 可以将事件源连接到自动化解决方案，因此，它可以用来启动[事件驱动型自动化](https://www.redhat.com/zh-cn/topics/automation-and-management/shenmeshishijianqudongzidonghua)以便在发生特定事件时执行各种 IT 操作。


# 1 什么是 API？

API 由一组用于构建和集成应用软件的定义和协议组合而成。有时我们可以将 API 之间的通信视为信息用户和信息提供者之间的合约，它规定了消费者（调用方）需要提供的内容和提供者（响应方）需要提供的内容。这种关系也可描述为_客户端_应用调用_服务器_应用，但这两个角色可以互换，具体取决于特定情景中的哪个应用在请求数据。  

Web API 通常使用 HTTP 从其他应用请求数据，并定义响应报文的结构，其通常采用 XML 或 JSON 文件格式。XML 和 JSON 均为首选格式，因为二者都以结构化方式呈现数据，易于其他应用处理。 

当客户端 API 从服务器 API 请求数据时，它会通过调用来了解是否发生了特定的事件，即服务器的数据是否发生了可能对客户端有用的变化。在这个过程（称为_轮询_）中，客户端以固定间隔发送 HTTP 请求，直到服务器 API 发送相关的数据，此数据有时称为_有效负载（Payload）_。 

==客户端应用并不知道服务器应用的状态，因此会轮询服务器 API 来获取更新，不断调用到发生特定事件为止，但服务器只会在信息可用时发送所请求数据。客户端应用必须一直请求更新，并等待到相关事件发生为止。==


# 2 Webhook和 API 有什么区别？

要设置 Webhook，客户端需向服务器 API 提供唯一的 URL，并指定其要了解的事件。==设置 Webhook 后，客户端不再需要轮询服务器。发生特定的事件时，服务器会自动将相关的有效负载发送到客户端的 Webhook URL。== 

==Webhook 通常被称为_“反向 API”_ 或_“推送 API”_，因为通信责任落在了服务器而非客户端上。与客户端不断发送 HTTP 请求来请求数据，直至服务器作出响应的方式不同，服务器在数据可用时会立即向客户端发送一个 HTTP POST 请求。虽然有这些别名，但其实 Webhook 并非 API，二者可以结合使用。应用必须具有 API 才能使用 Webhook。== 

_Webhook_ 这一名称取自 _Web_（表明指的是基于 HTTP 的通信）与 _hooking_ 编程函数（允许应用截获调用或可能感兴趣的其他事件）。Webhook 可“钩住”服务器应用上发生的事件，并提示服务器通过 Web 将有效负载发送到客户端。Jeff Lindsay 在 2007 年发表的一篇名为[《Webhook 革新 Web 世界》（Web hooks to revolutionize the web）的博客文章推动了这一概念的普及。](https://progrium.github.io/blog/2007/05/03/web-hooks-to-revolutionize-the-web/)

IT 团队可以利用多种方式来保护通过 Webhook 通信的应用。大多数支持 Webhook 的应用都会将一个机密密钥添加至有效负载的请求标头中，以便客户端确认服务器的身份。Webhook 通常由相互传输层安全（mTLS）身份验证提供保护，这种方式会在发送有效负载之前对客户端和服务器进行验证。此外，客户端应用往往将 SSL 加密用于 Webhook URL，确保传输的数据保持私密。

Webhook： 

- **无需轮询：**这可为客户端应用节省资源。
- **易于设置：**如果某款应用支持 Webhook，便可通过服务器应用的用户界面轻松对其进行设置。在这里，客户端输入其应用的 webhook URL ，并设置一些基本参数，例如它们对哪些事件感兴趣。
- **自动传输数据：**服务器应用上发生特定事件时会立即发送有效负载。这种数据交换是由事件触发的，因此发生速度与数据从服务器传输到客户端一样快，实时性可以媲美任何数据传输。
- **适合特定的轻量级有效负载：**Webhook 依靠服务器来确定所发送的数据量，让客户端去解译有效负载并以高效的方式加以使用。由于客户端不控制数据传输的确切时间或大小，webhook 能够处理 2 个端点之间的少量信息，这些信息通常采用通知形式。


# 3 Webhook 用于基础架构的自动化

Webhook 最常用于简化两个应用之间的通信，但也可用于自动化基础架构即代码（IaC）工作流以及启用 GitOps 实践。

## 3.1 什么是基础架构即代码（IaC）？ 

[基础架构即代码（IaC）](https://www.redhat.com/zh-cn/zh-cn/topics/automation/what-is-infrastructure-as-code-iac)是通过代码（而非手动流程）来管理和置备基础架构的方法。版本控制是 IaC 的重要组成部分，配置文件应像任何其他软件源代码文件一样处于源代码的控制之下。以基础架构即代码方式部署还意味着基础架构可被划分为若干模块化组件，然后通过自动化以不同的方式进行组合。

借助 IaC 实现[基础架构置备](https://www.redhat.com/zh-cn/zh-cn/topics/automation/what-is-provisioning)的自动化，意味着开发人员无需再在每次开发或部署应用时手动置备和管理服务器、操作系统、存储及其他基础架构组件。将基础架构的配置和部署以代码形式定义下来，可供置备时有模板可循。这一过程仍然可以手动完成，但也可以利用企业级预期状态引擎（如[红帽® Ansible® 自动化平台](https://www.redhat.com/zh-cn/technologies/management/ansible)）实现流程的自动化。

## 3.2 什么是 GitOps？

[GitOps](https://www.redhat.com/zh-cn/zh-cn/topics/devops/what-is-gitops) 通常被认为是 IaC 的演变，它是一种通过 Git（一种开源版本控制系统）来管理基础架构和应用配置的战略方法。遵循 GitOps 实践时，开发人员将 Git 用作声明性基础架构和应用的单一数据源，并通过 Git 来拉取请求以实现基础架构置备和部署的自动管理。Git 存储库包含系统的完整状态，因此，更改记录既可查看也可审计。 

## 3.3 Webhook 发挥着什么样的作用？

Webhook 可减少实施和管理以 Git 为中心的部署管道所需的步骤，且可用于自动启动整个 IaC 工作流。在 GitOps 环境中，Git 存储库充当可信来源，Webhook 发挥的作用与它在 2 个应用之间所起到的作用相同。被特定事件触发后，某个 API 会将有效负载发送至另一个 API。不同之处在于触发 Webhook 的事件类型以及接收方如何处理有效负载。

在这种情况下，Git 存储库充当的是服务器应用的角色，而预期状态引擎（负责管理基础架构的状态）则充当客户端应用。Webhook 可用于在每当存储库发生更改时，向预期状态引擎发出通知。==如果某段代码在更新后被推送到存储库，该事件便会触发 Webhook。接着，存储库自动将有效负载发送到预期状态引擎的 Webhook 地址，通知代码已更改==。

如果预期状态引擎支持自动化，这些 Webhook 还可以启动 IaC 工作流以将代码更改转化为自动化操作。例如，系统管理员可以设置每当收到 Webhook 的有效负载时都运行的自动化操作，从而自动在其托管的主机上应用新的代码更改，然后将其恢复到默认状态。这种通过 Webhook 触发自动化的方法可以延伸至其他 IT 操作而无需人工干预，进而实现事件驱动型自动化流程。

唯一的不同之处在于可信来源，Webhook 可以将预期状态引擎连接到第三方工具，以监控特定事件的来源，而非将预期状态引擎连接到依靠人工来推送代码更新的 Git 存储库。一旦这些事件源检测到目标事件并触发 Webhook，有效负载便可以启动自动化，在一天中的任何时间都可以立即执行操作来响应事件，无需 IT 人员执行任何操作。

[了解 Ansible 自动化平台如何利用 Webhook 来支持 GitOps 实践](https://www.redhat.com/sysadmin/ansible-webhooks-gitops "将 Ansible 自动化 Webhook 用于 GitOps")

# 4 Webhook 用于事件驱动型自动化

[事件驱动型自动化](https://www.redhat.com/zh-cn/topics/automation-and-management/shenmeshishijianqudongzidonghua)指的是对 IT 环境中不断变化的条件做出自动响应的过程，目的是加快问题解决速度并减少日常的重复性任务。借助事件驱动型自动化，IT 团队可以对有关任何事件（如硬件问题、分布式拒绝服务（DDoS）攻击、内存不足或应用故障）的响应进行编码，从而在事件发生时自动执行必要的操作。

事件驱动型解决方案依靠第三方工具或插件（如 ServiceNow、Kafka、Prometheus、Sensu、Dynatrace 和 Appdynamics）来监控事件源。Webhook 可用于将这些事件源连接到自动化平台，事件源检测到事件时，Webhook 将触发相应的自动响应。

IT 团队可以逐步采用事件驱动型自动化以缩短平均解决时间（MTTR），还可执行仍然需要人工参与的功能（例如自动创建工单），然后逐步实现全自动修复，以便在出现特定问题时自动执行相应操作。

# 5 红帽能做些什么？

[红帽 Ansible 自动化平台](https://www.redhat.com/zh-cn/technologies/management/ansible)是一个端到端自动化平台，旨在帮助 IT 团队在整个企业中创建、管理和扩展自动化。您可以用 Webhook 通过 [GitHub](https://github.com/) 或 [GitLab](https://about.gitlab.com/) 等服务将 Ansible 自动化平台与 Git 存储库集成，为 IaC 和 GitOps 实践提供支持。设置存储库链接后，Ansible 自动化平台便会从 Git 系统中捕获 Git 提交，并使用这些事件来触发自动化作业以更新项目、管理清单以及执行部署。

您可以通过 Webhook 在源代码控制系统中发生事件时自动激活自动化。这样一来，便不再需要 Jenkins 等其他 CI/CD 工具来监控存储库并在变更的同时启动自动化作业，不仅简化 GitOps 工作流，也精简了运维。Ansible 自动化平台可与各种开发和部署工具结合使用，因此，您可以使用偏好的工具和流程来定制 GitOps 工作流。

# 6 **通过事件驱动的 Ansible 实现任务关键型自动化** 

作为 Ansible 自动化平台的一部分，[事件驱动的 Ansible](https://www.redhat.com/zh-cn/technologies/management/ansible/event-driven-ansible) 提供的事件处理功能可在任何 IT 领域中自动执行耗时的任务以及响应不断变化的条件。它可以处理包含有关 IT 环境条件中离散智能的事件，判断出最佳的应对措施，然后自动执行操作以解决或修复事件。

事件驱动的 Ansible 可用于自动执行 IT 服务管理任务（例如工单增强、修复和用户管理）以及各种其他 IT 流程。它可按照_规则_将事件_源_与相应的_操作_连接起来。[Ansible Rulebook](https://www.redhat.com/zh-cn/topics/automation-and-management/shenmeshi-ansible-rulebook) 会定义事件源，并以条件指令“如果满足某个条件，就自动触发（if-this-then-that）”的形式解释事件发生时要执行的操作。这意味着当特定事件发生时，事件驱动的 Ansible 会根据设计的 Rulebook 识别出事件，并自动执行与该事件匹配的操作。

您可以使用享受全面支持的通用 Webhook 将事件驱动的 Ansible 连接到事件源，不过，事件驱动的 Ansible 还提供了一个由合作伙伴为其特定技术构建的[源插件生态系统库](https://catalog.redhat.com/)。您可以使用这些享受全面支持的插件构建事件驱动型自动化，无需为每个新事件编写代码或 Webhook。您只需确定要关注的事件以及希望执行的操作，然后将这些指令写入 Ansible Rulebook，之后，只要发生这类事件，就能触发自动执行您预设的现有 [Ansible Playbook](https://www.redhat.com/zh-cn/topics/automation-and-management/shenmeshi-ansible-playbook) 或自动化工作流。


