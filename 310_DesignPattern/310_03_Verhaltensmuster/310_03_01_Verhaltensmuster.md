
# 1 Observer

The Observer pattern (also known as "Observer" or "Listener ") is used when multiple objects continuously monitor the state of another object. When the state of the observed object changes, it should inform all of its observers so that they can reactively update themselves. The pattern thus corresponds to the principle: "Don't call us, we'll call you!"


https://blog.csdn.net/suifeng3051/article/details/51263718

被观察对象是Observable，观察者是Observer

观察者定义了一种一对多的依赖关系，当一个主题(Subject)对象状态发生变化时，所有依赖它的相关对象都会得到通知并且能够自动更新自己的状态，这些依赖的对象称之为观察者(Observer)对象这类似于发布/订阅模式。

观察者模式中的主题对象，会维持着一个依赖它的观察者对象列表，当主题对象状态发生改变时，主题对象便会调用这个列表中所有观察者对象的方法。

观察者模式一般用在分布式时间处理系统，它也是流行的MVC模型的核心设计模式。


观察者模式主要涉及到三个组件：Subject、Observer、ConcreteObserver
    主题（Subject）：保存了所有观察者的引用，并供注册、删除观察者的接口，提供自己状态变化触发所有观察者更新的方法
    观察者（Observer）：定义了更新自己状态的接口
    具体观察者（ ConcreteObserver）：具体实现观察者接口，使自己的状态和主题状态一致

其实观察者模式很简单，其核心内涵依然是用面向对象思想思考问题而非面向过程思想，面向接口编程，解耦合依赖。

但是观察者模式也有缺点，观察者模式是一种常用的链式触发机制，由于是链式触发，当观察者比较多的时候，性能问题是比较令人担忧的。并且，在链式结构中，比较容易出现循环引用的错误，造成系统假死。


## 1.1 引子

Beobachter (observer)
Problem:
• Mehrere Objekte sollen unmittelbar informiert werden, wenn sich eines ändert (Beobachtung)
• Das beobachtete Objekt (Subjekt) kann nicht vorhersehen, welche Beobachter es gibt
• Beobachter können wechseln

Lösung:
• Subjekt stellt Möglichkeit bereit, sich anzumelden (publish)
• Beobachter melden sich beim Subjekt an (subscribe/register)
• Subjekt aktualisiert angemeldete Beobachter bei Änderung (notify/update)


### 1.1.1 小例子

We can imagine the following example: The observed object is a date picker field. Its observers are a task list and an appointment list, which change their respective state (i.e., the tasks and appointments to be displayed) depending on the selected date.

The observed object (observable ) often stores data in the form of a `List<T>`or a `Map<T,T>`, which is displayed in the user interface (UI) at various points using different views , e.g., as a table or chart. These views in the UI are then the observers, which must update their display when the state of the observable changes. The basic goal of the observer pattern is therefore to decouple the observable and the observer from each other, which in practice often corresponds to the separation of the model and the observing views.

![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327144146.png]]


## 1.2 observer pattern is publish-subscribe pattern

The dependency between objects can be mapped in software design using different basic methods:

- **Pull** : The dependent objects are active and query the state of the observed object. If this state request is specifically triggered by a certain user interaction (e.g., opening a new page or a new dialog), the pull principle makes sense. The HTTP protocol functions exclusively according to the pull principle. The pull principle is less suitable if the state of the observable changes frequently and the observers always need the current state. Since the observers do not know when the state of the observable changes in this case, they must query at short, regular intervals, a process called _polling ._
- **Push** : The observed object is active and notifies its observers directly and unprompted by calling their corresponding methods. This is simple and efficient, but requires that the objects to be notified are known in advance and do not change at runtime. Since these two conditions do not apply in most use cases, the pull and push principles are combined into a bidirectional communication pattern: _publish-subscribe_ .
- **Publish-Subscribe** : The basic idea is to reverse the dependency by having interested observers register their desire to be notified of state changes with the observable ( _Subscribe_ ). All registered observers are notified by the observable when its state changes ( _Publish_ ).

The observer pattern is therefore also called the publish-subscribe pattern. The concept here is similar to a newsletter or newsfeed subscription: observers subscribe to messages from publishers who publish them occasionally. The observable is the sending publisher , and the observers are the receiving subscribers , who can unsubscribe if they are no longer interested in the messages.


## 1.3 Struktur 

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

![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327145141.png]]

An Observable provides methods for registering ( `addObserver`) and removing ( `deleteObserver`) observers and manages its observers in a corresponding list ( `observers`). The following UML sequence diagram illustrates the basic flow of the Observer pattern. As soon as the state of the concrete Observable is changed externally ( `setState`), the Observable informs all registered concrete observers about the method `notifyObservers`by `update`calling their method and passing the new state, as well as itself as the source of this new state.

In an alternative variant of the pattern, the method can `update`also be represented without arguments. In this case, after the notification, an observer must `getState`retrieve the new state from the observable itself using a corresponding method.


![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327150007.png]]







## 1.4 例子

### 1.4.1 例子 

![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327150313.png]]

#### 1.4.1.1 Observable 和 Observer 的实现

1 使用  java.util.Observable 和 java.util.Observer
To apply the original observer pattern, Java provides [`java.util.Observable`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Observable.html)and . However, both have been _deprecated_[`java.util.Observer`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Observer.html) since Java 9 and are therefore no longer recommended. The following code example demonstrates the use of (line 1) and (line 11), where is a functional interface [_,_](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/function/package-summary.html) meaning the interface specifies exactly one method and can therefore be implemented in lambda expressions.`Observable``Observer``Observer`[](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/function/package-summary.html)

All code examples for the Observer design pattern shown below can be found in the /patterns/observer directory of the module repository.

```java
class MyObservable extends Observable {
    Object state;

    void setState(Object newState) {
        this.setChanged();
        this.notifyObservers(state = newState);
    }

    public static void main(String[] args) {
        // Observer is implemented as a lambda expression
        Observer o1 = (source, newState) -> System.out.printf("Received new state = %s from %s\n", newState, source);

        MyObservable source = new MyObservable();
        source.addObserver(o1);

        List.of(1, 2, 3).forEach(i -> source.setState(i));
    }
}
```


2 使用 PropertyChangeListener

The following code example demonstrates the use of a PropertyChangeListeneras a newer alternative to java.util.Observer. Instead java.util.Observableof extending the list of registered observers and the method can notifyObserversalso be implemented independently (lines 3-7). In this case, the observers can `java.beans.PropertyChangeListenerimplement` the functional interface and PropertyChangeEventreceive a .

```java
class MySecondObservable {
    Object state;
    List<PropertyChangeListener> listeners = new ArrayList<>();

    void notifyListeners(String property, Object oldValue, Object newValue) {
        listeners.forEach(l -> l.propertyChange(new PropertyChangeEvent(this, property, oldValue, newValue)));
    }

    void setState(Object newState) {
        notifyListeners("state", state, state = newState);
    }

    public static void main(String[] args) {
        // Observer is implemented as a lambda expression
        PropertyChangeListener o1 = (PropertyChangeEvent e) ->
            System.out.printf("Received new state = %s [previous state = %s] from %s\n", 
                e.getNewValue(), e.getOldValue(), e.getSource());

        MySecondObservable source = new MySecondObservable();
        source.listeners.add(o1);

        List.of(1, 2, 3).forEach(i -> source.setState(i));
    }
}
```



```java
import java.beans.PropertyChangeEvent;
import java.beans.PropertyChangeListener;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class ObservableObject {
    private Object state;
    private final List<PropertyChangeListener> listeners = new CopyOnWriteArrayList<>();

    public void addListener(PropertyChangeListener listener) {
        listeners.add(listener);
    }

    public void removeListener(PropertyChangeListener listener) {
        listeners.remove(listener);
    }

    private void notifyListeners(String property, Object oldValue, Object newValue) {
        listeners.forEach(listener -> 
            listener.propertyChange(new PropertyChangeEvent(this, property, oldValue, newValue))
        );
    }

    public void setState(Object newState) {
        Object oldState = this.state;
        this.state = newState;
        notifyListeners("state", oldState, newState);
    }
}
```


#### 1.4.1.2 special UIs

This PropertyChangeListeneris a special . In Java, we often encounter special UIs as part of the observer pattern java.util.EventListenerin UI programming , for example, to observe various UI components such as text fields, buttons, and the like, or user interaction via keyboard, mouse, and the like. In JavaFX, the interface , which is also a specialization, is typically used to observe another object and respond to its state changes:EventListenerjavafx.event.EventHandlerEventListener

```java
interface EventHandler<T extends Event> extends EventListener {
    void handle​(T event);
}
```



The following code shows how to observe keyboard input, mouse movement, and a button in JavaFX. Here, Buttonand are Sceneobservables provided by JavaFX.

```java
Scene scene = new Scene(/* ... */);
scene.setOnKeyPressed((KeyEvent e) -> System.out.println("Pressed key: " + e.getCode()));

Button btn = new Button(/* ... */);
btn.setOnMouseEntered((MouseEvent e) -> System.out.println("Moved mouse over button"));
btn.setOnAction((ActionEvent e) -> System.out.println("Clicked button"));
```

#### 1.4.1.3 Subscriber and Publisher 

This class, new since Java 9, java.util.concurrent.Flowprovides the interfaces Publisherand Subscriberthrough which the observer pattern can be implemented as an asynchronous data stream . The following code demonstrates a basic use of the Flow API.


```java
class MySubscriber implements Subscriber<Integer> {
    Subscription subscription;

    @Override
    public void onSubscribe(Subscription subscription) {
        System.out.println("Subscription startet");
        this.subscription = subscription;
        subscription.request(1);
    }

    @Override
    public void onNext(Integer item) {
        System.out.printf("Received new state = %s\n", item);
        this.subscription.request(1);
    }

    @Override
    public void onError(Throwable throwable) { }

    @Override
    public void onComplete() { System.out.println("Subscription ended"); }
}
```



A specific subscriber must implement the methods onSubscribe, onNext, onErrorand onComplete. Upon initial call to onSubscribe(line 5), the subscriber receives an object of type Subscription, which it can use to control whether it wishes to receive further messages (method requestto be executed, lines 8 and 14) or not (method cancel). If a subscriber wishes to receive further messages, they are onNextdelivered to it via the method (line 12).

```java
class MyPublisher {
    public static void main(String[] args) throws InterruptedException {
        SubmissionPublisher source = new SubmissionPublisher<Integer>();
        source.subscribe(new MySubscriber());

        List.of(1, 2, 3).forEach(i -> source.submit(i));
        source.close();
        Thread.sleep(100); // wait for termination of subscriber thread
    }
}
```

The class `SubmissionPublisher` provides a standard implementation of the `Publisherinterface`. `submit` The publisher sends messages submitted via the method (line 6) to all currently registered subscribers until it closeis closed via the method (line 7). 

It is important to note that a publisher delivers messages asynchronously, meaning it does not wait for confirmation of receipt or processing from the subscribers, each of which runs in a parallel thread (see the chapter Threads in Java ). 

Each current subscriber receives newly submitted messages in the same order in which they are sent—except in the case of exceptions ( onError) or timeouts. 

If a message is sent via `offer` instead of `submit`, a timeout can be defined in case a subscriber takes too long to accept the message. In this way, publishers can function as non-blocking, reactive streams . 

To ensure that the subscriber thread has sufficient time to process the messages, the main thread in the above code sleeps for a while (line 8). This is not a good solution in practice, but rather an anti-pattern , but it helps to maintain the simplicity of the example.



### 1.4.2 例子


https://blog.csdn.net/suifeng3051/article/details/51263718


![[310_DesignPattern/image/20160427192029534.png]]

主题对象类
```java
import java.util.ArrayList;
import java.util.List;
public class Subject {
    //主题对象维持着一个依赖它的观察者对象列表
    private List<Observer> observers = new ArrayList<Observer>();
    //主题对象的状态
    private int state;
    //当主题对象状态变化时，调用所有观察者对象的方法
    public void setState(int state) {
        this.state = state;
        notifyAllObservers();
    }
    //调用所有依赖它的观察者的方法
    public void notifyAllObservers(){
        for (Observer observer : observers) {
            observer.update(state);
        }
    }
    //主题对象也可以注册、和删除依赖它的观察者对象
    public void attach(Observer observer){
        observers.add(observer);
    }

    public int getState() {
        return state;
    }
}
```


观察者接口
```java
public abstract class Observer {
    public abstract void update(int state);
}
```

具体的观察者
```java
public class ConcreteObserverA extends Observer{

    @Override
    public void update(int state) {
        System.out.println( "ConcreteObserverA get state change event: " + state );
    }
}
```

```java
public class ConcreteObserverB extends Observer{

    @Override
    public void update(int state) {
        System.out.println( "ConcreteObserverA get state change event: " + state );
    }
}
```


最后写一个测试类测试一下
```java
public class ObserverPatternDemo {
    public static void main(String[] args) {
        //初始化主题对象
        Subject subject = new Subject();
        //注册观察者对象到主题对象
        ConcreteObserverA ConcreteObserverA=new ConcreteObserverA();
        ConcreteObserverB ConcreteObserverB=new ConcreteObserverB();
        subject.attach(ConcreteObserverA);
        subject.attach(ConcreteObserverB);
        //主题对象状态发生变化
        subject.setState(20);

    }
}
```



# 2 strategy

不同的 strategy , 能够得到 相同的goal, 但是  过程的 perfoamce 不一样 

The Strategy design pattern (also known as "Strategy" or "Policy" ) addresses the fact that, even in software development, there can be different paths to the same goal. These paths correspond to algorithms that are implemented in methods and share a common interface. Simply put, the Strategy pattern simply emphasizes the consistent use of interfaces and the associated polymorphism in an object-oriented language.

If alternative algorithms exist that can achieve the desired result, the client should be able to decide at runtime which algorithm it wants to use as a solution strategy. The choice of the optimal strategy can vary depending on the context. For example, there may be a specific strategy A that delivers particularly accurate results, but requires significantly more resources (such as processor time or memory) than another strategy B, which means that its calculation takes longer or is more expensive. Strategy B, on the other hand, requires fewer resources but only delivers approximately accurate results, which may or may not be sufficient depending on the context. 


## 2.1 Struktur 

The strategy pattern allows the specific strategy to be changed at runtime. It is visualized in the following UML class diagram.

![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327160128.png]]

The application's surrounding context ( `Context`) does not implement the alternative algorithms itself, but only knows an interface `Strategy` that specifies the signature for the algorithm and `ConcreteStrategyA` is expressed at runtime by concrete strategies (e.g. ). 


## 2.2 Advantage

The advantages of this pattern are obvious:
- The application context is decoupled from the concrete strategies and is not dependent on them, ie `Context`there is no `import`statement for a concrete strategy in the class.
- For more complex algorithms, such as optimized sorting, searching, or hashing procedures, creating a strategy class for each algorithm promotes clarity. The specific algorithm can be tested and maintained independently of other aspects.
- The strategies become reusable and can also be used in other contexts.
- New strategies can be easily added, requiring virtually no changes to existing code.


## 2.3 例子

![[310_DesignPattern/310_03_Verhaltensmuster/image/Pasted image 20250327163332.png]]


This example can be easily applied to route optimization in a navigation system. Depending on the context, the search can be for the fastest, shortest, or cheapest route, meaning that the goal is to minimize either travel time, distance, or travel costs. Here, too, there are different concrete paths with a uniform interface for reaching the specified destination.

The code examples shown for the Strategy design pattern can be found in the /patterns/strategy directory of the module repository.


The following example illustrates the strategy pattern using a simple implementation. The distance between two points is to be determined, e.g., for navigation from a starting point to a destination. The algorithms implement three alternative distance functions in two-dimensional space: Euclidean distance , city-block distance , and Chebyshev distance . At runtime, the client tries all three strategies for measuring the distance between the points (0,0) and (3,4) one after the other and obtains different results with 5, 7, and 4.

```java
class Client {
    public static void main(String[] args) {
        Context context = new Context(new Point2D(0, 0), new Point2D(3, 4));

        context.strategy = new EuclideanDistance(); // set initial strategy
        System.out.println("Distance to destination is " + context.distance());

        context.strategy = new CityBlockDistance(); // change strategy at runtime
        System.out.println("Distance to destination is " + context.distance());

        context.strategy = new ChebyshevDistance(); // change strategy at runtime again
        System.out.println("Distance to destination is " + context.distance());
    }
}
```

```java
class Context {
    Point2D from, to;
    DistanceFunctionStrategy strategy;

    Context(Point2D from, Point2D to) { this.from = from; this.to = to; }

    Double distance() { return strategy.distance(from, to); }
}
```


```java
interface DistanceFunctionStrategy {
    Double distance(Point2D a, Point2D b);
}
```


```java
class EuclideanDistance implements DistanceFunctionStrategy {
    
	@Override
    public Double distance(Point2D a, Point2D b) {
        return Math.sqrt(Math.pow(a.getX() - b.getX(), 2) + Math.pow(a.getY() - b.getY(), 2));
    }
}
```


```java
class CityBlockDistance implements DistanceFunctionStrategy {
    
	@Override
    public Double distance(Point2D a, Point2D b) {
        return Math.abs(a.getX() - b.getX()) + Math.abs(a.getY() - b.getY());
    }
}
```


```java
class ChebyshevDistance implements DistanceFunctionStrategy {
    
	@Override
    public Double distance(Point2D a, Point2D b) {
        return Math.max(Math.abs(a.getX() - b.getX()), Math.abs(a.getY() - b.getY()));
    }
}
```