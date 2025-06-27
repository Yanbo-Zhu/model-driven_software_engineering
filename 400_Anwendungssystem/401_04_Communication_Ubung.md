

# 1 Distributed System 



Was ist Distributed System 
Trankstion auf meherer Server verteilen 
mehrere Host, die physisch getrennt ist 




Welche Problem haben wir in Distributed Systems 
Fehler Toleranz,  wenn einen server kaputt ist, wie schafft man datan konsistenz , trade off cost 



# 2 Konzept von kommunikation in Distributed Systems



Socket


Komunuzieren Sockets synchron oder asynchron 
asynchron 



## 2.1 synchrone Komunucationsystem
![[400_Anwendungssystem/image/b007b9e15c3bb4a048790ada1e01094.jpg]]
Client blocket, ins Idle Zeit eingeben, warte auf response aus Server 

---


Vor und Nachteile hat syncharone Komunication in Client Server? 

in Welchen Anwendungsfallen wurdet ihre synchrone Komunikation verwending :  bank, geld transition,    . Ticket buchung system: Ticket reserversation 


----


Systemdeisgn mit synchroner Komunication brechen, wenn Komponent nicht mehr erreichbar ist? 

CLient lange warten , ganz blockiert, nicht good -> Time out setzen 



---

Synchrone Kommunication aus die Skalierbarkeit eines Systemes sich auswirken ? 

Synchrone Kommunikation bedeutet, dass ein Systemteil (z. B. ein Service) auf die Antwort eines anderen warten muss, bevor es weiterarbeiten kann.

Synchrone Kommunikation kann die **Skalierbarkeit eines Systems** deutlich beeinflussen – meistens **negativ**. 

Auswirkungen auf die Skalierbarkeit

 **Erhöhte Kopplung**
- Dienste hängen voneinander ab: Wenn Service B ausfällt oder langsam ist, blockiert auch Service A.
- Das verhindert unabhängiges Skalieren.
    
**Ressourcenblockade**
- Threads oder Verbindungen bleiben während der Wartezeit belegt.
- Das begrenzt die Anzahl gleichzeitiger Nutzer oder Anfragen.
    

 **Weniger Fehlertoleranz**
- Bei einem Ausfall eines abhängigen Systems schlägt die gesamte Kette fehl.
- Ein skalierbares System sollte robust gegen Ausfälle sein.
    

 **Schlechtere Antwortzeiten**
- Antwortzeiten addieren sich: 100 ms (Service A) + 200 ms (Service B) = 300 ms insgesamt.
- Unter Last verlängert sich das weiter → schlechteres Nutzererlebnis.

**Skalierung ist komplexer**
- Um Leistung zu steigern, müssen mehrere Systeme gleichzeitig skaliert werden.
- Das erhöht Infrastruktur- und Wartungskosten.

## 2.2 Asynchrone Kommunikation System 

Vor und Nachteile hast asynchrone Kommunikationsystem 

Wann sollte wir asynchrone Kommunication eher als synchrone Kummnikation verwenden 


Wie verandert sich die Verantwortung einer einzelnen komponente in einem asynchronen System 
in syncrhonene Sytsem: eine ein komponent kaputt, dann alle system kaputt

Asynchrone Kommunication aus die Skalierbarkeit eines Systemes sich auswirken ? 
Positive , 



Welche Teile der kommunkation wurde WhastsApp synchrone welche asyncrhone 

asycrhone: telefonieren
sychrone : groupen chatten 


![[400_Anwendungssystem/image/Pasted image 20250626103622.png]]

# 3 RPC 

ermoglichen den Aufruf von Funktionen auf entfernten Comutern /programm 

![[400_Anwendungssystem/image/2eed57dca3a3a2f3cc8929894e44782.jpg]]


RPC
Stellen Sie sich vor, Sie sind Teil eines Dev-Teams in einem großen Unternehmen. Um von Ihrem Anwen-
dungssystem Anfragen an das System eines anderen Teams im Unternehmen stellen zu k¨onnen, entscheiden
Sie sich dazu, Remote Procedure Calls (RPCs) zu benutzen.
a) Was sind Gr¨unde daf¨ur und dagegen, sich in diesem Fall f¨ur RPC zu entscheiden?
b) Welche organisatorischen- und implementierungs-Schritte k¨onnte es geben, bis die Kommunikation
zwischen den beiden Systemen funktioniert

L¨osung:
a)
Daf¨ur:
• Vereinfachte Implementierung im Vergleich zu manueller Implementierung mit Sockets
• Andere Teams k¨onnen die gleichen RPC Calls einfach auch benutzen
• Plattformunabh¨angig. Wenn sich ein Team spontan dazu entscheidet, alles in einer anderen Program-
miersprache neu zu implementieren, ist das f¨ur das andere Team egal.

Dagegen:
• nur synchrone Kommunikation (meistens)
• die Calls ”wirken“ im Code so, als ob sie lokal w¨aren, sind es aber nicht. Das kann zu unerwartet
langsamem Code f¨uhren

b) Beispielsweise:
1. Zuerst m¨ussen sich die Teams auf eine RPC-Technologie einigen, die die jeweils benutzten Program-
miersprachen unterst¨utzt (z.B. GRPC)
2. Dann m¨ussen sich die Teams auf ein Interface einigen, dass sie in einer IDL (Interface Definition Lan-
guage) beschreiben. Wie genau dieses Interface aussieht h¨angt von den bereits bestehenden Systemen
und dem aktuellen Use-Case a

---
## 3.1 RPC Data Flow
Ein Client ruft per RPC eine Prozedur auf einem Server auf. Beschreiben Sie, welche Komponenten auf
dem Server und Client benutzt werden, um diesen Aufruf auszuf¨uhren, und wie diese zusammenspielen.


L¨osung:
1. Die Client-Applikation ruft lokal eine Funktion auf, die von dem RPC-Tool generiert worden ist.
2. Diese Funktion baut eine Netzwerkverbindung zum Server auf und sendet den codierten Funktions-
aufruf an diesen Server.
3. Auf dem Server empf¨angt ein RPC-Server den Aufruf und entpackt die Parameter der aufgerufenen
Funktion.
4. Der RPC-Server ruft dann die dazugeh¨orige Funktion der Applikation auf.
5. Das Ergebnis wird auf die gleiche Weise an den Client zur¨uckgegeben.

![[400_Anwendungssystem/image/Pasted image 20250626110148.png]]



# 4 gRPC

![[400_Anwendungssystem/image/9a6eaa31b5e8a6dfc7848a33fb889d8.jpg]]


## 4.1 Beispielcode 

echo.proto 
![[400_Anwendungssystem/image/d85fa2d71990d0093689bc86a9cead8.jpg]]

写完上面

后通过 gradle build 产生两个class , 如下 
EchoServiceGrpc.java 


![[400_Anwendungssystem/image/4e53260cb9571290ba414eb1af58269.jpg]]



---

EchoServiceImpl.java
Server aufbauen 
![[400_Anwendungssystem/image/12a5e309f104dd4a3e86e50c7d239c4.jpg]]

![[400_Anwendungssystem/image/5e10166de7379d1f1d201232b04272e.jpg]]

![[400_Anwendungssystem/image/e2c9c84717eedfded91b2c9f0deae49.jpg]]

记得去 start Server 通过 run this classs file 

---

EchoServiceClient.java 
Client aufbauen
![[400_Anwendungssystem/image/7c7030cf5134b079644454fdfd15fce.jpg]]


![[400_Anwendungssystem/image/d4e66ce82264408a68fcba3eec0a22d.jpg]]

![[400_Anwendungssystem/image/361af196887b347cd9480f8d5f3e82a.jpg]]


# 5 Message Queues 

![[400_Anwendungssystem/image/a3d9c7ab4be2e3528f432593d6dbbb5.jpg]]


![[400_Anwendungssystem/image/d72d6ef473f9ec94d2fa4c5236dc903.jpg]]

![[400_Anwendungssystem/image/15bb94207519b1fa652af4134044d7f.jpg]]

![[400_Anwendungssystem/image/406f1ca877c706d47c9a4c7aa63694a.jpg]]



# 6 How to send and deliver messages

While end-to-end communication (A) may be asynchronous, this doesn‘t say
anything about the way in which B and C are realized and how they affect
program flow at sender/recipient.
Both B and C can be either synchronous or asynchronous

![[400_Anwendungssystem/image/Pasted image 20250626103521.png]]


---

Sending to the queue synchronously

In synchronous sending, the client will make a
blocking sendMessage(msg) call.
Advantage: The sender can be sure that the
message has been recorded by the queue.
Reliable messaging asserts eventual delivery.
Disadvantage: The sender encounters the usual
delay of a blocking RPC call

![[400_Anwendungssystem/image/Pasted image 20250626103547.png]]

---

Sending to the queue asynchronously

In asynchronous sending, the client will use a
separate thread to make the blocking
sendMessage(msg) call.

Advantage: Latency is minimal.

Disadvantage: When continuing processing, the
client cannot know about the success of the
operation (only later).
Often implemented using thread pools inside
the messaging client library.

![[400_Anwendungssystem/image/Pasted image 20250626103714.png]]

---

Delivering messages asynchronously

The messaging system is responsible for
delivering messages to the recipient. Usually, the
recipient registers a callback endpoint. Afterwards,
the queue will try to call the endpoint whenever a message needs to be delivered (=> observer
pattern).

Advantage: The recipient is not blocked and idle
waiting/active polling are avoided.

Disadvantage: The recipient can get overloaded,
messages might, as a mitigation mechanism, be
queued again on the recipient (=> increases
latency).

---


Messaging is the standard mechanism to realize asynchronous communication and loose
coupling.
In contrast to the parameters of RPC calls, messages tend to be self-explaining documents.
We can send messages to a messaging system synchronously and asynchronously; messages
can be delivered synchronously (active polling) or asynchronously (observer/listener, callbacks).
⇒ All options are used.
⇒ What is best depends on the application use case.




# 7 Middleware - paradigmen 


Welcbe Art der kommunikation wurdet Ihr wahlen wenn euer System, highly availbale sein soll? 

Ayschronoe Komunikation,  
Es should never akzpt, server kapuut 



Wie wirkt sich die Wahl von RPC oder MQ auf die sklierbarkeit des Systems aus 


# 8 Queues

Wof¨ur k¨onnen Queues in komplexen System eingesetzt werden? Erkl¨aren Sie die Unterschiede, Nachrichten
synchron oder asynchron in die Queue einzuliefern und abzuhole

load balancer 中使用 queue 

L¨osung: 
Queues k¨onnen unter anderem eingesetzt werden, um ==Komponenten voneinander zu entkoppeln==.
Damit k¨onnen diese Komponenten z.B. unterschiedliche Technologien verwenden. Außerdem kann das
empfangende System die Anfragen zu einem sp¨ateren Zeitpunkt bearbeiten. Damit kann die Reliability erh¨oht werden.

Die Kommunikation insgesamt ist asynchron, aber einzelne Teile der Kommunikation mit der Queue k¨onnen trotzdem synchron sein, woraus sich folgende Matrix ergibt: 
![[400_Anwendungssystem/image/Pasted image 20250618112220.png]]


Nachteil  in Asyncrhoen mode of queue 
wenn queue leer ist, Empfanger fragt immer quere an, ob neue Nachrichten kommt -> uberlastet 

wenn queue voll  ist, Empfanger wird uberlastet 






# 9 Losse Coupling

Coupling =  Abhangigkeit zwischen 2 komponenten in einem system 


Coupling = Measure of dependencies between application components:
• Technology dependency
• Location dependency
• Temporal dependency
• Data format dependency
⇒ In general, we always want to minimize coupling ⇒ loose coupling.

Lose gekoppelt impliziert Austauschbarkeit von Komponenten und auch eine insgesamt h¨ohere Verf¨ugbarkeit durch graceful degradation. Auch k¨onnen Komponenten leichter migriert oder skaliert werden, da Kommunikation nur noch an einen logischen Endpoint und nicht an eine physische Komponente gerichtet wird.





Wie schaffst , so wenig Coupling zu machen : Druch Kafka oder another Mittelware , durch, die alle Traffic verwalten kann 


Auf welche Ebene kann es Abhangigkeiten zwischen Kompinenten geben 


![[400_Anwendungssystem/image/cb50aec2bc716ae4666603314398599.jpg]]


# 10 Pub/Sub




![[400_Anwendungssystem/image/4c685028152f95a27312531492b4c8c.jpg]]



Wie unterscheidet sich pub/sub von Point-to-Point-Komunikation (RPC)



Wie wirk such pub/sub auf das Counpling von Komponenten aus 
realisieren Entcouplen 


Wie kann man als Publisher sicherstellen das ein Subscriber die Nachricht bekommen hat 


在 **Pub/Sub（发布-订阅）架构**中，常见的 **消息中间件（Message Broker）** 有以下几种。它们被广泛用于微服务通信、事件驱动架构、异步处理等场景。

| 名称                           | 类型                  | 语言生态          | 特点                            | 使用场景         |
| ---------------------------- | ------------------- | ------------- | ----------------------------- | ------------ |
| **Apache Kafka**             | 分布式日志系统 / 流平台       | Java (多语言客户端) | 高吞吐、可持久化、支持流处理（Kafka Streams） | 大数据、微服务、日志系统 |
| **RabbitMQ**                 | 传统消息队列（AMQP）        | Erlang        | 灵活的路由、多种消息协议、低延迟              | 企业应用、微服务     |
| **Redis Pub/Sub**            | 内存数据库内建的 Pub/Sub    | 多语言           | 超快但不持久、不可靠                    | 实时通知、缓存驱动的事件 |
| **NATS**                     | 轻量级消息系统             | Go            | 极简、低延迟、适合边缘计算                 | IoT、容器通信     |
| **Google Pub/Sub**           | 托管云服务               | 多语言           | 弹性自动伸缩、集成 BigQuery            | GCP 原生项目     |
| **Amazon SNS/SQS**           | 托管云服务               | 多语言           | SNS = Pub/Sub 推送，SQS = 队列拉取   | AWS 微服务通信    |
| **MQTT Broker（如 Mosquitto）** | 专为物联网设计的 Pub/Sub 协议 | 多语言           | 小巧、轻量级、适合不稳定网络                | IoT设备、嵌入式系统  |

## 10.1 Brokerless und broker-based Pub/Sub Systemen


L¨osung: In Pub/Sub Systemen kann eine Nachricht mehrere Empf¨anger haben. Sender (sog. Publis-
her) ver¨offentlichen Nachrichten und geben zu jeder Nachricht zus¨atzlich das Topic dieser Nachricht an.
Empf¨anger (Subscriber) k¨onnen festlegen, welche Topics f¨ur sie relevant sind und erhalten nur diese Nach-
richten. Bei einem broker-based System interagieren die Publisher und Subscriber nicht direkt miteinander,
sondern benutzen eine zentrale Communication Middleware (Broker). In brokerless Systemen interagieren
die Publisher und Subscriber direkt miteinander und tauschen die Nachrichten ohne zentrale Instanz aus.
Vorteil von broker-based Systemen ist, dass diese f¨ur Clients einfacher (und ressourcensparender) zu ver-
wenden sind: Publisher m¨ussen z.B. nicht wissen, wo die Subscriber sind. Ein zentrales System k¨onnte
aber bei hoher Last ¨uberlastet werden. Brokerless Systeme k¨onnen skalierbarer sein, da es keine zentrale
Komponente gibt. Allerdings ist der Aufwand f¨ur Clients und der Communication Overhead h¨oher.

## 10.2 Interaction patterns

So far, we assumed that one sender sends a message to one recipient via a shared channel.

A common pattern, however, is to use a single shared channel for multiple senders and
recipients.

![[400_Anwendungssystem/image/Pasted image 20250626104038.png]]

Messages are processed by only one server in each stage but each pool of workers can be
managed (scaled, …) independently


While multiple servers might use the same shared channel, this is still so-called Point-to-Point
Messaging (PTP). We still have 1:1 communication (or n:1) as a message is only processed by
one recipient.
Sometimes, we need to have messages processed by several recipients (1:m or n:m
communication). Examples:
• Announcing an updated room to all students in AS
• Broadcasting TV or radio signals
• Distributing sensor data (e.g., temperature) to all interested parties
• …
This interaction pattern is usually refered to as publish/subscribe (pub/sub).

## 10.3 Pub/sub

In pub/sub, senders are called publishers (who publish messages/events), recipients are called
subscribers.
For two-way communication, systems/servers/components usually act as both publishers and
subscribers.
As in PTP messaging, pub/sub can also be implemented with a remote middleware/service
(broker-based) or distributed through local middleware components (brokerless).
Examples: ZeroMQ (brokerless), Apache Kafka (broker-based)
Let‘s focus on broker-based pub/sub (brokerless works very similar)…


----

Message delivery in (broker-based) pub/sub

![[400_Anwendungssystem/image/Pasted image 20250626104612.png]]


Messages from the publishers are delivered to the subscribers.
But: not all potential recipients might be interested in that message.
⇒ Recipients need a way to specify what they are interested in
⇒ Brokers need to run message matching/event filtering based on those interests

In practice, most pub/sub systems are topic-based (content-based or channel-based would be the alternatives).

---

Topic-based pub/sub

In topic-based pub/sub, recipients subscribe to a topic X (=> subscribers of topic X)
When a sender publishes a message/event Y to topic X (=> publisher of Y), all subscribers of
topic X will receive Y.
Disadvantage: Usually, only the subscribers that can be reached at the time of publishing will
receive the message (some systems buffer messages for a while but delivery is usually not guaranteed due to the potential number of
subscribers and the sheer volume of messages).
Advantages: Scalability, loose coupling
Topics can often be multi-level, e.g., /sensors/berlin/temperature or /sensors/*/temperature



---

Inter-broker routing


In distributed setups, we might have multiple interconnected brokers and clients will connect to the nearest broker.

Sending all messages and subscriptions to all brokers is inefficient.
⇒ We have to worry about inter-broker routing strategies
⇒ Join us in Fog Computing in your master‘s if you want to learn more about those…

![[400_Anwendungssystem/image/Pasted image 20250626104717.png]]


---

Summary

PTP messaging:
• 1:1
• Usually reliable message delivery
• Often used in fan-out-fan-in patterns

Pub/sub:
• n:m
• Messages usually delivered only to those already subscribed and reachable
• Broker-based or broker-less
• Often topic-based




# 11 Anwendungsszenarien
¨Uberlegen Sie sich f¨ur folgende Anwendungsszenarien, welche Art der Kommunikation zwischen den Kom-
ponenten am besten geeignet ist:
1. Zwei Systeme. Eines produziert Events, das andere verschickt diese Events als PushNotification an User.
2. Banken tauschen Finanztransaktionen aus.
3. Verteilung von Aktienkursen an mehrere Clients.
4. Eine Instant-Messaging App, bei der 2 User miteinander schreiben k¨onnen



1 Queue. Es gibt zwei Komponenten: Das die Events produzierende System, und das System, dass die
Mitteilungen an User raussendet. Die beiden Systeme k¨onnen ¨uber eine Queue miteinander verbunden
werden, damit beide unabh¨angig voneinander arbeiten k¨onnen. Damit k¨onnen z.B. viele unterschied-
liche Event-Produzenten mit nur wenigen Nachichtensendern verbunden werden. Außerdem kann ein
System weiterarbeiten, wenn das andere kurz offline ist o. ¨A. Ein Pub/Sub System ist hier nicht not-
wendig, wenn ein Event nur an einen User gesendet wird. Wenn ein Event potentiell an mehrere User
gesendet wird, k¨onnte ein Pub/Sub System in Frage kommen.

 
 2 Synchrone Kommunikation, z.B. per RPC. Es ist wichtig, dass beide Banken immer einig sind, wel-
che Konten aktuell welchen Konstostand haben. Deshalb m¨ussen beide Banken direkt miteinander
kommunizieren. Auch asynchron ist m¨oglich, sofern die Nachrichtenzustellung zuverl¨assig erfolgt und
beide Banken nicht zu jedem Zeitpunkt den gleichen Informationsstand besitzen m¨ussen.


3 Pub/Sub. Damit kann jeder Client anmelden, welche Aktienkurse interessant sind.


4 Kombination von synchroner und asynchroner Kommunikation. Wenn beide User online sind k¨onnen
Nachrichten synchron ausgetauscht werden. Wenn ein User offline ist, k¨onnen die Nachrichten in einer
Queue zwischengespeichert werden, bis der User wieder online ist.


