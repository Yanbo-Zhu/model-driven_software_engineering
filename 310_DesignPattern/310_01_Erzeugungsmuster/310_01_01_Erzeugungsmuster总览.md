


# 1 Singleton

==Let's start with one of the simplest creation patterns: the singleton . The singleton pattern is closely linked to the question of how to ensure at runtime that a maximum of one object of a class can be created in an object-oriented application.==

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

## 1.1 例子


==Let's start with one of the simplest creation patterns: the singleton . The singleton pattern is closely linked to the question of how to ensure at runtime that a maximum of one object of a class can be created in an object-oriented application.==

1 
The Singleton pattern is illustrated by the following example: Let's imagine that we want to establish a connection to a database in a Java application. To do this, we `DatabaseConnection`create the following class, which contains a method for establishing the connection and additional methods for querying and modifying the data. These specialized methods are hidden in the code listings below.

```java
public class DatabaseConnection {
	public boolean connect(String host, String user, String pass, String db) { /* ... */ }
	public ResultSet query(String sqlStatement) { /* ...*/ }
	/* ... */ 
}
```



2 
static 和 pricate 的使用 
“static 关键字的作用可以用一句话来描述：'方便在没有创建对象的情况下进行调用，包括变量和方法'。也就是说，只要类被加载了，就可以通过类名进行访问

It is sufficient if this database connection is only established once when the application starts, and therefore there is only one object of this class. This object corresponds to a global variable. It should not be possible for `DatabaseConnection`instances to be created from methods in any other classes in our project or by third parties who continue to use our class. To prevent this, the visibility of the class's constructor is set `DatabaseConnection`to `private`. This means that instances can only be created from the class itself. A static attribute of type is created in the class itself `DatabaseConnection`, which can be accessed via a static and public method. In the following code, the attribute is `instance`instantiated when the class is loaded.

```java
public class DatabaseConnection {
	// Singleton object
	private static DatabaseConnection instance = new DatabaseConnection();
	// Private constructor
	private DatabaseConnection() {}
	// Static access to singleton object
	public static DatabaseConnection getInstance() { return instance; }
	/* ... */ 	
}
```


3 
synchronized 的使用 
If the singleton object is to be created upon first access rather than upon class loading ( _lazy creation_ ), ==the access method must be protected by the Java keyword `synchronized`"critical section," so that only one parallel _thread_ can execute this method at a time==. Threads and critical sections are discussed later in the chapter " [Thread Synchronization](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-sync) . "

If the database connection example is generalized (i.e. the class is now called `Singleton`instead of `DatabaseConnection`), we get the common Singleton creation pattern, which ensures that at runtime a maximum of 1 object of a class can be instantiated.


```java
public class Singleton {
	// Singleton object
	private static Singleton instance;
	// Private constructor
	private Singleton() {}
	// Static access to singleton object with lazy creation
	public static synchronized Singleton getInstance() {
		if (instance == null) { instance = new Singleton(); }
		return instance;
	}
	/* ... */ 	
}
```


4 
The following model corresponds to the Java code above. Underlined attributes or methods in a UML class diagram indicate that they are static.  
![[310_DesignPattern/310_01_Erzeugungsmuster/image/Pasted image 20250325212228.png]]

The pattern implements simple access control to the singleton object. Subclassing allows a general singleton class to be specialized. However, the singleton pattern should not be used as a replacement for all global variables. If this is done excessively, a large number of classes would be created, but the object-oriented concept would still be undermined.