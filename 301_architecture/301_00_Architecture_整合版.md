
来自 MODEL-DRIVEN SOFTWARE ENGINEERING 这门课 chapter12  和 
aus Softwaretechnik chapter 11 Implementierung  和
BHT pattern and framework 的课件 


---

Architektur spielt für die Implementierung eine wesentliche Rolle
• Eine klare Struktur sollte frühzeitig gewählt werden
• Verwendung bewährter Architekturstile empfohlen
• Funktionales Verhalten möglicherweise mit mehr Architekturstilen abbildbar als nicht-funktionale Anforderungen

Innerhalb von Systemen können Architekturstile gemischt auftreten

Einzelne Komponenten verteilter Systeme können wiederum jeweils eine eigene Architektur haben


# 1 SYSTEM ARCHITECTURE ARCHITECTURAL CONCEPTS

System Architecture:
− A structured and abstract description of a system, its elements and their relations and interactions
− Exact contents of a system architecture depends on the context/project
− Describes the elements and their properties
− Describes also the context of a system
− E.g. interaction with external systems
− Other software requirements
− Hardware requirements
− Sometimes additional information regarding aspects like security, scalability, operations, maintenance

# 2 Principles of Software Engineering

The purpose of a software architecture is aligned with the general principles of software engineering. According to Ghezzi et al. [[GJM03]](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#cite-GJM03) , these principles include :

- **Abstraction** : An architecture should provide a holistic overview of an application system and, to do so, must be generalizable. Important aspects must be identified and represented, while unimportant aspects must be hidden. The level of abstraction is adapted to the purpose of the representation. Abstraction is fundamentally necessary to manage complexity.
- **Modularization** : A complex system is divided into components/modules. Hierarchy allows modules to be broken down into finer sub-modules, and encapsulation allows a module to deliberately limit the external visibility of the details of its implementation. When dividing modules, the goal is high cohesion and low coupling. High cohesion means that there is a close relationship between the components within the module, e.g., many dependencies between classes within a module. Low coupling means that there are as few dependencies as possible between the modules via their externally visible interfaces.
- _Separation of_ **concerns** : The goal is for each module to be responsible for a distinct aspect , if possible. If the aspects are clearly separated, responsibility for the development of the modules can also be divided and parallelized. Since the holistic architectural model serves as a means of communication for various stakeholders, it should be possible to view it from different aspect-oriented perspectives. An early proposal in this regard is the ["4+1 Architectural View Model](https://en.wikipedia.org/wiki/4%2B1_architectural_view_model) ," which proposes the following four mutually consistent views of an architecture and then combines them in selected application scenarios for illustration (+1):
    - _Logical View_ : The functionality of the system from the user's perspective is represented, e.g. as a sequence or class diagram in UML.
    - _Development View_ : The hierarchical structure and dependencies of the system are represented from the developer's perspective, e.g. as a component or package diagram in UML.
    - _Process View_ : The dynamic behavior of the system at runtime is represented, ie in particular parallelism and synchronization, e.g. as an activity diagram in UML.
    - _Physical View_ : The distribution of components across runtime environments, particularly virtual and dedicated servers, from the perspective of the system administrator responsible for operations is represented, e.g. as a distribution diagram in UML.
- **Generality :** _At_ the beginning of a software project , the question arises as to how individual the architecture of the system to be constructed needs to be. If a proven framework already exists as a suitable architectural template, it is generally advisable to build on it. When reusing proven components, it can generally be assumed that the effort required for an in-house implementation will be significantly higher than the time required to familiarize yourself with the API of the reused component. In addition, reuse can avoid vulnerabilities that would otherwise frequently occur. The advantages and disadvantages of an individual architectural design compared to an adapted standard architecture must be carefully weighed. It should be noted that the architecture of a new system is manifested by the company's own design decisions.
- **Incrementality** : Incrementality is based on the assumption that changes cannot be avoided during development and therefore implies an iterative approach. As long as the application system is continually being developed, its architecture is usually not permanently stable. The evolution of an application system leads to differences in what is known as prescriptive and descriptive architecture. Prescriptive architecture describes what the architects planned at design time, while descriptive architecture expresses what can actually be observed during runtime, e.g. through dynamic code analysis using suitable [application performance monitoring tools](https://www.gartner.com/reviews/market/apm) such as [AppDynamics](https://www.appdynamics.com/) , [Dynatrace](https://www.dynatrace.de/) or [New Relic](https://newrelic.com/) . We speak of software erosion when the target state (prescriptive) and the actual state (descriptive) of the architecture differ significantly from one another.
- Anticipation _of_ **Change** : Building on the incremental development of a system, it is important to anticipate potential future changes as early as possible and incorporate them into the architecture. A good architecture is flexibly expandable, yet still as easy to manage as possible. The reasons for changes vary widely, e.g., eliminating errors (corrective maintenance), improving non-functional features (perfect maintenance), and adapting functionality to changing conditions (adaptive maintenance).
- **Accuracy and formality** : The goal here is not to stifle creativity, but to capture the results of creative design phases as precisely as possible, e.g., using standardized notations. Accuracy creates trust in a design. Formality is the highest degree of accuracy, although each organization or team must define an appropriate standard for itself. This raises questions such as: Is verification necessary for certain algorithms? How systematically are test data and test cases generated? How thorough is the documentation of activities?



## 2.1 MODULARITY

− Decomposition based on function (functional decomposition)
	− Discrete, disjunctive functions
	− Interaction via interface (black box model)
− Advantages:
	− Cope with complexity
	− Restriction of impact of failures
	− Flexibility and (re-)use
− Different levels of granularity
	− Objects
	− Source Code
	− System vs. Sub systems

## 2.2 DISTRIBUTION

− Spatial partitioning
	− Location based distribution of modularized functionality
	− Needs communication mechanisms between those modules
− Advantages
	− Allows to build different kinds of applications
	− Flexible localisation of computing resources
	− Performance improvement is possible
		− Use of more computing resources (Scalability)
		− Trade-off between local computation and communication overhead
	− Access Control
		− Data may stay in local database but computation results may be shared


# 3 Component-based development

aus BHT Pattern und Framework 

Component-based development expresses the paradigm that makes software engineering a true engineering science. The central idea lies in the a forementioned principle of modularization, i.e., the hierarchical division of the system into its components, and the associated reusability of components in other projects. This creates an analogy to classical engineering disciplines, in which it is common practice to reuse previously tested/approved components and best practices. For reusability, it is important that each component implements and encapsulates a clearly defined aspect, if possible. There can be multiple components that serve the same substantive purpose. ==If a common interface is specified for these components, they are easily interchangeable as alternative implementations==. Linking several basic components to form a composite component or ultimately to an overall system remains an architectural task, in which the [composite design pattern](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-composite) is reflected.

![[301_architecture/image/Pasted image 20250413232145.png]]

---

什么是 component 

The question now arises as to what exactly constitutes a software component. A common definition by Clemens Szyperski et al. [SGM02] is cited and discussed below.

> "A software component is a unit of composition with contractually specified interfaces and explicit context dependencies only. A software component can be deployed independently and is subject to composition by third parties." (Clemens Szyperski et al.)


- _Unit of Composition_ : A component is a unit within a hierarchical system architecture. The granularity of a component is not specified. A component could implement only a single method—or a complex subsystem that includes several other components.
- _Specified Interfaces_ : A component specifies its external interface. Other components or a human user can only interact with the component through this interface. The component's behavior is guaranteed according to the so-called ["design-by-contract principle](https://en.wikipedia.org/wiki/Design_by_contract) ." This includes, for example, the typing of input and return arguments, the specification of possible exceptions, and, if necessary, special preconditions and postconditions. Everything not described in the interface appears as _a black box_ to the caller .
- _Explicit Context Dependencies_ : A component depends on defined premises regarding its execution context, which must be permanently fulfilled at runtime. These dependencies can include, for example:
    - Required interfaces that are provided and implemented at runtime by other components in the system (see [Dependency Injection](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-dependency_injection) )
    - Availability of certain resources such as databases or file system paths
    - Specific requirements for a runtime environment (e.g. JVM version), an operating system or drivers for communication with peripheral devices
- _Independent Deployment_ : A base component that is not further subdivided is limited by the requirement that each component can be deployed independently of other components, which usually also implies independent executability. This requirement is achieved through a [microservice architecture](https://martinfowler.com/articles/microservices.html) based on [container virtualization](https://de.wikipedia.org/wiki/Containervirtualisierung) (in practice, especially [Docker](https://www.docker.com/) ). The term _service-oriented architecture (SOA)_ implies that an application was divided into loosely coupled components at design time, but these components typically cannot be deployed independently of one another.
- _Third-party composition_ : Components created by third parties can be reused. There is an explicit separation of roles between provider and user of a component. A provider acts as a developer who implements the functionality of a component, specifies its interface and dependencies, and publishes these in a repository from which future users can retrieve the component (possibly subject to licensing). A user, for their part, can work as an architect by combining various basic components to create a higher-value component for which they themselves act as the provider. The following figure outlines the different roles in component-based development. A person or team can, of course, take on multiple roles simultaneously.


![[301_architecture/image/Pasted image 20250413232813.png]]


## 3.1 Component models

In the context of component-based development, we will in practice choose a specific component model. A component model specifies how components and their composition are represented textually (e.g., in a programming language) or graphically (e.g., in UML notation).

>  A software component model is a definition of (1) the semantics of components, that is, what components are meant to be, (2) the syntax of components, that is, how they are defined, constructed, and represented, and (3) the composition of components, that is, how they are composed or assembled." (Kung-Kiu Lau et al.)


UML also offers a component model that, unlike previous models, is not focused on a concrete implementation, but rather on the specification, documentation, and communication of an architectural design. The following figure shows how components are represented in a UML component diagram.


![[301_architecture/image/Pasted image 20250413233847.png]]


A component can be implemented in the UML by other subcomponents or by classes. 
In the UML sense, both are so-called _classifiers_ . In the figure above, the component is implemented `Provider` by the subcomponents `PartA`and , which in turn is implemented by the classes and . Further implementations are not specified. 
The component is manifested by the executable artifact . The component diagram does not contain any information about the runtime environment in which and the host on which this artifact is deployed. This information would be shown in the view of a deployment diagram belonging to the overall model. The component provides the interface to the outside world that is used by the component. Internally, this interface is implemented in the subcomponent. A clear notational description of the UML component diagram can be found in the [documentation for Microsoft Visual Studio](https://docs.microsoft.com/visualstudio/modeling/uml-component-diagrams-reference?view=vs-2015#reading-component-diagrams) and in the UML textbooks referenced in the chapter [Object Orientation and UML .](https://moodle.oncampus.de/modules/ir843/onmod/public/index.html?uid=zhuyanb&cid=BHT-MIB-20-W24-015987#unit-0-2)`PartB``PartA``ClassA1``ClassA2``Provider``provider.jar``Provider``ProvidedInterface``Client``PartA`


## 3.2 Java Platform Module System (JPMS)

[In contrast to UML, the Java module system (JPMS)](https://www.informatik-aktuell.de/entwicklung/programmiersprachen/java-9-das-neue-modulsystem-jigsaw-tutorial.html) does not allow components to be nested hierarchically. In Java, only packages, as implementations of modules, can be hierarchically subdivided as usual. Dependencies can be explicitly defined at the module level ( `requires <module>`). The visibility of the module's internal implementation can also be explicitly enabled externally ( `export <package>`).

The following code example shows the two Java modules `client`and `simpleRegression`, the latter implementing a simple linear regression. The input for the regression model is a data series with 2D coordinates in JSON format, i.e., corresponding x and y values ​​(lines 13-14). 
he module depends `client`exclusively on the module `simpleRegression`, whose dependencies remain hidden from it (line 3). It is therefore largely decoupled. Access to the package `regression`is only possible because it is explicitly exported (line 7 in the module `simpleRegression`). The module `simpleRegression`depends on the following modules: [commons.math3](http://commons.apache.org/proper/commons-math/) for training the regression model, [gson](https://github.com/google/gson) for converting JSON to Java objects, and [java.desktop](https://docs.oracle.com/en/java/javase/11/docs/api/java.desktop/module-summary.html) due to the use of the class `Point`for 2D coordinates (lines 3-5). Furthermore, the module results in `gson`a transitive dependency on the module `java.sql`(line 6).


Client
```java
// module-info.java
module client {
    requires simpleRegression;
}

// Client.java
package client;
import regression.SimpleRegressionModel;

class Client {

    public static void main(String[] args) {
        String json = "[{'x': 2, 'y': 4}, {'x': 4, 'y': 3}, {'x': 3, 'y': 6}, {'x': 9, 'y': 7}, {'x': 7, 'y': 8}]";
        SimpleRegressionModel model = new SimpleRegressionModel(json);

        System.out.println("Regression function: y = f(x) = " + model.getSlope() + "x + " + model.getIntercept());
        System.out.println("R² = " + model.getDeterminationCoefficient());

        double x = 5;
        System.out.printf("Prediction: y = f(%.1f) = %.1f", x, model.predict(x));
    }
}
```


SimpleRegression
```java
// module-info.java
module simpleRegression {
    requires gson;
    requires commons.math3;
    requires java.desktop;
    requires transitive java.sql; // gson depends on java.sql
    exports regression; // exports package
}

// SimpleRegressionModel.java
package regression;
import com.google.gson.Gson;
import org.apache.commons.math3.stat.regression.SimpleRegression;
import java.awt.Point;

public class SimpleRegressionModel {

    Gson gson = new Gson(); // dependency to Google GSON
    SimpleRegression model = new SimpleRegression(); // dependency to Apache Commons Math

    public SimpleRegressionModel(String json) {
        Point[] points = gson.fromJson(json, Point[].class); // dependency to Java Desktop (Point class)
        for (Point p : points) model.addData(p.x, p.y);
    }

    public double getIntercept() { return model.getIntercept(); }

    public double getSlope() { return model.getSlope(); }

    public double getDeterminationCoefficient() { return model.getRSquare(); }

    public double predict(double x) { return model.predict(x); }
}
```

The JPMS code examples shown can be found in the /architecture/jpms directory of the module repository. The following UML component diagram illustrates the dependencies of the modules from the above code example. It clearly shows how JPMS helps to explicitly define and, if necessary, restrict dependencies between components.

![[301_architecture/image/Pasted image 20250413235408.png]]

## 3.3 Encapsulation at the class level

So far, we've only discussed modularization and encapsulation at the component level. In the detailed design, components are implemented using classes, which can also hide internal data and behavior structures from each other in the form of private attributes and methods. In object-oriented languages, the securability modifiers `private`and are particularly `protected`useful for this purpose. If a class is regularly accessed from outside, a stable interface is important. In this case, access can be usefully encapsulated using getter and setter methods to modify the underlying internal structures without affecting external access. The fundamental question is which external accesses should be used to encapsulate the internal structures:

- From accesses from methods in other classes in the same module?
- Or from access from other modules by external users who use their own module as a dependent library?

In the latter case, the effort required to implement a change to the interface can be very high because external users may reject any changes out of inertia and possibly abandon the system, or because they are not fully known due to their large number, etc. However, many classes are also developed that are only used internally for a small application or module and are not made available externally via an API. If the number of accesses to a class's structures is manageable and can be changed relatively easily by refactoring, even in dependent locations, it is worth considering whether getters and setters are really necessary. The structural design of the class should remain simple and not reflect the scars of the class's version history. _Keep it simple!_ 
Getters and setters always make sense if they provide additional functionality and the caller also expects this. Since Java 9, however, visibility can sometimes be more effectively restricted at the module level than at the class level.

[The Lombok](https://projectlombok.org/) project allows Java to generate getters and setters without any additional functionality, which merely represent _[boilerplate code](https://en.wikipedia.org/wiki/Boilerplate_code)_ , at compile time using annotations such as `@Getter`and . A corresponding plugin (see here for [IntelliJ IDEA](https://projectlombok.org/setup/intellij) ) must be installed in the development environment used so that the Lombok annotations can also be resolved during development.`@Setter`


# 4 ARCHITECTURAL CONCEPT


In dieserVL:
• Model-View-Controller
• Layer-based
• Repository-based
• Pipes-and-Filter
• Event-based
• Interrupt-based
• Client-Server
• Peer-to-Peer
• Service-oriented


## 4.1 MONOLITHIC

− „Oldest“ architectural concept
− No modularity and no distribution
− Self contained
	− Contained user interface and data access
− “No” external dependencies
	− Usually executed on top of a operating system
− A binary executable

## 4.2 PIPELINE / Pipes-and-Filter Architecture

− Sequential execution of processing steps
	− Order of steps needs to be defined
− Data will be handed over from one process to the next process
− Buffers between the processes
− Targets on data stream processing
	− Continuous throughput, variable latency

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200828.png]]

---

Arbeitsschritte nur durch Daten verknüpft
• fließen wie durch ein Rohr von einer Komponente zur nächsten
• typisch für Systeme, die Daten schrittweise weiterverarbeiten
• Bearbeitung sequenziell und parallel möglich

![[301_architecture/image/Pasted image 20250219155901.png]]


Vorteile
• System bildet Geschäftsprozesse direkt ab
• übersichtlich
• modular, leicht erweiterbar
• Systemzustand in den Daten gekapselt

Nachteile
• Daten müssen von jeder Komponente erneut aufbereitet werden
• Vorgegangene Schritte müssen immer vollständig abgeschlossen werden

## 4.3 Model View Controller

Model
• enthält die persistenten Daten
• Geschäftslogik innerhalb dieser Daten
• unabhängig von anderen Einheiten

View (Präsentation)
• Darstellung der Daten
• Entgegennahme von Benutzerinteraktion
• kennt Modell und Control
• verschiedene Präsentationen (Views) möglich

Controller (Steuerung)
• verwaltet die Präsentation
• führt Benutzeranfragen aus und gibt sie ggf. an das Modell weiter


![[301_architecture/image/Pasted image 20250219160344.png]]

Ermöglicht verschiedene Interaktionen mit dem System
• Typisch in Systemen mit Fokus auf Benutzerschnittstellen
• Erleichtert spätere Erweiterungen, z.B. neue Präsentationsarten (View)
• Ermöglicht Austausch getrennter Komponenten, z.B. der Datenbank

MVC ähnelt dem ECB-Pattern: Aufteilung in boundary, entity & control

![[301_architecture/image/Pasted image 20250219160407.png]]


## 4.4 Layer-based Architecture

Aufteilung in mehrere Abstraktionsschichten
• Trennung z.B. von technischen Details und Inhalten
• Schichten bieten darüber liegenden Schichten Dienste an
• Elementare Dienste in unterster Schicht



![[301_architecture/image/Pasted image 20250219160002.png]]


TCP/IP-Referenzmodell (Wikipedia)
verschiedene Schichten und Protokolle der Internet-Kommunikation
Klar festgeschriebene Schnittstellen!

![[301_architecture/image/Pasted image 20250219160016.png]]

Vorteile
- Abstraktion von Details der einzelnen Schichten. 
	- Separation of Concerns, Information Hiding
- Flexibler Austausch von Schichten möglich
	- Wartbarkeit, Erweiterbarkeit

Nachteile
- Trennung kann Performance-Nachteile bringen
	- Spezialfälle mit effizienteren Lösungen müssen generalisiert werden
- Anfragen/Antworten müssen über mehrere Schichten weitergeleitet werden


## 4.5 EVENT-DRIVEN

− Communication between components via „events“
	− Events do not have a dedicated receiver
− Components register at event bus as receiver or emitter
− One single event can be received by multiple components
− Event Bus can implement specific event handling paradigms
− Loose coupling between components
− Can be used for user interface


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200847.png]]


---

Komponenten sind unabhängig von einander
• warten auf Ereignisse (Events) zum Start von Aktivitäten (consumer)
• oder lösen Ereignisse aus (producer)
• zentrale Komponente (event channel) verteilt die Ereignisse

![[301_architecture/image/Pasted image 20250219160503.png]]

Vorteile
• Reaktion auf Ereignisse kann unmittelbar erfolgen
• geeignet für asynchrone/chaotische Umgebungen (z.B. Benutzer-Interaktion)

Nachteile
• Nicht behandelte Events müssen u.U. zwischengespeichert werden
• erhöhte Anforderungen an Synchronisation
• Verhalten schwerer vorhersagbar

## 4.6 Interrupt-based Architecture

Spezialfall von event-based architectures
• Verwendet Interrupts als Hardware(„low-level“)-Events
• Interrupts können maskiert (ignoriert) werden

![[301_architecture/image/Pasted image 20250219160600.png]]


## 4.7 Repository-based Architecture

Organisation des Systems um einen zentralen Datenspeicher
• Komponenten sind über gemeinsame Daten (z.B. Datenbank) verbunden
• Koordination von Komponenten im Repository (z.B.: Trigger, Locks)

![[301_architecture/image/Pasted image 20250219160802.png]]

Vorteile
• wenig Schnittstellen
• konsistente, zentrale Datenhaltung
• Einfache Erweiterbarkeit durch Hinzufügen neuer Komponenten

Nachteile
• Kommunikation zwischen Komponenten über die Daten teilweise ineffizient
- Die Anbindung ans Repository wird zum Bottleneck
• Repository ist „single point of failure“
- Ausfall der zentralen Komponente bedeutet Totalausfall

## 4.8 CLIENT-SERVER


10
− A.k.a Remote Procedure Calls (RPC)
− Client
	− Initiates a connection
	− Requires a service
− Server
	− Receives services requests
	− Provides the service
	− Sends a reply
− A server can serve multiple clients
	− Robust
	− May in particular improve performance
	− Server can be a bottleneck

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408200916.png]]



---


Architekturstil für verteilte Systeme
• jede Systemfunktion wird als Dienst auf einem zentralen Server angeboten
• Clients können diese Funktion in Anspruch nehmen

![[301_architecture/image/Pasted image 20250219160844.png]]

Keine direkte Kommunikation zwischen Clients
• Interaktion zwischen Clients nur über Server möglich

![[301_architecture/image/Pasted image 20250219160942.png]]


Vorteile
• Funktionen stehen zentral zur Verfügung, müssen nicht mehrfach implementiert werden
• einfache Verwaltung gemeinsam genutzter Ressourcen
• Leistungsschwache Clients können Arbeitslast auf den Sever auslagern

Nachteile
• Single point of failure
• zusätzliche Kommunikation
• ungleiche Lastverteilung / schlechte Ressourcennutzung
• Zentrale Daten (Privacy)




## 4.9 PEER-TO-PEER

− Decentralized: No dedicated server
	− Peers provide services
	− Peers use services
	− Robustness via redundancy
− Supports flexibility but harder to control
	− Prediction
	− Scheduling
	− Load distribution

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201015.png]]


---


Komponenten in verteilten Systemen sind gleichberechtigt
• jede kann Funktionen bereitstellen und nutzen
• meist macht ein zentraler Server die Teilnehmer bekannt

Skype (in den Anfängen)
• Jeder Client meldet sich beim Server nur zur Authentifizierung und zum
Abrufen des Status der Kontakte


![[301_architecture/image/Pasted image 20250219161019.png]]

Vorteile
• effiziente Kommunikation (keine Umwege)
• gleichmäßige Last/Ressourcenverteilung
• Ausfallsicherheit

Nachteile
• Gemeinsam genutzte Ressourcen schwierig zu synchronisieren
• Funktionen mehrfach implementiert (Komponenten komplexer)
• Datenverkehr nur zwischen einzelnen Teilnehmern

Verteilung kann auch dezentral geschehen (cf. Distributed Hash Tables)



## 4.10 BLACKBOARD


− Metaphor: Experts in front of a black board
	− Problem is written on the blackboard
	− A single instance can not solve the problem
	− But a single instance can solve a part of the problem
− Blackboard used as centralised data structure
	− Contains problem and partial solutions
	− Access control/coordination needed
	− Changes are propagated to the experts
− This is used in the context of artificial intelligence systems


## 4.11 MOBILE AGENTS


− Mobile agents are processes
	− Data (state)
	− Behaviour (executable code)
− Move (migrate) from one computer to another
	− Mobile agent selects target
	− Collects and processes data
− Try to solve a problem
− Inverse blackboard approach
− Needs a specific agent platform
	− Multi Agent System


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201136.png]]

## 4.12 3-TIER

− A tier may use the functionality of a neighbour tier
− Separation of general responsibility / functionality
	− Easier to distribute
− Typical incarnation: 3 tier
	− Presentation tier
	− Application tier
	− Database tier
− Very common for web based enterprise systems


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201206.png]]

## 4.13 Service Oriented Architecture

System besteht aus verteilten, unabhängigen Komponenten
• Komponenten bieten Funktionalitäten als Services an
• Globale Registry für Service-Anbieter und Services
• Komplexe Komponenten können wieder andere Services verwenden
• Standard-Protokoll (z.B. SOAP, REST) für alle Services


Vorteile
• Services/Funktionalitäten austauschbar
• Erweiterung durch simple Registrierung neuer Services
• erlaubt loose coupling (dynamische Verbindung im Betrieb)
• einheitliche/standardisierte Protokolle für Schnittstellen

Nachteile
• komplexe technische Infrastruktur
• Registry ist „single point of failure“


![[301_architecture/image/Pasted image 20250219161113.png]]


![[301_architecture/image/Pasted image 20250219161123.png]]




# 5 MICRO SERVICES


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201239.png]]



MICRO SERVICES
Source: https://microservices.io/
− Single self contained services
− Smaller code base
	− Good separation of concerns
	− Focus on scalability
	− Quick and easy to deploy
	− Isolation of faults
− But:
	− Complexity can’t be reduced
	− Complex communication
	− Unit testing is easier integration testing might not


# 6 HEXAGONAL ARCHITECTURE

− Used as natural choice in Domain-Driven Design approach
− Separates
	− Domain model
	− Application
	− External systems (active or passive)
− Uses ports and adapters

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408201406.png]]


# 7 方式

## 7.1 Softwarearchitektur in der plangesteuerten Softwareentwicklung

Vorgehensweise
• Anforderungen ( 橙色圆圈 ) stehen zu Beginn der Implementierungsphase fest
• Grundlegende Architektur ( 蓝色方框 ,  蓝色圆圈 ) kann nach Anforderungsspezifikationentworfen werden
• Top-Down-Implementierung möglich: abstrakte Implementierung der Architektur (Grundgerüst), gefolgt von Details und Features

![[301_architecture/image/Pasted image 20250219161430.png]]

Vorteile
• Berücksichtigung späterer Anforderungen, wodurch eine strukturierte Entwicklung der Architektur möglich wird
• Parallele Implementierung von Anforderungen innerhalb der Grundstruktur
• Identifikation von Fehlern in den Anforderungen

Nachteile
• Fehler in der Architektur aufgrund hoher Komplexität
• Änderungen der Architektur aufgrund von neuen Anforderungen ist teuer
• Geringe Akzeptanz der Architektur durch Entwickler
• Technische Probleme werden erst nach dem Architekturentwurf ersichtlich

## 7.2 Softwarearchitektur in der agilen Softwareentwicklung 

Vorgehensweise
• Stetige Änderungen der Anforderungen ( 橙色圆圈 )
• Grundlegende Architektur ( 蓝色方框 ,  蓝色圆圈 ) nicht planbar, sondern Ergebnis permanenter Veränderung
• Iterative Weiterentwicklung der Architektur anhand der nächsten Anforderungen
• Prinzip Einfachheit: Aktuelle Implementierung sollte simple sein und somit spätere Änderungen erlauben

![[301_architecture/image/Pasted image 20250219161912.png]]

Vorteile
• Demokratischer Prozess: Architektur orientiert an technischer Umsetzung
• Schnelle, lauffähige Iterationen mit integrierter Implementierung
• Aktualität und zeitnahes Feedback zur Architektur

Probleme
• Mehraufwand durch häufiges Umbauen der Architektur bzw.vollständige Neuimplementierung notwendig
• Ständige Anpassungen bereits fertiger Implementierung(kann auch positiv sein – Refactoring)

Achtung: Die vorgestellten Architekturstile sind in der plangesteuerten und agilen Softwareentwicklung identisch!


