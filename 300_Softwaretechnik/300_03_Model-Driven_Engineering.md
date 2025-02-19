
Chapter 09 10 中的

# 1 Model-Driven Development (MDD)

Bei der modellgetriebenen Softwareentwicklung steht die Erstellung eines abstrakten Modells im Vordergrund
- Erlaubt Entwicklung auf hohem Abstraktionsniveau
- Reduziert die Wahrscheinlichkeit von technischen Fehlern
- Erhöht die Wiederverwendbarkeit und verringert Redundanz
- Entwurfs- und Implementierungsprozess wird beschleunigt
- Die Modelle sind plattformunabhängig (frei von Ausführungsdetails)
- Domänenspezifische Modelle können direkt von Experten und Expertinnen erstellt werden

Das endgültige Produkt kann aus dem Modell abgeleitet werden
- Manuell: Durch klassische Implementierung
- Automatisch: Durch Code-Generatoren


![](image/Pasted%20image%2020250115230025.png)


# 2 Von UML zu Code

UML bietet viele Abstraktionsmöglichkeiten und Gestaltungsmittel ohne eindeutige Semantik
- Keine direkt Erstellung von ausführbaren Systemen möglich
- Aber: Tools bieten die Möglichkeit Skelette zu erzeugen

xUML (Executable UML)
- Klassendiagramme beschreiben die domänenspezifische Struktur
- Zustandsmodelle beschreiben den Lebenszyklus für jede Klasse
- OCL oder eine UML Action Language beschreiben das Verhalten
- Zusätzliche Ausführungssemantik und Zeitverhalten ermöglichen die automatische Generierung von plattformspezifischen Modellen


# 3 Generative Programmierung

In der generativen Programmierung wird Programmcode automatisch durch einen Generator erzeugt
• Eine parametrisiert Eingabespezifikation (Modell) wird konfiguriert und in eine konkretisierte Ausgabespezifikation (Code) übersetzt
• Das geschieht entweder statisch (Entwickler:in) oder dynamisch (Kunde/Kundin)
• Ermöglicht die gleichzeitige Entwicklung mehrerer Programmvarianten


![](image/Pasted%20image%2020250115230224.png)

# 4 Modellbasiertes Testen 

Auch Testfälle können direkt aus Modellen abgeleitet werden
• Die Modelle dienen dann als Spezifikation für das System Erstellung und Anwendung von Testfällen kann automatisiert werden
• Personenunabhängige Qualität der Testfälle
• Höhere Transparenz und Wiederverwendbarkeit

Achtung: Modell und Testfallgenerator müssen validiert/verifiziert werden!

![](image/Pasted%20image%2020250115230507.png)


# 5 Domänenspezifische Sprachen

General-Purpose Languages, GPLs
C, C++, C#, Python, Java, Haskell…
•Meist Turing-Mächtig
•Domänenspezifische Probleme müssen erst in generelle Form überführt werden
•Resultierende Programme enthalten meist wenig direkt zugängliche Information über die Domäne

Domain-Specific Languages, DSLs
SQL, VHDL, MATLAB,EN 61131-3…
•Auf Domäne zugeschnitten
•Direkte Beschreibung domänenspezifischer Probleme
•Resultierende Programme sind oft frei von „boilerplate“-Code
•Für Endnutzer:in leichter zu erlernen




