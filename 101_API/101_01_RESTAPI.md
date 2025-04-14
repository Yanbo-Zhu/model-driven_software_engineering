Rest 不是一种标准, 而是一个设计风格 

REST steht für Representational State Transfer
- REST ist ein Software-Architekturstil zur Erstellung skalierbarer Webservices
- RESTful Webservices verwenden standardisierte HTTP-Methoden wie GET, POST, PUT, DELETE und PATCH, um mit Ressourcen zu interagieren, die als URIs repräsentiert warden
- REST-Schnittstellen sind “stateless”, d.h. der Server speichert keine Informationen über den Client zwischen Anfragen.

- RESTful是目前流行的互联网软件服务架构设计风格。
- REST(Representational State Transfer,表述性状态转移)一词是由Roy Thomas Fielding在2000年的博士论文中提出的，它定义了互联网软件服务的 架构原则，如果一个架构符合REST原则，则称之为RESTfu架构。
- ==REST并不是一个标准，它更像一组客户端和服务端交互时的架构理念和设计原则，基于这种架构理念和设计原则的Web API更加简洁，更有层次==。

- 每一个URI代表一种资源
- 客户端使用GET、POST、PUT、DELETE四种表示操作方式的动词对服务端资源进行操作：GET用于获取资源，POST用于新建资源（也可以用于更新资源）PUT用于更新资源，DELETE用于删除资源。
- 通过操作资源的表现形式来实现服务端请求操作。
- 资源的表现形式是JSON或者HTML。
- 客户端与服务端之间的交互在请求之间是无状态的，从客户端到服务端的每个请求都包含必需的信息。

- 符合RESTfu规范的Web API需要具备如下两个关键特性：
- 安全性：安全的方法被期望不会产生任何副作用，当我们使用GET操作获取资源时，不会引起资源本身的改变，也不会引起服务器状态的改变。
- 幂等性：幂等的方法保证了重复进行一个请求和一次请求的效果相同（并不是指响应总是相同的，而是指服务器上资源的状态从第一次请求后就不再改变了)，在数学上幂等性是指N次变换和一次变换相同。


# 1 REST 代编什么 

- **RE**presentational: Die Daten können in verschiedenen Repräsentationen abgerufen werden, beispielsweise in XML, JSON, HTML, JPEG und SVG.

- **S**tate **T**ransfer: Die Kommunikation ist "stateless" (zustandslos) und somit werden keine vorhergegangenen Kommunikationsprozesse gespeichert. Die Übermittlung in einem zustandslosen Protokoll wurde im Kapitel [HTTP 1.1 zustandslos und persistent](https://isp.eduloop.de/loop/HTTP_1.1_zustandslos_und_persistent "HTTP 1.1 zustandslos und persistent") vertieft.
    - Jede Anfrage enthält alle Informationen.
    - Jede Anfrage ist in sich geschlossen.

REST selbst ist ein Architekturstil für die Datenübertragung in der Maschine-zu-Maschine-Kommunikation und unabhängig von HTTP und Web. In der Praxis jedoch nutzt man REST für eine HTTP-basierte Kommunikation über Web-Schnittstellen, also **RESTfull Services via HTTP**, die wir im Folgenden beschreiben.

# 2 常用的相关的 HTTP Method 

![[101_API/image/Pasted image 20250221113206.png]]

![[101_API/image/Pasted image 20250221113340.png]]

![[101_API/image/Pasted image 20250221113355.png]]

# 3 Query Parameters

![](image/Pasted%20image%2020250104124016.png)


# 4 Beispiel REST-Schnittstelle

1 Beschreibung: Alle TODOs auflisten die noch nicht abgeschlossen sind
Resource: /todos
HTTP Method: GET

Request:
`curl https://example.com/todos`

Response:
`[{ ”id”: 0, ”title": "My First TODO", "date": "2023-04-25", "content": "Lorem ipsum dolor sit amet, consectetur adipiscing elit..." }, {”id”: 1, "title": "My Second TODO", "date": "2023-04-26", "content": "Ut enim ad minim veniam, quis nostrud exercitation ullamco..." }]`


---
2  Beschreibung: TODO mit der ID 1 anfragen
Resource: /todos/1
HTTP Method: GET

Request:
`curl https://example.com/todos/1`

Response:
`[{”id”: 1, "title": "My Second TODO", "date": "2023-04-26", "content": "Ut enim ad minim veniam, quis nostrud exercitation ullamco..." }]


---

3 Beschreibung: Alle offenen TODOs auflisten
Resource: /todos?completed=False
HTTP Method: GET

Request:
curl https://example.com/todos?completed=False

Response:
`[{”id”: 0, "title": "My First TODO", "date": "2023-04-25", "content": "Lorem ipsum dolor sit amet...”, ”completed”: False}]`


---

4 Beschreibung: Neues TODO anlegen
Resource: /todos
HTTP Method: POST

Request:
`curl --request POST https://example.com/todos --header 'Content-Type: application/json --data-raw ‘[{"title": "My Third TODO", "content": "Lorem ipsum dolor sit amet...”}‘`

Response:
` {”id”: 2, "title": "My Third TODO", "date": "2023-04-26", "content": "Lorem ipsum dolor sit amet...”, ”completed”: False}`