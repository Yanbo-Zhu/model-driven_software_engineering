
# 1 Composite

==The _Composite_ pattern is a structural pattern used to combine objects that share a common interface into a hierarchical tree structure. The basic idea of ​​the Composite pattern is to represent both primitive objects and composite objects, which consist of multiple primitive objects, in one interface==. This allows individual objects and their compositions to be treated consistently.

==The pattern is intended to answer the following question: How can individual objects be cleverly combined as parts (= components) to form a whole (= compound) in an object-oriented language?==

The derivation of the pattern will again be done using an example. The following implementation is provided to graphically represent the three basic shapes `Line`, , `Circle`and `Text`. Complex, composite shapes should be able to be created from these primitive shapes.

Line
```java
// line
class Line {
    GraphicsContext gc;
    Point2D a, b;
    void draw() { gc.strokeLine(a.getX(), a.getY(), b.getX(), b.getY()); }
}

class Circle {
    GraphicsContext gc;
    Point2D a;
    Double radius;
    void draw() { gc.strokeOval(a.getX(), a.getY(), radius, radius); }
}

class Text {
    GraphicsContext gc;
    Point2D a;
    String value;
    void draw() { gc.fillText(value, a.getX(), a.getY()); }
}
```

It's a good idea to specify a common interface for the forms that drawdefines the method for each form. Since all forms also share a common type attribute GraphicsContextand an interface is stateless, an abstract class is Shapeintroduced.

```java
abstract class Shape {
    GraphicsContext gc;
    abstract void draw();
}

class Line extends Shape {
    Point2D a, b;

    @Override
    void draw() { gc.strokeLine(a.getX(), a.getY(), b.getX(), b.getY()); }
}

class Circle extends Shape {
    Point2D a;
    Double radius;

    @Override
    void draw() { gc.strokeOval(a.getX(), a.getY(), radius, radius); }
}

class Text extends Shape {
    Point2D a;
    String value;

    @Override
    void draw() { gc.strokeText(value, a.getX(), a.getY()); }
}
```


The actual compound is a compound form, which in turn consists of the other forms available so far.
```java
class CompositeShape extends Shape {
    Set<Shape> components = new HashSet<>();
	
    @Override
    void draw() { components.forEach(c -> c.draw()); }
}

```


Examples of compound shapes are the rectangle shown below, which consists of 4 lines, and the stick figure, which consists of a circle and 4 lines.

Compound shapes
![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412112610.png]]


Because the compound can contain not only primitive forms but also other complex forms as parts, a part-whole hierarchy is created that is fundamentally unlimited in its depth.==The relationships between the objects involved represent a tree structure, as cyclical relationships are generally undesirable and should therefore be avoided when parts are combined to form a whole==. Because it is a tree structure, we also call the basic forms leaves . 

---

The following object diagram illustrates such a nested tree structure, which further uses the above forms. The resulting complex form is shown next to it. In this way, compositions of any complexity can be put together on the basis of just a few basic forms.

Composite form "Vitruvian Man"
![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412112752.png]]


---


The Composite design pattern is a generalization of the above example for 2-dimensional shapes. This means that the method to be implemented is called general `anyOperation`instead of `draw`, the common interface is called `Component`instead of `Shape`, etc. Methods are added to the Composite class to add or remove a component. The following UML class diagram of the Composite pattern corresponds to the code below.

Component 对应 上面的 shape 
Composite  对应 上面的 CompositeShape
lead 是一个单一的class, 不以组合的方式出现 , 不需要添加新的 method 

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412114852.png]]

```java
interface Component {
    void anyOperation();
}

class Leaf implements Component {
    @Override
    void anyOperation() { /* ... */ }
}

class Composite implements Component {
    Set<Component> components = new HashSet<>();
	
    @Override
    void anyOperation() { components.forEach(c -> c.anyOperation()); }
	
	void addComponent(Component c) { components.add(c); }
	void removeComponent(Component c) { components.remove(c); }
}
```


# 2 Adapter


An adapter helps connect two initially incompatible interfaces. The analogy of this design pattern to a power adapter for international travel is obvious. The power adapter makes it possible to connect a power outlet to a plug for an electrical device, even if the plug doesn't directly fit because it has a different interface.

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412115616.png]]

In software development, ==an adapter (or _wrapper_ ) is used when an existing class is to be used whose interface does not correspond to the intended interface==. ==The existing class should not or cannot be changed, especially if it is part of an external library developed by a third party==. The class to be adapted from the library is often of interest because it implements complex behavior that is to be reused. 
The principle of reuse is generally efficient when external libraries are integrated into a project that are known to have been extensively tested and that have already proven themselves in practice in other projects. In the Java environment, libraries are usually automatically integrated into a project from a [central repository](https://mvnrepository.com/) using build management tools such as [Maven](https://maven.apache.org/) or [Gradle](https://gradle.org/) .

客户端（Client）：使用目标接口，与和目标接口一致的对象合作。
适配器（Adapter）：负责将Adaptee的接口转换为Target的接口。
被适配者（Adaptee）：一个现存需要适配的接口。

## 2.1 理论 

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412115855.png]]

- 由 Adaptee class  create a object adapter (可以是 Objektadapter or Klassenadapter )
- Adapter 是 adapted class
- 在 object adapter  中 Adapter 中有一个 property with name adaptee . adaptee ist ein object of Adaptee class
- 在 class adapter 中 Adapter Class 是 subclass, Adaptee ist superclass . Adapter class extends Adaptee Class 

The adapter pattern exists in two variants: object adapter and class adapter. 
In both variants, the target interface is implemented Target by the adapter class . Furthermore, the adapter calls the method of the adapted class Adapter in the Target prescribed method . Of course, multiple methods can also be adapted.   

With the object adapter, there is an association between the adapter and the adapted class, whereas with the class adapter, the adapter extends the adapted class. The class Adapter cannot be implemented in Java if it is not an interface but a stateful class, as this would require multiple inheritance. 
The following UML class diagrams visualize object and class adapters in `comparison.operationserviceAdapteeTarget`


## 2.2 例子 

In the following example to motivate an adapter, we want to determine the area and perimeter of different 2-dimensional shapes. For this purpose, we have Shape specified the interface and already developed implementations for simple shapes such as circles, rectangles, and triangles.


```java
interface Shape {
    Double perimeter();
    Double area();
}


class Circle implements Shape {
    Point2D center;
    Double radius;

    Circle(Point2D center, Double radius) {
        this.center = center;
        this.radius = radius;
    }

    @Override
    public Double area() { return Math.PI * radius * radius; }

    @Override
    public Double perimeter() { return 2 * Math.PI * radius; }
}


class Rectangle implements Shape {
    Point2D topLeft;
    Double width, height;

    Rectangle(Point2D topLeft, Double width, Double height) {
        this.topLeft = topLeft;
        this.width = width;
        this.height = height;
    }

    @Override
    public Double perimeter() { return 2 * (width + height); }

    @Override
    public Double area() { return width * height; }
}


class Triangle implements Shape {
    Point2D p1, p2, p3; // points
    Double e1, e2, e3; // edges

    Triangle(Point2D p1, Point2D p2, Point2D p3) {
        this.p1 = p1; this.p2 = p2; this.p3 = p3;
        e1 = p1.distance(p2); e2 = p2.distance(p3); e3 = p3.distance(p1);
    }

    @Override
    public Double perimeter() { return e1 + e2 + e3; }

    @Override
    public Double area() {
        Double s = perimeter() / 2;
        return Math.sqrt(s * (s - e1) * (s - e2) * (s - e3));
    }
}

```


用 polygon 去 implement the Shape interface  

We use the interface Shape( ) in our application `Client` to iterate over different shapes and process them using their common interface. We'll also Shape add an implementation for arbitrary polygons. A polygon consists of a series of points connected by edges. ClientTo illustrate this, we'll calculate the area and perimeter of the following four shapes.


![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412125205.png]]

Determining the area of ​​a polygon is not trivial. For this reason, we want to integrate the [Apache Commons Math](http://commons.apache.org/proper/commons-math/) library , which provides an API for many mathematical fundamentals. We are interested in the method `getSize`of the class [`PolygonsSet`](http://commons.apache.org/proper/commons-math/apidocs/org/apache/commons/math4/geometry/euclidean/twod/PolygonsSet.html)that can be used to calculate the area of ​​a polygon. 

The code example shows how the polygon can be implemented using an object adapter ( `PolygonObjectAdapter`) or, alternatively, a class adapter ( `PolygonClassAdapter`). The class `Client`shows that the new adapter classes also `Shape`implement the interface.


```java

import org.apache.commons.math3.geometry.euclidean.twod.PolygonsSet;  // 注意这里 
import org.apache.commons.math3.geometry.euclidean.twod.Vector2D;

class PolygonObjectAdapter implements Shape {
    PolygonsSet adaptee;

    PolygonObjectAdapter(Point2D... points) {
        Vector2D[] vectors = Stream.of(points).map( p -> new Vector2D(p.getX(), p.getY())).toArray(size -> new Vector2D[size]);
        adaptee = new PolygonsSet(.1, vectors);
    }

    @Override
    public Double perimeter() { return adaptee.getBoundarySize(); }

    @Override
    public Double area() { return adaptee.getSize(); }
}
```


> class PolygonClassAdapter extends PolygonsSet implements Shape  用 PolygonClassAdapter 去使用两者的 property 

```java
import org.apache.commons.math3.geometry.euclidean.twod.PolygonsSet; // 注意这里 
import org.apache.commons.math3.geometry.euclidean.twod.Vector2D;

class PolygonClassAdapter extends PolygonsSet implements Shape {

    PolygonClassAdapter(Point2D... points) {
        super(.1, Stream.of(points).map(p -> new Vector2D(p.getX(), p.getY())).toArray(size -> new Vector2D[size]));
    }

    @Override
    public Double perimeter() { return this.getBoundarySize(); }

    @Override
    public Double area() { return this.getSize(); }
}
```

```java
class Client {

    public static void main(String[] args) {
        Shape s1 = new Rectangle(new Point2D(0, 0), 3., 2.);
        Shape s2 = new Triangle(new Point2D(0, 0), new Point2D(0, 2), new Point2D(1, 1));
        Shape s3 = new PolygonObjectAdapter(new Point2D(0, 0), new Point2D(3, 0), new Point2D(3, 2), new Point2D(0, 2), new Point2D(1, 1));
        Shape s4 = new PolygonClassAdapter(new Point2D(0, 0), new Point2D(3, 0), new Point2D(2, 1), new Point2D(3, 2), new Point2D(0, 2), new Point2D(1, 1));

        List.of(s1, s2, s3, s4).forEach(s -> {
            System.out.printf("%s: perimeter = %.2f, area = %.2f\n", s.getClass().getSimpleName(), s.perimeter(), s.area());
        });
    }
}
```

The code examples shown for the Adapter design pattern can be found in the /patterns/adapter directory of the module repository.

# 3 Facade 

Facade 的作用就是 暴露接口给外部 

Like the adapter, the facade_ is also a wrapper class. While the adapter helps call methods from external components and integrate them into the system, ==the goal of the facade is to make the system easy to access from the outside by external components (= _clients_ )==. 
==The facade therefore designs the interface through which external access occurs==. As a rule, not all methods should be made available to the outside with their internal signature. The following figure outlines the idea of ​​a facade in a UML component diagram.

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412163825.png]]

A facade decouples the components of the developed system from its clients. The clients may only access the system via the facade. Behind the facade there are several components or subsystems, which in turn have defined interfaces and embody ( v. 具体表现，体现；收录 ) the system itself as building blocks. The components are often much more complex than a client requires. They can be implemented in different programming languages, using different frameworks and be provided in heterogeneous runtime environments (see the chapter [Modularization and Architecture](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-architecture) ). ==The facade, in turn, accesses these components and hides their complexity from the clients==. A method of the facade can, for example, combine a logical sequence of internal method calls into a higher-level functionality.

In practice, facade classes are very common, as almost every system grows in complexity over time, eventually requiring a facade to manage this internal complexity. A software system typically evolves organically, with different developers adding new classes, relationships, and methods at different points. Over the course of its continuous development, the system is gradually broken down into components maintained by different teams.

Facades can also be helpful when multiple components need to be reintegrated later because they contain similar functionalities. ==The facade then provides a uniform interface through which external access can be delegated to the existing components, while the components slowly merge behind the facade==.

In summary, there are various reasons for using a facade:
- A client should communicate with a simple interface. The complexity behind the facade is abstracted so that the client doesn't have to deal with it.
- Behind a facade, the components can generally be further developed (especially refactored) without the client noticing. The facade's interface should remain as stable as possible, while its implementation adapts to changes in the internal components.
- In general, a facade supports the principle of loose coupling between components to reduce their interdependencies.==Loose coupling facilitates the porting and replication of individual components in a distributed system.== 
	- Porting means that a component is made accessible under a different address, for example, because the host changes from a dedicated server in the company's own data center to a virtual server at a cloud service provider. 
	- Replication means that a component is instantiated multiple times in the distributed system in order to be able to handle a higher system load if necessary.
- A facade can also serve a security purpose. The system is easier to protect if access is only granted through the facade. The facade, in turn, protects the clients from incorrect use, i.e., from otherwise possible combinations and parameterizations of method calls that make no sense from a business perspective.
- _Cross-cutting concerns_ can be handled by a facade, e.g. authentication and authorization, session management, general validations, logging or monitoring.

---

The UML component diagram shown in the figure above is to be implemented using the [modules](https://www.informatik-aktuell.de/entwicklung/programmiersprachen/java-9-das-neue-modulsystem-jigsaw-tutorial.html) (= components) introduced with Java 9. Since its inception, the Java programming language has offered the option of restricting the visibility of classes, their attributes and methods, etc. By default, visibility is limited to content within its own package. This default can be adjusted using the well-known modifiers `public`, `private`and `protected`. 

Since Java 9, there have been modules at a higher level of abstraction that correspond to the components of the architecture and through which visibility can also be controlled. These modules usually contain several packages and specify in their module descriptor ( `module-info.java`), on the one hand which other modules they depend on ( `requires <<module>>`) and on the other hand which of their packages they release to other modules externally ( `exports <<package>>`). 
By convention, modules in Java are written in lowercase, just like packages, and are referred to with an inverted domain as an identifying prefix. The domain prefix is ​​omitted here for simplicity.

Client Module
```java
// module-info.java
module client {
    requires mySystem;
}

// Client.java
package client;
import facade.Facade;

public class Client {
    public static void main(String[] args) {
        System.out.println(Facade.helloFromX());
        System.out.println(Facade.welcomeHelloFromY());
        System.out.println(Facade.welcomeHelloFromXY());
    }
}
```

MySystem module 
```java
// module-info.java
module mySystem {
    requires componentX;
    requires componentY;
    exports facade;
}

// Facade.java
package facade;
import x.X2;
import y.Y1;

public class Facade {
    public static String helloFromX() { return X2.hello("X2"); }
    public static String welcomeHelloFromY() { return Y1.welcome() + Y1.hello(); }
    public static String welcomeHelloFromXY() { return Y1.welcome() + X2.hello("X2") + Y1.hello(); }
}
```


ComponentX Module
```java
// module-info.java
module componentX {
    exports x to mySystem;
}

// X2.java
package x;

public class X2 extends X1 {
    public static String hello(String from) { return "Hello from " + from + ". "; }
    X3[] x3;
}
```


ComponentY Module
```java
// module-info.java
module componentY {
    exports y to mySystem;
}

// Y1.java
package y;

public class Y1 {
    public static String welcome() { return "Welcome! "; }
    public static String hello() { return "Hello from " + Y1.class.getSimpleName() + ". "; }
}
```

- The module `client`can only `mySystem`access the components contained as building blocks via the module.
- The client cannot import the contents of the packages in the modules `componentX`because they are only released for the facade ( ). `componentY` `exports x to mySystem`
- The facade, on the other hand, exports the contents of its package publicly to any interested clients ( `exports facade`).
- In the class `Facade`, it can be seen that in the methods of the facade several methods from the components can be combined and their parameterization can be restricted.

The code examples shown for the Facade design pattern can be found in the /patterns/facade directory of the module repository.


# 4 Proxy 

> The purpose of a proxy is to prevent a client from communicating directly with a target object. Instead, communication between the client and the target object always runs through the intermediary proxy. This may manipulate, record, delay, completely block, or otherwise corrupt the client's original request.

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412174555.png]]

proxy 完全的实现了某个 interface 中的全部method, 但是不添加新的 method.  proxy 起到的作用是 授权 client's request and transmit it into target object 
==The proxy is a class that fulfills the same interface as the actual target object and can therefore be used in its place.==  
The following UML class diagram illustrates the proxy pattern. The proxy delegates the client's request to the target object `RealObject`, which is known to it for this purpose (attribute `Proxy.realObject`, see method implementation `Proxy.operation`). ==Unlike the adapter, the proxy does not modify the interface but implements it completely, adding additional functionality to reduce the load on the target object==. 

![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412174713.png]]

一定要有个好的理由 才值得去 添加proxy, 否则就节省他 
The proxy pattern initially has one clear disadvantage: The proxy class creates additional code, which increases the complexity of the call stack and the associated testing and maintenance effort. Therefore, ==there must be good reasons for the additional level of control provided by the proxy==, which are presented as examples below. The prerequisite for the need for a proxy class is that the underlying class cannot or should not be modified.
- **Protection proxy** : A protection proxy intercepts access to the real target object and, if necessary, restricts it after verifying authorization. This type of proxy can be used when the class to be secured cannot be modified because its code is inaccessible or changes are not permitted (e.g., due to licensing reasons).
- **Remote proxy** : A remote proxy is a representative object on the client side when the client calls methods on remote objects that reside in a different runtime environment (e.g. a remote JVM). The other runtime environment runs in a different process and address space, usually even on a different host, and offers a _remote service_ that can be called from outside (see chapter [Remote Method Invocation](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-rmi) and [SOAP Web Services](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-soap) ). T==he remote proxy is the endpoint for this interprocess communication in the client’s address space – as a representative of the remote object in the remote address space==. The proxy is responsible for packing the input arguments when a method is called on the remote object into a serialization format (e.g. `java.io.Serializable`) and for unpacking the return argument when the response is received. The proxy delegates the necessary network communication to the runtime environment or the operating system and takes care of errors such as _timeouts_ .
- **Virtual proxy** : A virtual proxy is used when creating the target object is very complex, i.e. it requires a lot of resources. In this case, ==the proxy can delay the complete creation of the target object or break it down into individual sub-step. Because the proxy controls access to the target object, it can only create its individual parts when needed, i.e. when accessed for the first time (= _lazy loading_ )==. A virtual proxy is suitable when the actual target object is large and cumbersome, e.g. when reading a complex object from a very large file or database. A virtual proxy can often be usefully combined with a remote proxy when communicating over a network connection with a limited data transfer rate, e.g. when using a mobile phone with poor reception.

## 4.1 例子 

In the following code example illustrating the proxy pattern, the function of the target object is `RealVideo`to load and play a video file from the web. The video is loaded in the class constructor `RealVideo`(line 6). Playback begins when the method is called `play`(lines 10-14).  The interface `Video`only specifies this method `play`, which is also implemented by the proxy class `ProxyVideo`. The proxy class changes the behavior of the target object in two ways:
- The loading of the video is delayed and does not take place in the constructor, but only directly before playing in the method `play`(lazy loading).
- Videos shorter than 30 seconds will not be played at all because (in this case) they are advertisements.


```java
class RealVideo implements Video {

    MediaPlayer player;

    RealVideo(String src) {
        player = new MediaPlayer(new Media(src));
    }

    @Override
    public void play(MediaView mediaView) {
        mediaView.setMediaPlayer(player);
        System.out.println("Start playing " + player.getMedia().getSource());
        player.play();
    }
}



interface Video {
    void play(MediaView mediaView);
}



class ProxyVideo implements Video {

    RealVideo video;
    String src;

    ProxyVideo(String src) {
        // video is not loaded here
        this.src = src;
    }

    @Override
    public void play(MediaView mediaView) {
        // lazy loading >> video is loaded only when played and not in constructor
        if (video == null) {
            video = new RealVideo(src);
            video.player.setOnReady(() -> checkBeforePlay(mediaView));
        }
        else { checkBeforePlay(mediaView); }
    }

    void checkBeforePlay(MediaView mediaView) {
        // don't play short advertising videos (duration < 30 s)
        Double duration = video.player.getMedia().getDuration().toSeconds();
        if (duration < 30) {
            System.out.printf("Ignored an advertising video %s (%.2f s)\n", src, duration);
            return;
        }
        video.play(mediaView);
    }
}


public class Client extends Application { // the client is a JavaFX application

    final static String VIDEO_URL = "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/";

    public static void main(String[] args) { launch(args); } // JavaFX application launcher

    @Override
    public void start(Stage stage) {
        MediaView mediaView = new MediaView();

        List<String> filenames = List.of("BigBuckBunny.mp4", "ElephantsDream.mp4", "ForBiggerBlazes.mp4", "ForBiggerEscapes.mp4");
        List<Video> videos = filenames.stream().map(filename -> new ProxyVideo(VIDEO_URL + filename))
            .collect(Collectors.toList());  // In this line, the constructor call could be replaced `new ProxyVideo`with `new RealVideo`to avoid using the proxy, thus preloading the videos and playing the short advertisements instead of skipping them.

        Button b = new Button("Play next video");  // The client launches a simple JavaFX application that plays a random video each time the user clicks a button (lines 15-19 in `Client`). 
        b.setOnAction(e -> {
            Video anyVideo = videos.get(new Random().nextInt(videos.size()));
            anyVideo.play(mediaView);  // play random video
        });

        Scene scene = new Scene(new VBox(mediaView, b));
        stage.setScene(scene);
        stage.show();
    }
}

```


The client launches a simple JavaFX application that plays a random video each time the user clicks a button (lines 15-19 in `Client`). Before doing so, a list of `ProxyVideo`objects is populated (line 12). In this line, the constructor call could be replaced `new ProxyVideo`with `new RealVideo` to avoid using the proxy, thus preloading the videos and playing the short advertisements instead of skipping them.


The following UML class diagram corresponds to the code example for the proxy pattern.
![[310_DesignPattern/310_02_StructuralPattern_StrukturMuster/image/Pasted image 20250412194131.png]]


The code examples shown for the Proxy design pattern can be found in the /patterns/proxy directory of the module repository.


