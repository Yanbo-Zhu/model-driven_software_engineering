
aus Softwaretechnik chapter 11 Implementierung 

Einige prinzipielle Systemstrukturen haben sich als häufig angewendetes Muster (Architekturstil) etabliert

In dieserVL:
• Model-View-Controller
• Layer-based
• Repository-based
• Pipes-and-Filter
• Event-based
• Interrupt-based
• Client-Server
• Peer-to-Peer
• Service-oriented


Architektur spielt für die Implementierung eine wesentliche Rolle
• Eine klare Struktur sollte frühzeitig gewählt werden
• Verwendung bewährter Architekturstile empfohlen
• Funktionales Verhalten möglicherweise mit mehr Architekturstilen abbildbar als nicht-funktionale Anforderungen

Innerhalb von Systemen können Architekturstile gemischt auftreten

Einzelne Komponenten verteilter Systeme können wiederum jeweils eine eigene Architektur haben


# 1 Pipes-and-Filter Architecture

Arbeitsschritte nur durch Daten verknüpft
• fließen wie durch ein Rohr von einer Komponente zur nächsten
• typisch für Systeme, die Daten schrittweise weiterverarbeiten
• Bearbeitung sequenziell und parallel möglich

![[301_architecture/image/Pasted image 20250219155901.png]]


Vorteile
• System bildet Geschäftsprozesse direkt ab
• übersichtlich
• modular, leicht erweiterbar
• Systemzustand in den Daten gekapselt

Nachteile
• Daten müssen von jeder Komponente erneut aufbereitet werden
• Vorgegangene Schritte müssen immer vollständig abgeschlossen werden


# 2 Layer-based Architecture

Aufteilung in mehrere Abstraktionsschichten
• Trennung z.B. von technischen Details und Inhalten
• Schichten bieten darüber liegenden Schichten Dienste an
• Elementare Dienste in unterster Schicht



![[301_architecture/image/Pasted image 20250219160002.png]]


TCP/IP-Referenzmodell (Wikipedia)
verschiedene Schichten und Protokolle der Internet-Kommunikation
Klar festgeschriebene Schnittstellen!

![[301_architecture/image/Pasted image 20250219160016.png]]

Vorteile
- Abstraktion von Details der einzelnen Schichten. 
	- Separation of Concerns, Information Hiding
- Flexibler Austausch von Schichten möglich
	- Wartbarkeit, Erweiterbarkeit

Nachteile
- Trennung kann Performance-Nachteile bringen
	- Spezialfälle mit effizienteren Lösungen müssen generalisiert werden
- Anfragen/Antworten müssen über mehrere Schichten weitergeleitet werden


# 3 Model View Controller

Model
• enthält die persistenten Daten
• Geschäftslogik innerhalb dieser Daten
• unabhängig von anderen Einheiten

View (Präsentation)
• Darstellung der Daten
• Entgegennahme von Benutzerinteraktion
• kennt Modell und Control
• verschiedene Präsentationen (Views) möglich

Controller (Steuerung)
• verwaltet die Präsentation
• führt Benutzeranfragen aus und gibt sie ggf. an das Modell weiter


![[301_architecture/image/Pasted image 20250219160344.png]]

Ermöglicht verschiedene Interaktionen mit dem System
• Typisch in Systemen mit Fokus auf Benutzerschnittstellen
• Erleichtert spätere Erweiterungen, z.B. neue Präsentationsarten (View)
• Ermöglicht Austausch getrennter Komponenten, z.B. der Datenbank

MVC ähnelt dem ECB-Pattern: Aufteilung in boundary, entity & control

![[301_architecture/image/Pasted image 20250219160407.png]]


# 4 Event-based Architecture

Komponenten sind unabhängig von einander
• warten auf Ereignisse (Events) zum Start von Aktivitäten (consumer)
• oder lösen Ereignisse aus (producer)
• zentrale Komponente (event channel) verteilt die Ereignisse

![[301_architecture/image/Pasted image 20250219160503.png]]

Vorteile
• Reaktion auf Ereignisse kann unmittelbar erfolgen
• geeignet für asynchrone/chaotische Umgebungen (z.B. Benutzer-Interaktion)

Nachteile
• Nicht behandelte Events müssen u.U. zwischengespeichert werden
• erhöhte Anforderungen an Synchronisation
• Verhalten schwerer vorhersagbar


# 5 Interrupt-based Architecture

Spezialfall von event-based architectures
• Verwendet Interrupts als Hardware(„low-level“)-Events
• Interrupts können maskiert (ignoriert) werden

![[301_architecture/image/Pasted image 20250219160600.png]]


# 6 Repository-based Architecture

Organisation des Systems um einen zentralen Datenspeicher
• Komponenten sind über gemeinsame Daten (z.B. Datenbank) verbunden
• Koordination von Komponenten im Repository (z.B.: Trigger, Locks)

![[301_architecture/image/Pasted image 20250219160802.png]]

Vorteile
• wenig Schnittstellen
• konsistente, zentrale Datenhaltung
• Einfache Erweiterbarkeit durch Hinzufügen neuer Komponenten

Nachteile
• Kommunikation zwischen Komponenten über die Daten teilweise ineffizient
- Die Anbindung ans Repository wird zum Bottleneck
• Repository ist „single point of failure“
- Ausfall der zentralen Komponente bedeutet Totalausfall

# 7 Client-Server Architecture

Architekturstil für verteilte Systeme
• jede Systemfunktion wird als Dienst auf einem zentralen Server angeboten
• Clients können diese Funktion in Anspruch nehmen

![[301_architecture/image/Pasted image 20250219160844.png]]

Keine direkte Kommunikation zwischen Clients
• Interaktion zwischen Clients nur über Server möglich

![[301_architecture/image/Pasted image 20250219160942.png]]


Vorteile
• Funktionen stehen zentral zur Verfügung, müssen nicht mehrfach implementiert werden
• einfache Verwaltung gemeinsam genutzter Ressourcen
• Leistungsschwache Clients können Arbeitslast auf den Sever auslagern

Nachteile
• Single point of failure
• zusätzliche Kommunikation
• ungleiche Lastverteilung / schlechte Ressourcennutzung
• Zentrale Daten (Privacy)




# 8 Peer-to-Peer Architektur

Komponenten in verteilten Systemen sind gleichberechtigt
• jede kann Funktionen bereitstellen und nutzen
• meist macht ein zentraler Server die Teilnehmer bekannt

Skype (in den Anfängen)
• Jeder Client meldet sich beim Server nur zur Authentifizierung und zum
Abrufen des Status der Kontakte


![[301_architecture/image/Pasted image 20250219161019.png]]

Vorteile
• effiziente Kommunikation (keine Umwege)
• gleichmäßige Last/Ressourcenverteilung
• Ausfallsicherheit

Nachteile
• Gemeinsam genutzte Ressourcen schwierig zu synchronisieren
• Funktionen mehrfach implementiert (Komponenten komplexer)
• Datenverkehr nur zwischen einzelnen Teilnehmern

Verteilung kann auch dezentral geschehen (cf. Distributed Hash Tables)


# 9 Service Oriented Architecture

System besteht aus verteilten, unabhängigen Komponenten
• Komponenten bieten Funktionalitäten als Services an
• Globale Registry für Service-Anbieter und Services
• Komplexe Komponenten können wieder andere Services verwenden
• Standard-Protokoll (z.B. SOAP, REST) für alle Services


Vorteile
• Services/Funktionalitäten austauschbar
• Erweiterung durch simple Registrierung neuer Services
• erlaubt loose coupling (dynamische Verbindung im Betrieb)
• einheitliche/standardisierte Protokolle für Schnittstellen

Nachteile
• komplexe technische Infrastruktur
• Registry ist „single point of failure“


![[301_architecture/image/Pasted image 20250219161113.png]]


![[301_architecture/image/Pasted image 20250219161123.png]]


# 10 方式

## 10.1 Softwarearchitektur in der plangesteuerten Softwareentwicklung

Vorgehensweise
• Anforderungen ( 橙色圆圈 ) stehen zu Beginn der Implementierungsphase fest
• Grundlegende Architektur ( 蓝色方框 ,  蓝色圆圈 ) kann nach Anforderungsspezifikationentworfen werden
• Top-Down-Implementierung möglich: abstrakte Implementierung der Architektur (Grundgerüst), gefolgt von Details und Features

![[301_architecture/image/Pasted image 20250219161430.png]]

Vorteile
• Berücksichtigung späterer Anforderungen, wodurch eine strukturierte Entwicklung der Architektur möglich wird
• Parallele Implementierung von Anforderungen innerhalb der Grundstruktur
• Identifikation von Fehlern in den Anforderungen

Nachteile
• Fehler in der Architektur aufgrund hoher Komplexität
• Änderungen der Architektur aufgrund von neuen Anforderungen ist teuer
• Geringe Akzeptanz der Architektur durch Entwickler
• Technische Probleme werden erst nach dem Architekturentwurf ersichtlich

## 10.2 Softwarearchitektur in der agilen Softwareentwicklung 


Vorgehensweise
• Stetige Änderungen der Anforderungen ( 橙色圆圈 )
• Grundlegende Architektur ( 蓝色方框 ,  蓝色圆圈 ) nicht planbar, sondern Ergebnis permanenter Veränderung
• Iterative Weiterentwicklung der Architektur anhand der nächsten Anforderungen
• Prinzip Einfachheit: Aktuelle Implementierung sollte simple sein und somit spätere Änderungen erlauben

![[301_architecture/image/Pasted image 20250219161912.png]]

Vorteile
• Demokratischer Prozess: Architektur orientiert an technischer Umsetzung
• Schnelle, lauffähige Iterationen mit integrierter Implementierung
• Aktualität und zeitnahes Feedback zur Architektur

Probleme
• Mehraufwand durch häufiges Umbauen der Architektur bzw.vollständige Neuimplementierung notwendig
• Ständige Anpassungen bereits fertiger Implementierung(kann auch positiv sein – Refactoring)

Achtung: Die vorgestellten Architekturstile sind in der plangesteuerten und agilen Softwareentwicklung identisch!


