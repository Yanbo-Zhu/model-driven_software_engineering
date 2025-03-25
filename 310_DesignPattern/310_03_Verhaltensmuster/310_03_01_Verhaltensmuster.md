
# 1 Observer


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



## 1.1 例子

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




