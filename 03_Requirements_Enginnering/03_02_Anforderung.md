

# 1 Textuelle Anforderungsspezifikation

1. Spezifikation in natürlicher Sprache
	1. Ausführliche textuelle Beschreibung der Anforderungen an ein System bzw. einer Funktionalität
	2. Problem: Häufig entsteht viel Interpretationsspielraum, da natürliche Sprache nicht eindeutig ist
2. Strukturierte Spezifikation
	1. Übersichtlichere Erfassung von Anforderungen im Vergleich zu natürlicher Sprache
	2. Tabellarische Erfassung von Anforderungen mit einheitlichen Eckdaten wie z.B.
	3. ![[03_Requirements_Enginnering/image/Pasted image 20250104163253.png]]
3. Mathematische Spezifikation
	1. Nutzen eines mathematischen Formalismus zur Beschreibung der Anforderung
	2. Formalismen: Formale Logiken, Automaten, (Object) Z, OCL, Prozesskalküle, …


## 1.1 Strukturierte Spezifikation


Informelle, tabellarische Darstellung nach einheitlichem Schema
➢ Übersichtlicher als reiner Text
➢ unterstützt Konsistenz und Vollständigkeit

Typische Felder:
• Name, Beschreibung (verbal)
• Inputs, Outputs: Datenaustausch mit der Komponente
• Pre: Nötige Vorbedingungen für die Ausführung
• Post: Nachbedingung – Zustand nach Ausführung; Zielbeschreibung
• Aktion: Ausgeführte Aktionen, auch Details/Zwischenschritte mgl.



## 1.2 User Stories

Agile Methoden gehen von häufigen Anforderungsänderungen aus
➢Detaillierte/vollständige Dokumentation vorab nicht möglich (oder Zeitverschwendung)
➢Anforderungen werden inkrementell entsprechend dem Entwicklungsprozess ermittelt

Passendes Format: User Stories
• Kurze Beschreibung einer einzelnen Anforderung
• 1-2 Sätze (natürlichsprachlich)
• Aus Sicht von Benutzer:innen (Rolle) geschrieben
• Konzentriert sich auf das Ergebnis (“was brauchen Benutzer:innen”, statt “was sollte das System (wie) liefern”)

Je nach Vereinbarung gibt es ein einheitliches Format, z.B.:
- As a (role) I want (something) so that (benefit). [Quelle:2]

Beispielsweise:
• User Stories werden auf Karten notiert (bzw. möglichst einfaches Tool)
• Auf der Rückseite werden Kriterien für die Validierung notiert
• Weitere Infos: Priorität (kann, sollte, muss), Aufwandsabschätzung



# 2 Grafische Anforderungsspezifikation

## 2.1 Verhaltensmodellierung mit UML

Verhaltensmodellierung betrifft dynamische Aspekte des Systems

Verhalten ist beobachtbar als Veränderungen…
… der Eigenschaften der beteiligten Elemente (Zustandsänderungen)
… der Struktur des Gesamtsystems

Grundformen der Verhaltensbeschreibung zur Unterstützung verschiedener Sichten auf das Verhalten
• Anwendungsfälle (Use Cases)
• Zustandsautomaten
• Aktivitäten
• Interaktionen

## 2.2 Use cases

Spezifikation eines fachlichen Ziels von Akteur:in (Anwendungsfall)
• Akteur:in (Rolle) wird identifiziert
• Use Case wird benannt und ggfs. genauer spezifiziert
• Wesentliche Spezial- und Fehlerfälle werden mit aufgeführt

Darstellung:
• UML Use-Case-Diagramm (enthält mehrere Use-Cases, Details später)
• Detaillierte Dokumentation von Use-Cases z.B. durch strukturierte Spezifikation
• Abläufe durch weitere Diagramme dokumentiert (z.B. Sequenzdiagramm)

Nach dem Requirements Engineering:
➢ alle möglichen Interaktionen mit dem System als Use-Cases dokumentiert

---

Use-Case (Anwendungsfall-) Diagramm
Grafische Erfassung von Akteuren/Akteurinnen und Anwendungsfällen

Modellierungselemente
• Akteur:in
• Anwendungsfälle
	• Akteur:in versucht mit dem System ein fachliches Ziel zu erreichen
	• Kann mehrere Ablauf-Szenarien zusammenfassen (Bsp. Erfolg/Misserfolg)
• Beziehungen

Use-Case Diagramm möglichst einfach halten
• Konzentration auf sichtbares Verhalten
• Von Akteur:in angestoßen
• Darstellung von Details/Abläufen nicht im Use-Case-Diagramm!
➢ Grundlage für detailliertere Verhaltensdiagramme

### 2.2.1 Use-Case Modellierung 

Sie werden gebeten für einen kleines Unternehmen, das Schuhe und Kleidung verkauft, die Verwaltungssoftware eines Online-Shops zu entwickeln. Der Onlineshop soll es den Kunden und Kundinnen ermöglichen, Produkte in einen Warenkorb zu legen und diesen zu bezahlen. Als Bezahlmethoden sind zunächst Bankeinzug und Kreditkartenzahlung vorgesehen. Bevor die Bestellung aufgegeben wird, muss sichergestellt werden, dass die Bezahlung tatsächlich erfolgen kann. 

Weiterhin soll das System gleichzeitig auch die Nutzer:innen verwalten. Sowohl Kunden und Kundinnen als auch Mitarbeitende sollen registriert werden können. Auf Sicherheit soll entsprechend geachtet werden.

Produkte sollen über das Webinterface auch gesucht werden können. Dabei sollen Rechtschreibfehler toleriert werden und innerhalb von einer akzeptablen Zeit sollen passende Produkte angezeigt werden. 

Alle Funktionen werden von Nicht-Entwickler:innen ausgiebig getestet.

![[03_Requirements_Enginnering/image/Pasted image 20250104170632.png]]


Extension 的作用 
Use Case mit `<<extend>>` kann, aber muss nicht direkt mit ausgeführt werden (Zur Modellierung von Spezial-/Fehlerfällen)

![[03_Requirements_Enginnering/image/Pasted image 20250104170644.png]]

---


![[03_Requirements_Enginnering/image/Pasted image 20250104171142.png]]

include 
Use Case mit `<<include>>` muss direkt mit ausgeführt werden

![[03_Requirements_Enginnering/image/Pasted image 20250104171316.png]]

---

![[03_Requirements_Enginnering/image/Pasted image 20250104171601.png]]

![[03_Requirements_Enginnering/image/Pasted image 20250104171847.png]]

# 3 Nicht-funktionale Anforderungen


Funktionale vs. Nicht-Funktionale Anforderungen

Funktional
- Beschreibt was das System tun bzw. was es nicht tun soll
- Betrifft meist einzelne Aufgaben des Systems
- Kann meistens unmittelbar geprüft werden
	- Funktionalität wie vorhanden ja/nein?

Nicht-Funktional
• Beschreibt auf welche Weise das System bestimmte Dinge tun soll, bzw. wie das System sein soll
• Betrifft oft das gesamte System
• Kann nicht immer direkt überprüft werden
• Wieviel Speicher wird maximal gebraucht? Wie lange braucht die Berechnung schlimmstenfalls? …

![[03_Requirements_Enginnering/image/Pasted image 20250104172331.png]]








