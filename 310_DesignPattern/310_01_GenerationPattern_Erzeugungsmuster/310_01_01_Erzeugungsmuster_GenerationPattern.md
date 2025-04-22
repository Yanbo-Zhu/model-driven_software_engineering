


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
![[310_DesignPattern/310_01_GenerationPattern_Erzeugungsmuster/image/Pasted image 20250325212228.png]]

The pattern implements simple access control to the singleton object. Subclassing allows a general singleton class to be specialized. However, the singleton pattern should not be used as a replacement for all global variables. If this is done excessively, a large number of classes would be created, but the object-oriented concept would still be undermined.


# 2 Builder 

Our final pattern, **the Builder Pattern,** is also a **creation pattern** . The **Builder Pattern** consists of two parts: the concrete Builder, which can assemble a complex construct from objects, and the (optional) Director, which controls the Builder.

**Changing requirements**  
With the expansion of orders, we have created many combinations and want to offer customers the opportunity to choose a menu and get it at a lower price.

**Menus**

- Burger, salad and fries menu (discount €1.50)
    - A burger of your choice
    - lettuce
    - Fries with ketchup or mayonnaise
    - A cola or lemonade

- Burger and fries menu (discount €1.00)
    - A burger of your choice
    - Fries with ketchup or mayonnaise
    - A cola or lemonade

- Salad and fries menu (discount €1.00)
    - lettuce
    - Fries with ketchup or mayonnaise
    - A cola or lemonade

- French Fries Menu (Discount €0.50)
    - Fries with ketchup or mayonnaise
    - A cola or lemonade


**The revised software design. In the** [Composite](https://isp.eduloop.de/loop/Composite_Pattern_\(dt._Kompositum\) "Composite Pattern")  
subsection , we already created the _**MainOrder**_ class , which can accept partial orders. In the _**Factory**_ subsection , we created special factories for all order types, which simplify the creation of the objects.[](https://isp.eduloop.de/loop/Composite_Pattern_\(dt._Kompositum\) "Composite Pattern")

We now need two more things

- The orders must be able to be combined into a menu. To do this, we create the classes _**MenuBuilderInterface**_ , _**MenuBuilder**_ , and _**MenuDirector**_ .
- The calculation of the discount for a menu is done by a _**DiscountDecorator**_


---

MenuBuilderInterface and MenuBuilder
The MenuBuilderInterface defines, as always, the necessary methods thatmust be programmed in the MenuBuilder .

MenuBuilderInterface
```php
<?php  declare ( strict_types  =  1 );
/**
* Interface for menu builders.
*/

interface  MenuBuilderInterface
{
    public  function  getMenu () :  OrderInterface ;
    public  function  setBurger ( OrderInterface  $burger ) :  void ;
    public  function  setSalad ( OrderInterface  $salad ) :  void ;
    public  function  setFries ( OrderInterface  $fries ) :  void ;
    public  function  setDrink ( OrderInterface  $drink ) :  void ;
}
```


MenuBuilder

```php
<?php  declare ( strict_types  =  1 );
/**
* Builds a menu with the given orders.
*/

class  MenuBuilder  implements  MenuBuilderInterface
{
    protected  $burger ;
    protected  $drink ;
    protected  $fries ;
    protected  $salad ;
    protected  $customer ;

    public  function  __construct ( string  $customer )
    {
        $this -> customer  =  $customer ;
    }

    public  function  getMenu () :  OrderInterface
    {
        $menu  =  new  MainOrder ( $this -> customer ,  $this -> getOrders ());

        return  new  DiscountDecorator ( $menu ,  $this -> getDiscount ());
    }

    protected  function  getOrders () :  array
    {
        $orders  =  [];
        if  ( $this -> burger )  {
            $orders [] =  $this -> burgers ;
        }
        if  ( $this -> drink )  {
            $orders []  =  $this -> drink ;
        }
        if  ( $this -> fries )  {
            $orders []  =  $this -> fries ;
        }
        if  ( $this -> salad )  {
            $orders []  =  $this -> salad ;
        }

        return  $orders ;
    }

    protected  function  getDiscount () :  int
    {
        $discount  =  0 ;
        if  ( $this -> drink  instanceof  OrderInterface )  {
            if  ( $this -> fries  instanceof  OrderInterface )  {
                $discount  +=  50 ;
            }
            if   ( $this -> salad  instanceof  OrderInterface )  {
                $discount  +=  50 ;
            }
            if  ( $this -> burger  instanceof  OrderInterface )  {
                $discount  +=  50 ;
            }
        }

        return  $discount ;
    }

    public  function  setBurger ( OrderInterface  $burger ) :  void
    {
        $this -> burger  =  $burger ;
    }

    public  function  setDrink ( OrderInterface  $drink ) :  void
    {
        $this -> drink  =  $drink ;
    }

    public  function  setFries ( OrderInterface  $fries ) :  void
    {
        $this -> fries  =  $fries ;
    }

    public  function  setSalad ( OrderInterface  $salad ) :  void
    {
        $this -> salad  =  $salad ;
    }

}

'''''MenuDirector'''''  controls  the  factories  and  creates  the  menu .
< source  lang = "php"  line >
<? php  declare ( strict_types  =  1 );
/**
* Directs the creation of a menu.
*/

class  MenuDirector
{
    protected  $burgerFactory ;
    protected  $drinkFactory ;
    protected  $friesFactory ;
    protected  $saladFactory ;
    protected  $menuBuilder ;


    public  function  __construct (
        BurgerFactory  $burgerFactory ,
        DrinkFactory  $drinkFactory ,
        FriesFactory  $friesFactory ,
        SaladFactory  $saladFactory ,
        MenuBuilderInterface  $menuBuilder
    )  {
        $this -> burgerFactory  =  $burgerFactory ;
        $this -> drinkFactory  =  $drinkFactory ;
        $this -> friesFactory  =  $friesFactory ;
        $this -> saladFactory  =  $saladFactory ;
        $this -> menuBuilder  =  $menuBuilder ;
    }


    public  function  createOrder (
        string  $customer ,
        ? string  $burger ,
        ? array  $burgerExtras ,
        ? string  $fries ,
        ? array  $friesExtras ,
        ? string  $salad ,
        ? array  $saladExtras ,
        ? string  $drink ,
        ? array  $drinkExtras
    ) :  OrderInterface  {
        if  ( is_string ( $burger ))  {
            $this -> menuBuilder -> setBurger (
                $this -> burgerFactory -> createOrderForCustomer (
                    $burger ,  $customer ,  $burgerExtras
                )
            );
        }

        if  ( is_string ( $drink ))  {
            $this -> menuBuilder -> setDrink (
                $this -> drinkFactory -> createOrderForCustomer (
                    $drink ,  $customer ,  $drinkExtras
                )
            );
        }

        if  ( is_string ( $fries ))  {
            $this -> menuBuilder -> setFries (
                $this -> friesFactory -> createOrderForCustomer (
                    $fries ,  $customer ,  $friesExtras
                )
            );
        }

        if  ( is_string ( $salad ))  {
            $this -> menuBuilder -> setSalad (
                $this -> saladFactory -> createOrderForCustomer (
                    $salad ,  $customer ,  $saladExtras
                )
            );
        }

        return  $this -> menuBuilder -> getMenu ();
    }

}
```


The Decorator classes always increase the price per ingredient for the products. Now we need a corresponding DiscountDecorator to provide the discount.
```php
<?php  declare ( strict_types  =  1 );
/**
* Substrates the given discount from the order.
*/

class  DiscountDecorator  extends  AbstractOrderDecorator
{
    protected  $discount ;

    public  function  __construct ( OrderInterface  $order ,  int  $discount )
    {
        parent :: __construct ( $order );
        $this -> discount  =  $discount ;
    }

    public  function  getPrice () :  int
    {
        return  parent :: getPrice ()  -  $this -> discount ;
    }
}
```


**Adapting the main program:**  
We can now replace the _**createOrder function with the method of the**_ _**MenuDirector**_ class instance . We pass all factories and the _**MenuBuilder**_ into it via the constructor.

```php
if  ( $customer  &&  ( $burger  ||  $fries  ||  $salad  ||  $drink ))  {
    $menuDirector  =  new  MenuDirector (
        new  BurgerFactory (),
        new  DrinkFactory (),
        new  FriesFactory (),
        new  SaladFactory (),
        new  MenuBuilder ( $customer )
    );

    $order  =  $menuDirector -> createOrder (
        $customer ,
        $burger ,
        $burgerExtras ,
        $fries ,
        $friesExtras ,
        $salad ,
        $saladExtras ,
        $drink ,
        $drinkExtras
    );

    printOrderSummary ( $order );
}  else  {
    printOrderForm ();
}
```
Calling _**createOrder**_ returns an instance of OrderInterface, so the rest of our main program continues to work the same way.




# 3 Factory method

The **Factory Pattern** , also called Abstract Factory Pattern, creates related classes at runtime and thus belongs to the group **of creation patterns** .

The _factory method_ is a creation pattern that describes how ==an object is created by calling a method instead of a constructor==. This method is part of a so-called factory class, which is responsible for creating objects. It is misleading that, in common usage among software developers, the factory method describes both any static method for creating objects and one of the original GoF design patterns.


## 3.1 Static factory method

> A class can provide a public static factory method, which is simply a static method that returns an instance of the class." (Joshua Bloch)
> the factory class itself does not need to be instantiated to create objects,  and no interface needs to be defined for the creating method

The following code example illustrates the static factory method using the class , which can create `LoggerFactory`two different loggers (namely, a `ConsoleLogger`and a ) (lines 5-12). An argument is used to determine which specific logger is created. This is the default if the method is called without an argument (lines 14-16). This default could also be read from a configuration file or linked via _dependency injection_ .`FileLogger``ConsoleLogger``createLogger`

LoggerFactory
```java
public class LoggerFactory {

    public enum LogType {CONSOLE, FILE};

    public static MyLogger createLogger(LogType type) {
        if (type.equals(LogType.CONSOLE)) {
            return new ConsoleLogger();
        } else if (type.equals(LogType.FILE)) {
            return new FileLogger();
        }
        return null;
    }

    public static MyLogger createLogger() {
		return createLogger(LogType.CONSOLE); // default logger
    }
}
```

myLogger 
```java
interface MyLogger {
    void log(String message);
    default void close() {}
}
```

ConsoleLogger 
```java
public class ConsoleLogger implements MyLogger {
	
    @Override
    public void log(String message) {
        System.out.println(message);
    }
}
```

FileLogger
```java
public class FileLogger implements MyLogger {

    BufferedWriter writer;

    @Override
    public void log(String message) {
        try {
            if (writer == null) {
                writer = new BufferedWriter(new FileWriter("log.txt", true));
            }
            writer.append(message + "\n");
        } catch (IOException e) {}
    }

    @Override
    public void close() {
        try {
            writer.close();
        } catch (IOException e) {}
    }
}
```

Client
```java
class Client {

    public static void main(String[] args) {
        MyLogger a = new ConsoleLogger(); // constructor instantiation >> causes dependency to ConsoleLogger
        a.log("Hello World!");

        MyLogger b = LoggerFactory.createLogger(); // static factory method instantiation
        b.log("Hello World!");
    }
}
```

---

分析 
The interface `MyLogger` specifies the common interface for all specific loggers, which implement it individually. Unlike simply instantiating a logger via a constructor, when using the factory method, t==he client is no longer dependent on a specific logger class, but can `MyLogger` operate exclusively on the interface.==

The strength of this pattern lies in its simplicity. Compared to the GoF Factory Method design pattern (see below), the factory class itself does not need to be instantiated to create objects, and no interface needs to be defined for the creating method. A static factory method naturally assumes that the programming language `static`supports class methods (≙ keyword in Java).

The strength of this pattern lies in its simplicity. Compared to the GoF Factory Method design pattern (see below), ==the factory class itself does not need to be instantiated to create objects,  and no interface needs to be defined for the creating method==. A static factory method naturally assumes that the programming language `static`supports class methods (≙ keyword in Java).

The static factory method can also make use of the Java Reflection API, which is shown in the following code example (line 4).
```java
public class ReflectionLoggerFactory {

    public static MyLogger createLogger(Class<? extends MyLogger> loggerClass) {
        return loggerClass.getConstructor().newInstance(); // create instance by default constructor reflection
    }

    public static MyLogger createLogger() {
        return createLogger(ConsoleLogger.class); // default logger
    }	
}
```
---

The static factory method can also make use of the Java Reflection API, which is shown in the following code example (line 4).

```java
public class ReflectionLoggerFactory {

    public static MyLogger createLogger(Class<? extends MyLogger> loggerClass) {
        return loggerClass.getConstructor().newInstance(); // create instance by default constructor reflection
    }

    public static MyLogger createLogger() {
        return createLogger(ConsoleLogger.class); // default logger
    }	
}
```

Java Reflection API（反射机制）是一种**在运行时**查看和操作 Java 程序中类、方法、字段等结构的能力。简单来说，就是让程序自己「照镜子」，动态地了解自己的结构，并对其进行操作。

具体来说，Java 反射可以做什么？
1. **获取类的信息**：比如类名、父类、实现的接口等。
2. **访问字段和方法**：包括私有的字段和方法。
3. **调用方法或构造对象**：即使你在编写代码时并不知道这个类的具体名字。
4. **操作注解**：可以读取和处理类、方法、字段等上的注解信息。
5. **动态加载类**：比如通过类的全限定名加载某个类（常用于插件、框架中）

The real Java Logging API also uses a static factory method `getLogger` in the class `java.util.logging.Logger`to create a logger object:

```java
import java.util.logging.Logger;
// ...
Logger logger = Logger.getLogger(getClass().getName());
logger.log(Level.INFO, "Hello World!");
```


## 3.2 Design pattern factory method

Now we want to extend the existing static factory method to the GoF Factory Method design pattern. The goal of the pattern is to allow new classes to be added to an existing interface or inheritance hierarchy by third parties, and objects of these new classes to be created via an associated factory method. We can imagine, for example, that a third party `MyLogger`might want to implement another logger for the above interface, e.g., a `JDBCLogger`or a `RedisLogger`to store the log in a database. However, ==the third party does not have access to our code, especially not to the class `LoggerFactory`. Therefore, the must `LoggerFactory`itself be extensible from outside.== A solution could look like this, where the existing class is renamed `LoggerFactory`to `LoggerCreator`:

![[310_DesignPattern/310_01_GenerationPattern_Erzeugungsmuster/image/Pasted image 20250410110919.png]]


LoggerCreator
```java
abstract class LoggerCreator {

    Set<MyLogger> loggers;

    abstract MyLogger createLogger();

    MyLogger register() {
        MyLogger logger = createLogger();
        loggers.add(logger);
        return logger;
    }
}
```


ConsoleLoggerCreator
```java
class ConsoleLoggerCreator extends LoggerCreator {

    @Override
    MyLogger createLogger() { return new ConsoleLogger(); }
}
```


Client
```java
class Client {

    public static void main(String[] args) {
        LoggerCreator creator = new ConsoleLoggerCreator();

        MyLogger a = creator.register();
        a.log("Hello World!");

        MyLogger b = creator.register();
        b.log("Hello World!");

        System.out.println("Created "+ creator.loggers.size() + " loggers until now.");
    }
}
```

LoggerCreator 是个抽象类, 其中 abstract method createLogger 需要被拓展   . 拓展的基础是 已经有various concrete loggers (such as the JDBCLogger).  这个 concrete logger 需要去 

Superclass LoggerCreator 
- contain methods that implement behavior that is the same for all loggers - here, for example, the method register
- 但是 createLogger 是abtractor, 需要被 concrete Subclass LoggerCreator 去 override , 具体的override 中用的是具体的 logger class 而不是  abstract class Creator 

The abstract class `LoggerCreator`defines an abstract method `createLogger`. However, there is no longer a static method for creating the known concrete loggers. It is assumed that various concrete loggers (such as the `JDBCLogger`) will emerge in the future and be implemented by third parties. The focus is on extensibility. The superclass for object creation `LoggerCreator`can also contain methods that implement behavior that is the same for all loggers - here, for example, the method `register`. Only the concrete creators (such as `JDBCLoggerCreator`) depend on an associated logger implementation (such as `JDBCLogger`).

---

> "Define an interface for creating an object, but let subclasses decide which class to instantiate. The factory method lets a class defer instantiation it uses to subclasses." (Erich Gamma et al.)

定义一个用于创建对象的接口，但由子类决定要实例化哪个类。
工厂方法让类将实例化的工作延迟到其子类中进行。

![[310_DesignPattern/310_01_GenerationPattern_Erzeugungsmuster/image/Pasted image 20250410113915.png]]

interface Product 负责构建一个 class with commen arttibute 
factory class (就是 Creator ) 负责构建 technical behavior (Method )

In this pattern, there is a general superclass (or interface) for creating similar objects ( `Creator`). The concrete producers are prescribed a factory method ( `factoryMethod`) for object creation. The superclass has no dependency on a concrete producer or concrete product==. This pattern is particularly useful if the superclass contains another method in which it implements behavior that applies equally to every product==, regardless of the concrete type of product. In the UML class diagram above, this is the method `anyOperation`. 
From an object-oriented point of view, this shared behavior actually belongs more to the interface `Product`than to the factory class. ==In practice, however, the factory class may also implement technical behavior for the products, since the latter are reduced to simple model classes that only capture state via their attributes and are not intended to contain technical methods==. 

The disadvantage of the factory method design pattern is that two parallel specialization hierarchies must be maintained. This is the price for third-party extensibility. In contrast, a static factory method is always preferable as a simpler approach when all product classes are known in advance or when third-party extensibility is not important.

If the products to be created differ in the necessary arguments (就是 product 这个 interface 中 针对不同情况 必须包含不同的 argument ) when calling the constructor and therefore cannot agree on a common interface for instantiation, the Builder design pattern can [be _used_ ,](https://en.wikipedia.org/wiki/Builder_pattern) possibly in combination with the factory method.


## 3.3 Abstract Factory

If the factory class is intended to create not just one product, but an entire family of related products, the factory method is quickly expanded into the [Abstract Factory _design_ pattern. A popular example of this in the context of UI frameworks is the creation of UI elements (such as buttons, text fields, etc.), each of which is intended to follow a specific design style (](https://en.wikipedia.org/wiki/Abstract_factory_pattern) such as Cupertino or Material Design). 

This creates a separate specialization hierarchy for each product in a product family. The concrete products in a family are all created from a common factory class, which ensures a consistent design style. The client works exclusively on abstract products, which it creates using a concrete factory class. The idea of ​​the Abstract Factory is illustrated by the following UML class diagram and the corresponding code example.

![[310_DesignPattern/310_01_GenerationPattern_Erzeugungsmuster/image/Pasted image 20250412084713.png]]


Abstract factory/product
```java
interface AbstractFactory {
    Button createButton();
    TextField createTextField();
}

interface Button {} // abstract product
interface TextField {} // another abstract product
```


Specific factory/products
```java
class MaterialFactory implements AbstractFactory {

    @Override
    public Button createButton() { return new MaterialButton(); }

    @Override
    public TextField createTextField() { return new MaterialTextField(); }
}

class MaterialButton implements Button {} // concrete product
class MaterialTextField implements TextField {} // another concrete product
```



Client
```
AbstractFactory factory = new MaterialFactory();
Button button = factory.createButton();
TextField textField = factory.createTextField();
```


# 4 Dependecy Injection 

用这个的目的是 decoupling oder loose coupling 
_Dependency injection_ is best translated as "introducing dependencies" and is a well-known term in software development. Like the factory method, it involves outsourcing constructor calls for object creation. ==The goal of dependency injection is `import`to reduce the dependencies between classes created by statements, thereby decoupling the classes.== [Loose coupling](https://en.wikipedia.org/wiki/Loose_coupling) of classes (or components in general) has the advantage that changes can be implemented more easily, as they only have a local effect.

Dependency injection 不同于 inversion of Control 
Dependency injection is not a GoF pattern. Martin Fowler coined the term in his article ["Inversion of Control Containers and the Dependency Injection pattern"](https://martinfowler.com/articles/injection.html) [[Fow04]](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#cite-Fow04) . He was looking for a term for decoupling object creation that could be distinguished from the general term _Inversion of Control (IoC)_ (see chapter [Spring](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-spring) ). Dependency injection follows the single [_-_](https://en.wikipedia.org/wiki/Single_responsibility_principle) [responsibility principle [](https://en.wikipedia.org/wiki/Single_responsibility_principle) [Mar18]](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#cite-Mar18) , which, according to Robert Martin, states that each class in object-oriented programming should fulfill only one essential task and evolve accordingly:

> "There should never be more than one reason for a class to change." (Robert Martin)

---

Therefore, the situation in the following code example should be avoided: The class `MyClass`makes extensive use of the interface `MyLogger`, but, due to a direct constructor call `ConsoleLogger`, also depends on the concrete class that implements this interface. If the concrete class is to be replaced, the class must `MyClass`be modified, even though its actual functionality remains unchanged.

因此，应当避免下面代码示例中的这种情况：类 `MyClass` 虽然大量使用了接口 `MyLogger`，但由于直接调用了具体类 `ConsoleLogger` 的构造函数，它实际上也依赖于这个接口的具体实现类。
如果将来需要更换这个具体类（例如用 `FileLogger` 替换 `ConsoleLogger`），就必须修改 `MyClass` 的代码，尽管它本身的功能并没有改变。

这段话强调了**直接依赖具体实现**带来的问题：
- **违背了依赖倒置原则**（DIP）；
- **增加了代码耦合**，降低了可维护性和可扩展性；
- 应该通过**工厂方法模式**或**依赖注入**等方式，将具体实现的创建与使用分离。

```java
import MyLogger;
import ConsoleLogger; // this dependency should be avoided

class MyClass {
    MyLogger logger = new ConsoleLogger();
	
	// ...
}
```

The goal of dependency injection is to resolve this dependency. In the simplest case, this is done via a constructor or a setter method.


```java
import MyLogger; // no import of concrete implementation required   这是一个 dependency

class MyClass {
    MyLogger logger;
	
	MyClass(MyLogger logger) { this.logger = logger; } // constructor dependency injection, 参数中注入了 这个dependency 
	
	void setLogger(MyLogger logger) { this.logger = logger; } // setter dependency injection
	
	// ...
}
```


Now the question arises as to who creates the object and `MyClass`injects it into the class from outside. In general, ==the aim is to consolidate the configuration regarding which concrete implementation class should be used for each interface in a central location, e.g. in a configuration class or in a configuration file (e.g. XML, YAML)==. 
Such a configuration is usually provided and managed by a framework in its function as _an IoC container_ . The approaches can differ slightly from framework to framework. The following section presents the dependency injection approaches of [Spring](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies) and [Google Guice](https://github.com/google/guice/wiki/GettingStarted) . Both use the Java Reflection API to determine which concrete class can be bound to an interface. Guice refers to this as _binding_ , while the corresponding term in Spring is _wiring_ .

现在就会出现一个关键问题：**由谁来创建对象并将其注入到 `MyClass` 中？**
通常，目标是**在一个集中式的位置统一管理接口所对应的具体实现类的配置**，例如在一个配置类中，或通过配置文件（如 XML、YAML）进行管理。这样可以将对象创建的责任从使用者那里剥离出去，增强系统的灵活性和可维护性。
在实际开发中，这类配置通常由某个**框架**来提供和管理，此框架的角色就是所谓的 **IoC（Inversion of Control）容器**，即**控制反转容器**。IoC 容器的核心职责是负责对象的**生命周期管理**和**依赖注入**，从而降低系统模块之间的耦合度。

不同的框架在实现 IoC 的方式上可能略有差异。以下部分将会介绍 **Spring** 和 **Google Guice** 两个框架的依赖注入机制：
- 在 **Google Guice** 中，这一过程称为 **binding（绑定）**，开发者需显式地在模块中指定哪个具体类绑定到某个接口；
- 而在 **Spring Framework** 中，对应的术语是 **wiring（装配）**，可以通过注解（如 `@Autowired`）或 XML 配置来完成自动装配。
值得注意的是，这两个框架都利用了 **Java 的反射 API**，通过运行时反射机制来确定哪一个具体类应当被注入到某个接口类型中。


## 4.1 Spring

In order for the Spring Framework to inject a class's objects into constructors, methods, or directly into attributes, they must be made known to the framework as so-called _[beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html)_ . Spring defines beans as follows, with the framework itself acting as an IoC container:

> In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. ==A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container==. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container." (Spring Docs)

All classes annotated with the annotation `@Component`(from the package `org.springframework.stereotype`) or one of its specializations such as `@Service`, , `@Repository`or `@Controller`, ==are automatically recognized and registered as beans by Spring. Registered beans can be accessed at runtime via the application context==. 

### 4.1.1 @Component
In the following code example, an object of the class should be `CapsLockConsoleLogger`able to be included as a bean in other locations. Therefore, the class is `@Component`annotated as (line 1).

```java
@Component
class CapsLockConsoleLogger implements MyLogger {

    @Override
    public void log(String message) { System.out.println(message.toUpperCase()); }
}
```


### 4.1.2 @SpringBootApplication and @Autowired 

In the following class, `SpringClient`this bean is injected into the attribute `MyLogger logger`(lines 4-5), meaning this is precisely where dependency injection occurs. Although ==no constructor call is visible==, the logger can be used later (line 13). The annotation ==`@Autowired`ensures that the framework uses reflection to `MyLogger`search for a suitable bean for the interface and binds this bean to the attribute==. It is important that exactly one suitable bean is found—not none or multiple.


这段很重要 解释 @SpringBootApplication 的作用 
The class `SpringClient`here is a simple [Spring Boot application](https://spring.io/guides/gs/spring-boot/) , which is annotated as such (line 1) and started as usual (line 8). ==Calling creates `SpringApplication.run()`an `ApplicationContext`object that represents the Spring IoC container and serves as the central application context through which all beans are managed and accessible==.

```java
@SpringBootApplication
class SpringClient { // this example is a Spring Boot application

    @Autowired
    MyLogger logger;

    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(SpringClient.class, args);
    }

    @Bean
    CommandLineRunner run(ApplicationContext ctx) {
        return args -> logger.log("Hello World!");
    }
}
```


### 4.1.3 @configuration and @Bean and @Qualifier

If multiple beans satisfy the same interface, a qualifying name must be used to determine which bean the framework should inject. For this purpose, a configuration class can be created—such as the following class `MyConfiguration`, which defines the three beans (2 of type `MyLogger`, 1 of type `String`).


myLogger 
```java
interface MyLogger {
    void log(String message);
    default void close() {}
}
```


MyConfiguration
```java
@Configuration
class MyConfiguration {

    @Bean
    MyLogger loggerA() { return new CapsLockConsoleLogger(); }

    @Bean
    MyLogger loggerB() { return new TimestampConsoleLogger(dateFormat()); }

    @Bean 
	String dateFormat() { return "yyyy-MM-dd HH:mm:ss"; }
}
```


TimestampConsoleLogger
```java
@Component
class TimestampConsoleLogger implements MyLogger {

    SimpleDateFormat dateFormat;

    TimestampConsoleLogger(String pattern) { dateFormat = new SimpleDateFormat(pattern); }

    @Override
    public void log(String message) { System.out.println(dateFormat.format(new Date()) + "\t" + message); }
}
```


CapsLockConsoleLogger
```java
@Component
class CapsLockConsoleLogger implements MyLogger {

    @Override
    public void log(String message) { System.out.println(message.toUpperCase()); }
}
```



之后 使用的时候可以 

The beans can @Qualifierbe injected via an annotation and their method name as follows.
```java
@Autowired @Qualifier("loggerA")
MyLogger logger;
```

Alternatively, a bean can be retrieved from the application context using its method name.
```java
logger = ctx.getBean("loggerA", MyLogger.class); // request bean from application context
logger.log("Hello World!");
```


Instead of in the configuration class, MyConfigurationthe beans can also be defined in a corresponding XML configuration file, which must then be loaded as an application context or added to it.

### 4.1.4 define bean in a xml configuration file 

Instead of in the configuration class, MyConfigurationthe beans can also be defined in a corresponding XML configuration file, which must then be loaded as an application context or added to it.

beans.xml
```xml
 <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	   
    <bean id="loggerA" class="CapsLockConsoleLogger"></bean>

    <bean id="loggerB" class="TimestampConsoleLogger">
        <constructor-arg ref="dateFormat"></constructor-arg>
    </bean>

    <bean id="dateFormat" class="java.lang.String">
        <constructor-arg value="yyyy-MM-dd HH:mm:ss"></constructor-arg>
    </bean>
	
</beans> 
```


SpringXMLConfigClient
使用定义在 xml 中的 bean 
```java
class SpringXMLConfigClient {

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
        MyLogger logger = ctx.getBean("loggerA", MyLogger.class);
        logger.log("Hello World!");
    }
}
```


## 4.2 Google Guice

Google Guice offers a proven alternative for dependency injection if a project intentionally chooses not to use Spring. When Guice was released by Google in 2008, it was the first framework to enable dependency injection in Java using annotations. In Guice, the configuration class that binds interfaces to concrete implementation classes is called a module.

> "Guice uses bindings to map types to their implementations. A module is a collection of bindings specified using fluent, English-like method calls." (Guice Docs)


The following example code shows the module MyModule that binds the interface MyLogger to its implementation CapsLockConsoleLogger.

```java
class MyModule extends AbstractModule { // a Guice configuration module

    @Override
    protected void configure() {
        bind(MyLogger.class).to(CapsLockConsoleLogger.class);
		// more bindings follow here ...
    }
}
```


This configuration class is used in the following code example to Injector create an object (line 9). The objects created later via getInstancethis object's method (line 12) can have their dependencies injected Injector via the annotation —for example, with the attribute (lines 3-4). @InjectMyLogger logger


```java
class GuiceClient {

    @Inject
    MyLogger logger;

    public static void main(String[] args){

        // create an injector based on module configuration
        Injector injector = Guice.createInjector(new MyModule());

        // create objects using the injector
        GuiceClient client = injector.getInstance(GuiceClient.class);
        client.logger.log("Hello World!");
    }
}
```


The dependency injection code examples shown can be found in the /patterns/dependency-injection directory of the module repository.



