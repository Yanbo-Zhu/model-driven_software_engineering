
REST steht für Representational State Transfer
- REST ist ein Software-Architekturstil zur Erstellung skalierbarer Webservices
- RESTful Webservices verwenden standardisierte HTTP-Methoden wie GET, POST, PUT, DELETE und PATCH, um mit Ressourcen zu interagieren, die als URIs repräsentiert warden
- REST-Schnittstellen sind “stateless”, d.h. der Server speichert keine Informationen über den Client zwischen Anfragen.

# 1 Query Parameters

![](image/Pasted%20image%2020250104124016.png)


# 2 Beispiel REST-Schnittstelle

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