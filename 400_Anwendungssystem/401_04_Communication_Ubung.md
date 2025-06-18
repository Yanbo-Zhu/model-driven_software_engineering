

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



# 4 gRPC

![[400_Anwendungssystem/image/9a6eaa31b5e8a6dfc7848a33fb889d8.jpg]]


## 4.1 Beispielcode 

echo.proto 


写完上面

后通过 gradle 产生两个class , 如下 




---

EchoServiceImpl.java





记得去 start Server 通过 run this classs file 

---

EchoServiceClient.java 





# 5 Message Queues 


![[400_Anwendungssystem/image/d72d6ef473f9ec94d2fa4c5236dc903.jpg]]



# 6 Middleware - paradigmen 


Welcbe Art der kommunikation wurdet Ihr wahlen wenn euer System, highly availbale sein soll? 

Ayschronoe Komunikation,  
Es should never akzpt, server kapuut 



Wie wirkt sich die Wahl von RPC oder MQ auf die sklierbarkeit des Systems aus 


# 7 Queues

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






# 8 Losse Coupling

Coupling =  Abhangigkeit zwischen 2 komponenten in einem system 

Wie schaffst , so wenig Coupling zu machen : Druch Kafka oder another Mittelware , durch, die alle Traffic verwalten kann 


Auf welche Ebene kann es Abhangigkeiten zwischen Kompinenten geben 


![[400_Anwendungssystem/image/cb50aec2bc716ae4666603314398599.jpg]]


# 9 Pub/Sub




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




Welche Anwendungsfalle sind am besten fur Pub/sub/ 



















