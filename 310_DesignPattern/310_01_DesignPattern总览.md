

TU Berlin Softwaretechnik 课程

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
![[310_DesignPattern/image/Pasted image 20250219162404.png]]

| Erzeugungsmuster                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Strukturmuster                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Verhaltensmuster                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - [_Singleton_ (Einzelstück)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-singleton)<br>- [_Factory Method_ (Fabrikmethode)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-factory_method)<br>- [_Abstract Factory_ (abstrakte Fabrik)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-factory_method)<br>- _Builder_ (Erbauer)<br>- _Prototype_ (Prototyp) | - [_Composite_ (Kompositum)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-composite)<br>- [_Adapter_ (Adapter)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-adapter)<br>- [_Facade_ (Fassade)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-facade)<br>- [_Proxy_ (Stellvertreter)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-proxy)<br>- _Decorator_ (Dekorierer)<br>- _Bridge_ (Brücke)<br>- _Flyweight_ (Fliegengewicht) | - [_Observer_ (Beobachter](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-observer))<br>- [_Strategy_ (Strategie)](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-strategy)<br>- _State_ (Zustand)<br>- _Command_ (Kommando)<br>- _Memento_ (Memento)<br>- _Visitor_ (Besucher)<br>- _Iterator_ (Iterator)<br>- _Interpreter_ (Interpreter)<br>- _Template Method_ (Schablonenmethode)<br>- _Mediator_ (Vermittler)<br>- _Chain of Responsibility_ (Zuständigkeitskette) |


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
![[310_DesignPattern/image/Pasted image 20250219162547.png]]


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
![[310_DesignPattern/image/Pasted image 20250219164140.png]]

![[310_DesignPattern/image/Pasted image 20250219164153.png]]



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
![[310_DesignPattern/image/Pasted image 20250219165429.png]]

Vorteile
• vereinfacht den Client-Code
• neue Komponenten können leicht hinzugefügt werden

![[310_DesignPattern/image/Pasted image 20250219165247.png]]


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
![[310_DesignPattern/image/Pasted image 20250219165403.png]]

• Proxy und „echte“ Klasse erben von abstrakten Typ
• Proxy reicht Abfragen weiter und fügt eigene Funktionalität hinzu


---

Beispiel 

![[310_DesignPattern/image/Pasted image 20250219165744.png]]


Beispiel: Virtual Proxy
• ganzes Bild laden ist aufwendig
• Proxy stellt Thumbnail zur Verfügung 先载入小图片 
• lädt echtes Bild nur, wenn nötig

![[310_DesignPattern/image/Pasted image 20250219165811.png]]

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
![[310_DesignPattern/image/Pasted image 20250219170004.png]]

• Die Beobachter erweitern die abstrakte Klasse Observer und werden beim Subjekt registriert
• Das Subjekt führt die Aktualisierung in allen Beobachtern mit „notify“ durch




Beispiel: GUI mit MVC
• Model ist hier Subjekt
• Alle GUI-Elemente mit Inhalten aus dem Model sind Observer 
• Auch der Controller kann Observer sein
• Grund: View und Controller sollten unmittelbar über Änderungen informiert werden
• Für Model nicht klar, welche GUI-Elemente es gibt

model 发生了什么改变 , like通知所有的 observer. 但是 model 自己不知道有那些observer 

![[310_DesignPattern/image/Pasted image 20250219170049.png]]


---

被观察对象是Observable，观察者是Observer

https://blog.csdn.net/suifeng3051/article/details/51263718

观察者定义了一种一对多的依赖关系，当一个主题(Subject)对象状态发生变化时，所有依赖它的相关对象都会得到通知并且能够自动更新自己的状态，这些依赖的对象称之为观察者(Observer)对象这类似于发布/订阅模式。

观察者模式中的主题对象，会维持着一个依赖它的观察者对象列表，当主题对象状态发生改变时，主题对象便会调用这个列表中所有观察者对象的方法。

观察者模式一般用在分布式时间处理系统，它也是流行的MVC模型的核心设计模式。


观察者模式主要涉及到三个组件：Subject、Observer、ConcreteObserver
    主题（Subject）：保存了所有观察者的引用，并供注册、删除观察者的接口，提供自己状态变化触发所有观察者更新的方法
    观察者（Observer）：定义了更新自己状态的接口
    具体观察者（ ConcreteObserver）：具体实现观察者接口，使自己的状态和主题状态一致

其实观察者模式很简单，其核心内涵依然是用面向对象思想思考问题而非面向过程思想，面向接口编程，解耦合依赖。

但是观察者模式也有缺点，观察者模式是一种常用的链式触发机制，由于是链式触发，当观察者比较多的时候，性能问题是比较令人担忧的。并且，在链式结构中，比较容易出现循环引用的错误，造成系统假死。



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

![[310_DesignPattern/image/Pasted image 20250219170355.png]]




