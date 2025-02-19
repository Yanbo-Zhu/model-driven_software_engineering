
Implementierung stellt eigene Anforderungen an das Modell
• Durch Vielfalt der Probleme/Lösungsmöglichkeiten sind Vorgaben an Implementierung schwierig „Gute Erfahrungen“ in Mustern beschrieben
• Muster für Gesamtstruktur: Architekturstile
• Muster für wiederkehrende Probleme: Design Patterns

Generische Lösung für wiederkehrendes Entwurfsproblem
- Erfahrungen mit erfolgreichen Lösungsansätzen übertragbar machen
- Deutsch: Entwurfsmuster
- Überschneidungen mit Architekturstilen möglich
	- Ist MVC Architekturstil oder Design Pattern?


Design Patterns: Elements of Reusable Object-Oriented Software.
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219162404.png]]

# 1 Erzeugungsmuster 

## 1.1 Singleton


Singleton
Problem
• für manche Klassen ist nur eine Instanz sinnvoll: Beispiel: Dateisystem
• sicherstellen, dass wirklich nur eine Instanz erzeugt werden kann
• globale/statische Variable für die Instanz reicht nicht

Lösung
• die Klasse muss selbst dafür sorgen, dass sie (oder Subklassen) nur einmal instanziiert wird


Logging
Logger schreibt applikationsweit in die gleiche Datei
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219162547.png]]


Beispielhafte Implementierung in JAVA
• Privater Konstruktor nicht von außen erreichbar
• Instanziierung in einer statischen Methode versteckt
• erstellt die Instanz beim ersten Aufruf (lazy initialization)

```java
public classMySingleton{
	// static singleton instance
	private static MySingletoninstance= null;
	
	privateMySingleton() { } // private constructor
	public static synchronized MySingletongetInstance() {
		if(instance== null) {
			instance= newMySingleton();
		}
		
		return instance;
	}
}
```


Alternativ können Singletons in JAVA über Aufzählungstypen gelöst werden
• Enums können auch Methoden und Attribute haben
• Werden bei der ersten Verwendung initialisiert

```java
public enum Logger {
	INSTANCE;

	private String logFile;
		
	public void log (String message) {
		//....
	}
	//....
}
//....
Logger.INSTANCE.log("...");
```

Singletons wurden als Object in SCALA übernommen

## 1.2 Builder

Problem
• Viele optionale Parameter im Konstruktor schlecht darstellbar
• Die Reihenfolge ist nicht offensichtlich und erschwer die Lesbarkeit
• Benötigt werden benannte optionale Parameter mit Default-Werten

Lösung
• Konstruktor wird von Builder aufgerufen
• Builder enthält Initialwerte und
• stellt Funktionen für optionale Parameterübergabe vor dem Aufrufen des Konstruktors bereit

---

Telescoping

Teleskopkonstruktor
• Konstruktor für jede Kombination von Parametern

• Skaliert schlecht
• Schlecht erweiterbar
• Parameter hängt von Reihenfolge ab
• Konstruktoren können nicht verschieden benannt werden


```java
public class Cake {
    private finalintsugar; // required
    private finalintflour; // required
    private finalintbutter; // optional
    private finalintchocolate; // optional
    
    publicCake(ints, intf, intb, intc) {
	    this.sugar= s; this.flour= f;
	    this.butter= b; this.chocolate= c;
    }
    publicCake(ints, intf, intb) {
	    this(s, f, b, 0);
    }
    publicCake(intsugar, intflour) {
	    this(sugar, flour, 0);
    }
}
```




Java Beans
• Objekt-Konstruktor und Setter-Methoden
• Erlaubt ungültige Zwischenzustände des Objekts
• Viel Schreibarbeit beim Erzeugen

```java
public class Cake {
	privat eintsugar= -1; // required
	privat eintflour= -1; // required
	privat eintbutter= 0; // optional
	privat eintchocolate= 0; // optional
	public Cake() {} // standard constructor
	
	public void setSugar(ints) {this.sugar= s;}
	public void setFlour(intf) {this.flour= f;}
	public void setButter(intb){this.butter= b;}
	public void setChocolate(intc) {this.chocolate= c;}
	// ......
}
```

---

Builder 的例子 
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219164140.png]]

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219164153.png]]



Erstellung von Cake komfortabel mit
```
Cake dry   = newCake.Builder(500, 500).build();
Cake yummy = newCake.Builder(500, 500).butter(250).chocolate(200).build();
```


• optionale Parameter in benannten Funktionen
• Default-Werte für nicht gesetzte Parameter
• Keine inkonsistenten Zwischenzustände
• Reihenfolge der optionalen Parameter irrelevant


# 2 Strukturmuster
## 2.1 Composite

Kompositum (composite)

Problem
• Daten sind hierarchisch organisiert (baumförmig)
• Programm führt auf allen Knoten gleichartige Operation aus (Baum-Traversierung)

Lösung
• Composite definiert Hierarchien, die aus komplexen Objekten (composites) und einfachen Objekten bestehen
• für das Programm transparent, was für ein Objekt behandelt wird (gemeinsame abstrakte Operation für alle Knoten)

Struktur 
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219165429.png]]

Vorteile
• vereinfacht den Client-Code
• neue Komponenten können leicht hinzugefügt werden

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219165247.png]]


## 2.2 Proxy

Stellvertreter (proxy)
代理class 就是 在这个class 中 造一些功能 , 访问借口 以便能 通过 proxy 去 访问 RealSubject 

Problem
• Ein Zugriff/Verbindung zu einem Objekt kann durch einen Pointer nicht ausreichend dargestellt werden
• Zugriffsoperationen sind komplexer oder nur Teilmengen der Zugriffsmöglichkeiten sollen erlaubt sein

Lösung
• Proxy-Klasse ersetzt die tatsächliche Klasse an der Stelle der Verwendung
• Kapselt Zugriffe und implementiert zusätzliche Funktionalität


Struktur 
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219165403.png]]

• Proxy und „echte“ Klasse erben von abstrakten Typ
• Proxy reicht Abfragen weiter und fügt eigene Funktionalität hinzu


---

Beispiel 

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219165744.png]]


Beispiel: Virtual Proxy
• ganzes Bild laden ist aufwendig
• Proxy stellt Thumbnail zur Verfügung 先载入小图片 
• lädt echtes Bild nur, wenn nötig

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219165811.png]]

---

Anwendungsbeispiele
• Remote Proxy: Lokale Repräsentation eines Remote-Objekts (andere Bezeichnung: Botschafter)
• Virtual Proxy: Die Erzeugung des Objektes ist aufwendig, aber nicht immer notwendig. Proxy erstellt das Objekt erst bei Bedarf
• Protection Proxy: Bietet eingeschränkten Zugriff auf Objekte, die größeren Schutz benötigen
• Smart Reference: Führt zusätzliche Aktionen beim Zugriff aus (z.B. Zugriffszähler)

Kann in PYTHON auch für Properties eingesetzt werden




# 3 Verhaltensmuster

## 3.1 Observer


Beobachter (observer)
Problem:
• Mehrere Objekte sollen unmittelbar informiert werden, wenn sich eines ändert (Beobachtung)
• Das beobachtete Objekt (Subjekt) kann nicht vorhersehen, welche Beobachter es gibt
• Beobachter können wechseln

Lösung:
• Subjekt stellt Möglichkeit bereit, sich anzumelden (publish)
• Beobachter melden sich beim Subjekt an (subscribe/register)
• Subjekt aktualisiert angemeldete Beobachter bei Änderung (notify/update)


Struktur 
![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219170004.png]]

• Die Beobachter erweitern die abstrakte Klasse Observer und werden beim Subjekt registriert
• Das Subjekt führt die Aktualisierung in allen Beobachtern mit „notify“ durch




Beispiel: GUI mit MVC
• Model ist hier Subjekt
• Alle GUI-Elemente mit Inhalten aus dem Model sind Observer 
• Auch der Controller kann Observer sein
• Grund: View und Controller sollten unmittelbar über Änderungen informiert werden
• Für Model nicht klar, welche GUI-Elemente es gibt

model 发生了什么改变 , like通知所有的 observer. 但是 model 自己不知道有那些observer 

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219170049.png]]




## 3.2 Kommando (command)

Problem:
• Eine Anweisung soll nicht nur ausgeführt, sondern auch verwaltet werden

Beispiele:
• Verzögerung einer Ausführung durch Warteschlangen
• Parametrierung von Objekten (Clients) mit Anforderungen
• Aufzeichnung von Anforderungen

Lösung:
• Command: Interface für die Ausführung von Operationen
• Client erstellt Command statt eine Operation direkt zu starten
• Tatsächliche Ausführung der Funktionalität verzögert bzw. indirekt


Beispiel: Bildbearbeitung
• Kommandos werden im GUI-Framework für Buttons konfiguriert
• Aufzeichnung aller Kommandos für Rückgängig-Funktion

![[300_Softwaretechnik/DesignPattern/image/Pasted image 20250219170355.png]]




