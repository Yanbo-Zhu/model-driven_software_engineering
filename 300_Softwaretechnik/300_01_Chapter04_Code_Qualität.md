


# 1 Software-Metriken 

Objektivität kein Einfluss durch den Messenden
Zuverlässigkeit Ergebnis für dieselbe Messung immer gleich
Normierung Skala existiert, die Messergebnisse einordnet
Vergleichbarkeit mit anderen Maßen in Relation
Ökonomie Messung nicht zu teuer/durchführbar
Nützlichkeit hilft in der Praxis
Validität sinnvoll, misst das Richtige (Messergebnisse erlauben die gewünschten Rückschlüsse)


## 1.1 Statische Produktmetriken

- Traditionellen Metriken
    - Metriken zur Messung der Programmgröße und dessen Komplexität
        - Zeilenmetriken (LOC) und Halstead-Metriken
    - Metriken zur Messung der Programmstruktur
        - Zyklomatische Komplexität nach McCabe
- Objektorientierte Metriken
    - Verhältnisse der einzelnen Elemente (Klassen, Methoden) untereinander

### 1.1.1 Zeilenmetriken

Lines Of Code (LOC)
• Anzahl der Zeilen im Programm

Non-Commenting Lines Of Code (NCLOC)
• ignoriere Leerzeilen und reine Kommentarzeilen
• Anteil an Kommentarzeilen:
    • sollte zwischen 30% und 75% liegen 

Typische Vorgabewerte für LOC
• Länge einer Funktion zwischen 4 und 40 Zeilen
• Länge einer Datei zwischen 40 und 400 Zeilen (10-100 Funktionen)


Vorteil
• leicht zu berechnen
• leicht nachzuvollziehen / intuitiv

Nachteil
• wenig aussagekräftig
• abhängig vom Programmierstil
• bessere Programmstruktur kann auch weniger Zeilen bedeuten
• weniger Zeilen kann auch komplexeren Code bedeuten


### 1.1.2 Halstead Metriken

Programm aufgefasst als Sequenz von Operatoren und Operanden

![](image/Pasted%20image%2020250218160932.png)


Vorteile
• leicht automatisch zu berechnen
• in Studien nachgewiesen: korrespondiert mit echter Komplexität

Nachteile
• Konzepte moderner Programmiersprachen unberücksichtigt
(Namensräume, Sichtbarkeiten, Vererbung, …)
• Aufteilung in Operatoren vs. Operanden nicht immer möglich
• Struktur und Ausführung nicht berücksichtigt:  Wie beschreibt man eigentlich die Struktur eines Programmes?


### 1.1.3 Strukturmetriken

Zyklomatische Komplexität v(G)
• eingeführt 1976 von Thomas McCabe

Definiert als
• Anzahl konditioneller Zweige im Kontrollflussgraphen des Programms
• Entspricht Anzahl binärer Verzweigungen + 1

Zyklomatische Zahl
• v(G) ≤ 10: einfache Programme
• v(G) > 10: Fehler nehmen stark zu
• v(G) ≥ 50: sehr bzw. zu komplexe Programme, kaum noch zu testen
➢ je höher, desto komplexer das Programm, desto mehr Testfälle nötig

![](image/Pasted%20image%2020250218161112.png)


![](image/Pasted%20image%2020250218161120.png)

Vorteile
• leicht zu berechnen
• in Fallstudien: gute Korrelation zwischen zyklomatischer Komplexität und Programmverständlichkeit
• Eignet sich zur Testplanung

Nachteile
• berücksichtigt Kontroll-, aber nicht Datenfluss
• Wenig aussagekräftig in objektorientierter Software mit vielen einfachen Zugriffsmethoden (z.B. Attribute)



### 1.1.4 Weitere Metriken


D: Verschachtelungstiefe (nested block depth)
➢ sollte <5 sein

NST: Number of Statements (etwa Anzahl Semikolons in JAVA)
➢ sollte <50 sein

NFC: Number of Function Calls
➢ sollte <5 sein

NOM: Number of Methods

Objektorientierte Metriken für besondere Aspekte
• klassische Metriken können aber auf Methoden angewendet werden



## 1.2 Objektorientierte Metriken

Depth of Inheritance Tree (DIT)
• maximaler Abstand von der Wurzel der Klassenhierarchie zur Klasse
• Wahrscheinlichkeit für Fehler größer, wenn DIT größer, weil
    • Komplexität größer, Code schwerer verständlich
    • Testaufwand größer
    • aktuelle Klasse selbst schwerer wiederverwendbar


Number of Children (NOC)
• Anzahl direkter Subklassen
• nicht immer eindeutig interpretierbar
    • interpretierbar als Fortpflanzungswahrscheinlichkeit für Fehler
    • Fehlerwahrscheinlichkeit geringer, wenn NOC größer (inverses Maß)


Response for a Class (RFC)
• Anzahl der Methoden, die evtl. direkt aufgerufen werden, wenn ein Objekt der Klasse eine eingehende Methode ausführt
• Fehlerwahrscheinlichkeit steigt mit RFC-Wert


Weighted Methods per Class (WMC)
• Anzahl der Methoden einer Klasse, kann gewichtet werden nach Größe oder Komplexität
• je größer WMC, umso größer die Fehlerwahrscheinlichkeit


Coupling Between Objects (CBO)
• Anzahl Klassen, mit denen eine Klasse gekoppelt ist
• hoher Kopplungsgrad erhöht Fehlerwahrscheinlichkeit
• niedriger Kopplungsgrad zeigt bessere Wiederverwendbarkeit an



Lack of Cohesion in Methods (LCOM)
• Anzahl Methodenpaare in einer Klasse ohne gemeinsame Instanzvariablen
• hohe Kohäsion zeigt gute Kapselung innerhalb einer Klasse an, reduziert Programmkomplexität
• niedrige Kohäsion: Programmstruktur kann verbessert werden, z.B. durch Aufteilung in mehrere Klassen


# 2 Code-Smells

Schlecht strukturierter Code („Code-Smells“)ist oft offensichtlich und lässt sich kategorisieren.



Smell - Datenklumpen
Gleiche Variablen treten an vielen Stellen des Programms gemeinsam auf:
```
publicclassAdressbook{
    // ...
    voidcallContact(String name, String countryCode, String phonenumber) {
    // ...
    }
    
    voidshowContact(String name, String countryCode, String phonenumber) {
    // ...
    }
    
    voidsendMessage(String name, String countryCode, String phonenumber, String msg) {
    // ...
    }
    // ...
}
```


Smell - viele Bedingungen (Switch Anweisung )
Viele Bedingungen im Kontrollfluss, die durch eine Switch-Anweisung entstehen, können schwer verständlich sein.
```
voiddoesNotLookTooBad(inti, booleany) {
    switch(i) {
    case0:
        // do something
    case1:
        // do something else
    case2:
        // do another thing
    case3:
        if(y) {
            break;
        }
    // More possible cases here
    default:
        thrownewIllegalArgumentException("Unexpected value: "+ i);
    }
}

```


Smell – Neid (falsche Zuständigkeit)
Eine Methode in einer Klasse verwendet viele Attribute einer anderen Klasse.

![](image/Pasted%20image%2020250218162435.png)


• Lange Methoden
• Lange Parameterliste
• Große Klasse
• Duplizierter Code
• Neigung zu elementaren DatentypenViele elementare Datentypen weisen darauf hin, dass diese eventuell in einer Datenstruktur gekapselt werden können.
• Unangebrachte IntimitätEine Klasse verwendet viele Teile einer weiteren Klasse, bei denen Details der Implementierung (im Gegensatz zur Schnittstelle) eine Rolle spielen.
• Viele KommentareKommentare sind dort notwendig, wo der Code schwer verständlich ist.


# 3 Code Refactoring



不同的 Code_Refactoring 的方法

Dabei gilt für jede Regel
• Sie dient einem primären, ausgewiesenen Zweck
• Sie ist möglichst einfach, um neue Fehler zu vermeiden
• Sie ist konstruktiv und besteht wiederum aus mehreren Schritten, die eine Ausgangssituation in einen Zielzustand überführen
• Beobachtbares Verhalten wird beibehalten


Refactoring kann zu verschiedenen Zeiten im Entwicklungsprozess notwendig werden
• Falls das Hinzufügen von neuen Funktionen von früheren Designentscheidungen erschwert wird
• Falls Fehler behoben werden, deren Identifikation durch unverständlichen Code erschwert wurde
• Nach Code-Reviews


Jeder Code-Smell weist auf eine Reihe von Refactoring-Schritten hin, die helfen, das Problem zu beheben
• Die einzelnen Regeln wurden in einem Katalog zusammengetragen
• Sie werden unter anderem durch ihren Namen, der Motivation, einer ausführlichen Beschreibung der Vorgehensweise, sowie einer kurzen Zusammenfassung charakterisiert
• Die ausführliche Beschreibung der Vorgehensweise weist auf Fehlerpotenziale und andere Regeln hin
Der Katalog wurde ursprünglich von Martin Fowler zusammengetragen und umfasst über 70 Refactorings

![](image/Pasted%20image%2020250218162051.png)



## 3.1 Klasse extrahieren


Eine Klasse macht die Arbeit, die von zwei Klassen zu erledigen wäre.
Smells: Große Klasse, Datenklumpen, duplizierter Code
Zusammenfassung: Erstellen Sie eine neue Klasse, und verschieben Sie die relevanten Felder und Methoden von der alten Klasse in die neue

![](image/Pasted%20image%2020241129235512.png)



## 3.2 Methode extrahieren


Ein Fragment im Code kann zusammengefasst werden.

Smells: Lange Methode, duplizierter Code, Kommentare

Zusammenfassung: Machen Sie aus dem Fragment eine Methode, deren Name die Aufgabe der Methode erklärt.


![](image/Pasted%20image%2020241129235619.png)


## 3.3 Geschachtelte Bedingungen durch Wächterbedingungen ersetzen

Wächter: 守卫者 看守人

Eine Methode weist ein bedingtes Verhalten auf, das den normalen Ablauf nicht leicht erkennen lässt.

Smells: Lange Methode

Zusammenfassung: Verwenden Sie Wächterbedingungen für die Spezialfälle

![](image/Pasted%20image%2020241129235707.png)


## 3.4 Parameter durch explizite Methode ersetzen

Eine Methode führt abhängig von einem ihrer Parameter unterschiedlichen Code aus.

Smells: Lange Parameterliste, Switch-Befehl

Zusammenfassung: Erstellen Sie eine separate Methode für jeden Wert des Parameters



## 3.5 Parameterobjekt einführen

Eine Gruppe von Parametern gehört auf natürliche Weise zusammen.
Smells: Lange Parameterliste, Neigung zu elementaren Typen, Datenklumpen
Zusammenfassung: Ersetzen Sie sie durch ein Objekt

![](image/Pasted%20image%2020241130000058.png)



## 3.6 Methode verschieben

Eine Methode nutzt mehr Elemente einer anderen Klasse oder wird von mehr Elementen einer Klasse benutzt als von denen, in der sie definiert ist.

Smells: Datenklassen, unangebrachte Intimität  不恰当的亲密感 , Neid 嫉妒

Zusammenfassung: Ersetzen Sie sie durch eine neue Methode in der Klasse, die sie am meisten verwendet

![](image/Pasted%20image%2020241130001130.png)

