
- Structure diagrams to describe structural aspects of the systems (Classes, Components, Properties etc.)
- The following diagrams types are briefly introduced
	- Package diagram
	- Class diagram
	- Object diagram
	- Composite structure diagram
	- Component diagram

- Paketdiagramme (Package Diagrams)
	- Partitionierung des Modells in Pakete
	- Aggregations- und Gen/Spec-Beziehungen zwischen Paketen
	- Importbeziehungen:
		- Import einzelner Elemente mit `<<access>>`
		- Import ganzer Pakete mit `<<import>>`
	- Darstellung der hierarchischen Struktur des Systems
- Komponentendiagramme (Component Diagrams)
	- Komponenten: ausführbare Klassen, kapseln internen Aufbau, stellen Verhalten über Schnittstellen und Ports zur Verfügung
	- Komponentendiagramme stellen die Komponenten und deren Interaktion dar (Ports und Schnittstellen)
	- Darstellung der funktionalen Struktur (Software Architecture)
- Kompositionsstrukturdiagramme (Composite Structure)
	- Konfiguration von miteinander verbundenen Laufzeitelementen
	- z.B. Zusammenarbeit von Klassen oder Objekten zur Erfüllung einer bestimmten Aufgabe
- Verteilungsdiagramme (Deployment Diagrams)
	- Beschreibung der physikalischen Struktur (Topologie) von verteilten Systemen
	- Modellierung aller im realen System tatsächlich vorhandenen Hardware- und Software-Knoten und deren Verbindungen (Kommunikationspfade)
- Physikalische Struktur setzt sich zusammen aus
	- Artefakten (physikalische Informationseinheiten, z.B. Dateien)
	- HW- und SW-Knoten (Ausführungseinheiten, z.B. Geräte)
	- Kommunikationspfaden (physikalische Verbindungen)



# 1 PACKAGE DIAGRAM

![[01_UML/image/Pasted image 20250104174048.png]]

Packages diagram shows the structure of the system at Package level and interdependencies among those Packages.

Main constituents are:
- Package
- PackageableElements
- PackageImport
- ElementImport

![[01_UML/image/Pasted image 20250104174214.png]]

- A Package represents namespaces and therefore, influences the visibility of members of that namespace
- PackageableElements are elements that can be contained by a Package (e.g., Class/Component, InstanceSpecification, Depdendency etc.)
- ==Visibilitiy kinds are private (-), protected (#), public (+), packagewide==
- Packages can be nested

1 nested Packages 
Alternative Representations of Packages with nested Packages 
![[01_UML/image/Pasted image 20250104174507.png]]

2 PackageImport
PackageImport enables importing Packages to get access to all public PackageableElements of another Package
==(private with «access», public with «import»)==
![[01_UML/image/Pasted image 20250104174829.png]]


3 ElementImport
ElementImport enables Packages to import single public PackageableElements from another Package
![[01_UML/image/Pasted image 20250104174954.png]]


4 ElementImport with alias 
Elements imported via ElementImport can have an alias assigned to avoid nameclashes in the importing Package
![[01_UML/image/Pasted image 20250104175001.png]]


# 2 CLASS DIAGRAM

Class diagrams enable a view on structural information of the system at Class level

Klassendiagramm
- Zeigt die Objektklassen im System und deren mögliche Beziehungen
- Beschreibt alle möglichen Zustände des Systems
- Kann für Entwurf und Dokumentation verwendet werden
- Statisch (Compile-Zeit)

Klassendiagramme erlauben die Modellierung abstrakter objektorientierter Konzepte, die unabhängig von der tatsächlichen Implementierung sind
- Objektorientierung bekannt von objektorientierten Programmiersprachen wie Java
- Bspw. wird tatsächliche Implementierung von Methoden oder Vererbung nicht vorgegeben

![[01_UML/image/Pasted image 20250104175705.png]]

![[01_UML/image/Pasted image 20250104175845.png]]



Main constituents (among others) are
− Classes, DataTypes, Signals or Interfaces
− Generalizations
− InterfaceRealizations
− Attributes (Properties)
− Operations, and
− Associations

Wesentliche Modellierungselemente
- Klassen
- Attribute
- Assoziationen
- Stereotype

## 2.1 Classifier

- Classifier is the common super metaclass for all elements that may posses a structure.
- Abstract specializations (among others): StructuredClassifier, BehavioredClassifier, EncapsulatedClassifier
- Concrete specializations (among others): Class, Component, Interface, Signal, DataType, PrimitiveType, Association


## 2.2 Classes


Bilden ein abstraktes Modell für eine Menge von ähnlichen Objekten
- Erlauben Instanziierung mehrerer Objekte desselben Typs

A Class is an structured, complex type that is able to own Attributes (Properties), Operations and nested Classes
Class may have an internal structure
Other kinds of Classes are DataType, Signal and Interface
Formal derivation rule may be described with OCLD

![[01_UML/image/Pasted image 20250115141020.png]]

![[01_UML/image/Pasted image 20250104180604.png]]



## 2.3 Property/Attribute

- Property is kind of MultiplicityElement
- Lower and upper bound determine multiplicity
- Fundamental collection types defined by metaattributes isOrdered and isUnique

Klassen können getypte Attribute enthalten
Attribute können mit einer Sichtbarkeit versehen werden
- public (+): jede andere Klasse kann auf eigenes Attribut zugreifen
- private (-): Zugriff nur von eigener Klasse
- protected (#): Zugriff nur von eigener Klasse und Unterklassen

![[01_UML/image/Pasted image 20250115211738.png]]

![[01_UML/image/Pasted image 20250104181219.png]]

## 2.4 Generalisierung/Spezialisierung


- Ermöglicht die Modellierung von Vererbung in Klassendiagrammen
	- Eigenschaften (Attribute/Assoziationen) der generellen Klasse (Oberklasse) werden an spezielle Klassen (Subklassen) vererbt
	- Abstrakte Klassen (Oberklasse kann nicht instanziiert werden) oder Operationen (müssen durch Subklassen implementiert werden) werden kursiv dargestellt

In UML ist Mehrfachvererbung möglich: In Java z.B. durch Interfaces umsetzbar
![[01_UML/image/Pasted image 20250115215337.png]]

![[01_UML/image/Pasted image 20250115215442.png]]

- All Classifier may participate in Generalizations
- A Generalization specifies that a Class (or more generally, a Classifier) is a specialization of another, more general Classifier
	- Similar to inheritance in programing languages
- An InterfaceRealization specifies that a Class/Component realizes one or more certain Interfaces
	- Similar to interface implementation in programming languages (e.g. Java)

![[01_UML/image/Pasted image 20250104181516.png]]

## 2.5 ASSOCIATION

Stellen die Beziehungen zwischen Klassen dar
- Assoziationsbezeichnung hat eine Leserichtung (optional, aber hilfreich)
- Assoziationen können als Objektreferenzen bzw. durch verschiedene Container implementiert werden

![[01_UML/image/Pasted image 20250115212103.png]]


- Associations specify semantic relationships (called links) that can occur between typed instances.
- It has at least two association ends (binary association) but could be n-ary
- Association ends have a specific multiplicity, can be ordered, unique, supersets, subsets, derived, unions … because they are Properties
- ==`*`的意义是 Multiplicity== 就是 notation 的值 


![[01_UML/image/Pasted image 20250104191134.png]]


### 2.5.1 Rollen

Verschiedene Rollen (Namen am Assoziationsende) helfen mehrdeutige Assoziationen zu unterscheiden und Assoziationen passend zu benennen

Der Onlineshop führt normale und Premiumkunden und -kundinnen auf zwei verschiedenen Listen
![[01_UML/image/Pasted image 20250115213056.png]]


### 2.5.2 Navigierbarkeit

Gibt an, in welche Richtung die Assoziation (nicht) gelten soll
	→: In diese Richtung existiert die Assoziation (als Attribut)
	X: In diese Richtung darf sie nicht gelten (Es gibt kein Attribut)

Wenn keine Navigierbarkeit angegeben ist, ist sie unspezifiziert

![[01_UML/image/Pasted image 20250115213327.png]]





### 2.5.3 Multiplizitäten

![[01_UML/image/Pasted image 20250115213515.png]]

![[01_UML/image/Pasted image 20250115213533.png]]

- 1 cart contains 0 oder 1 customer ,  1 customer hat nur 1 cart 

### 2.5.4 class 的 attribute 作为 ausschnitt 用 association 相连

Je nach Zweck des Modells kann ein spezifischer Ausschnitt des Systems dargestellt werden
Die Auswahl der Klassen bestimmt welche Attribute als Assoziation dargestellt werden!

图片中的例子 
- customer 有个 attribute "name", 这个name 为 klasse string 的一个 object 

![[01_UML/image/Pasted image 20250115214241.png]]


### 2.5.5 Implementierungsdetails

Beispielhafte Implementierung der Navigierbarkeiten in JAVA
![[01_UML/image/Pasted image 20250115214611.png]]


Beispielhafte Implementierung verschiedener Multiplizitäten in JAVA
![[01_UML/image/Pasted image 20250115214625.png]]




### 2.5.6 aggregation and Composition

- Each Association end has a particular aggregation kind associated
	- None (Association)
	- Shared (Aggregation)
	- Composite (Composition)
- Determine lifecycle dependencies among instances of Classifiers that are associated
- ==None and Shared means instances are rather loosely coupled==
- Composite describes a whole-part coupling (lifecycle dependency of the part)
- ==注意实心和虚心三角的意义==, 虚心菱形代表 loosely coupled, 实心的route代表 a whole-part coupling

In UML enthalten Beziehungen als Teil-Ganzes hervorzuheben
- Aggregation 
	- ![[01_UML/image/Pasted image 20250115215116.png]]
	- Objekte der Klasse A sind aus anderen Objekten der Klasse B zusammengesetzt, bzw. die Objekte von B gehören zu A
	- Multiplizitäten werden nicht eingeschränkt
		- Beispiel: Eine Vorlesung besteht aus Studierenden, diese können aber mehrere VLs besuchen
- Komposition ( ♢ )
	- Spezialfall der Aggregation: Die Existenz der beherbergten Klasse hängt von der Existenz der beherbergenden Klasse ab
	- Kann Multiplizitäten 0..1 oder 1 bedeuten:
		- Beispiel 0..1: Ein Blatt gehört zu höchstens einem Baum, kann aber auch ohne existieren
		- Beispiel 1: Haus hat Räume (Haus stürzt ein → Räume sind weg)

下面的例子1
- customer 没了,  delivceradress 也没有了 
- 1 cart contains 0 oder 1 customer ,  1 customer hat nur 1 cart 

![[01_UML/image/Pasted image 20250115215514.png]]


![[01_UML/image/Pasted image 20250104191439.png]]

---

- Associations ends are Properties of either the Class at the other end of the Association or, in case of non-navigable association ends, of the Association itself
- Navigability is always possible – also for nonnavigable Association ends
- ==Dot-Notation== was introduced to determine Classifier- or Association ownership of Association ends
	- 实心点代表 这个 association end 属于  另外一边的 association end, 也属于 Association itself

![[01_UML/image/Pasted image 20250104191743.png]]




#### 2.5.6.1 Shared aggregation

https://www.uml-diagrams.org/aggregation.html

**Shared aggregation** (**aggregation**) is a [binary association](https://www.uml-diagrams.org/association.html#binary-association) between a [property](https://www.uml-diagrams.org/property.html) and one or more composite objects which group together a set of instances. I==t is a "weak" form of [aggregation](https://www.uml-diagrams.org/association.html#aggregation) when part instance is independent of the composite==. Shared aggregation has the following characteristics:

- it is [binary association](https://www.uml-diagrams.org/association.html#binary-association),
- it is asymmetric - only one end of association can be an aggregation,
- it is transitive - aggregation links should form a directed, acyclic graph, so that no composite instance could be indirect part of itself,
- shared part could be included in several composites, and if some or all of the composites are deleted, shared part may still exist.

---

Shared aggregation is depicted as association decorated with a _hollow diamond_ at the aggregate end of the association line. The diamond should be noticeably smaller than the diamond notation for N-ary associations.

Example below shows _Triangle_ as an aggregate of exactly three line segments (sides). Multiplicity '*' of the _Triangle_ association end means that each line _Segment_ could be a part of several triangles, or might not belong to any triangle at all. Erasing specific _Triangle_ instance does not mean that all or any segments will be deleted as well. (Note, that we named collection of three line Segments as 'sides', while usual UML convention is to use singular form, i.e. 'side', even for collections.)

![[01_UML/image/Pasted image 20250104194133.png]]

Shared aggregation could be depicted together with other association adornments such as [navigability](https://www.uml-diagrams.org/association.html#navigability) and [association end ownership](https://www.uml-diagrams.org/association.html#association-end). In the example below line _Segment_ is navigable from _Triangle_. Association end 'sides' is owned by _Triangle_ (not by association itself), which means that 'sides' is an [attribute](https://www.uml-diagrams.org/property.html#classifier-attribute) of _Triangle_.

![[01_UML/image/Pasted image 20250104194208.png]]

#### 2.5.6.2 Composition

https://www.uml-diagrams.org/composition.html

**Composite aggregation** (**composition**) is a "strong" form of [aggregation](https://www.uml-diagrams.org/association.html#aggregation) with the following characteristics:
- it is [binary association](https://www.uml-diagrams.org/association.html#binary-association),
- it is a _whole/part_ relationship,
- a part could be included in _at most one_ composite (whole) at a time, and
- if a composite (whole) is deleted, all of its composite parts are "normally" deleted with it.

Note, that UML does not define how, when and specific order in which parts of the composite are created. Also, in some cases a part can be removed from a composite before the composite is deleted, and so is not necessarily deleted as part of the composite.

---

Composite aggregation is depicted as a binary association decorated with a **filled black diamond** at the aggregate (whole) end.
![Composite aggregation is depicted as binary associations with filled black diamond](https://www.uml-diagrams.org/association/class-composition.png "Composite aggregation is depicted as binary associations with filled black diamond")

Folder could contain many files, while each File has exactly one Folder parent.  
If Folder is deleted, all contained Files are deleted as well.

---

When composition is used in **domain models**, both whole/part relationship as well as event of composite "deletion" should be interpreted figuratively, not necessarily as physical containment and/or termination. UML specification needs to be updated to explicitly allow this interpretation.
![Composite aggregation could be used in domain models.](https://www.uml-diagrams.org/association/class-composition-domain.png "Composite aggregation could be used in domain models.")

Hospital has 1 or more Departments, and  
each Department belongs to exactly one Hospital.  
If Hospital is closed, so are all of its Departments.

---

Note, that though it seems odd, multiplicity of the composite (whole) could be specified as **0..1** ("at most one") which means that part is allowed to be a "stand alone", not owned by any specific composite.

![Multiplicity of composite (whole) could be 0..1 (at most one).](https://www.uml-diagrams.org/association/class-composition-optional.png "Multiplicity of composite (whole) could be 0..1 (at most one).")

Each Department has some Staff, and each Staff could be  
a member of one Department (or none). If Department is closed,  
its Staff is relieved (but excluding the "stand alone" Staff).


## 2.6 Stereotype

Stereotype trennen Klassen nach ihrer Bedeutung im System 

Entity-Control-Boundary-Pattern (ECB)
- Verwendet drei Stereotype zur Trennung von Daten und Funktionalität
	- Entity:  Klasse repräsentiert Daten im System (lokale Funktionalität erlaubt)
	- Boundary:  Schnittstelle zu Akteuren/Akteurinnen 活跃分子 außerhalb des Systems.
	- Controller:  Übergeordnete Funktionalität, z.B. die Use-Cases

Controller-Klassen halten keine persistenten Daten, Entity-Klassen haben keine klassenübergreifenden Operationen
- Vereinfachte Form des Architekturmodells Model-View-Controller (MVC)

Im Allgemeinen existiert im System nur eine Instanz jedes Controllers

![[01_UML/image/Pasted image 20250115223256.png]]


Unter Umständen müssen einzelne Klassen zerlegt werden, um eindeutige Stereotype vergeben zu können
![[01_UML/image/Pasted image 20250115223340.png]]






## 2.7 Fallbeispiel (class-attribute-association)


1 先得到 class
Sie werden gebeten für einen kleines Unternehmen, das Schuhe und Kleidung verkauft, die Verwaltungssoftware eines Online-Shops zu entwickeln. Der Onlineshop soll es Kunden und Kundinnen ermöglichen, Produkte in einen Warenkorb zu legen und diesen zu bezahlen. Jeder Kunde/jede Kundin hat mindestens eine Lieferadresse.

Wir kennen bereits die Use-Cases:
Was sind sinnvolle Klassen dieses Systems?
![[01_UML/image/Pasted image 20250115205601.png]]

![[01_UML/image/Pasted image 20250115205745.png]]



---

2  算出 attribute/property 

Im System wird der Name und eine Email-Adresse von jedem Kunden/jeder Kundin des Onlineshops hinterlegt. Außerdem wird für jedes Produkt eine Produktnummer und eine Bezeichnung sowie der Preis und die Lagermenge gespeichert.

![[01_UML/image/Pasted image 20250115211935.png]]


---

3 Assoziationen

![[01_UML/image/Pasted image 20250115212430.png]]

# 3 Object Diagram 

同一个 klasse 的不同 object (object 有自己的 zustand (attribute 的值 ), 和其他的 object 不同)

Objekt-Diagramm ist eine Instanz des im Klassendiagramm beschriebenen Systems, d.h. genau ein Zustand
- Sinnvoll für Debugging, veranschaulichende Beispiele oder z.B. Startzustände

Objekte ähneln in der Darstellung Klassen:
![[01_UML/image/Pasted image 20250115223743.png]]

Verbindungen im Objektdiagramm entsprechen instanziierten Assoziationen im Klassendiagramm

![[01_UML/image/Pasted image 20250115224051.png]]


--- 


Object diagrams are used to visualize partial/complete instances (called InstanceSpecifications) of Classifier and Associations

Objektdiagramm
- Zeigt tatsächlich existierende Objekte im System und deren tatsächlichen Beziehungen
- Beschreibt genau einen Zustand des Systems
- Kann für Dokumentation und Debugging verwendet werden, nicht für Entwurf
- Dynamisch (Laufzeit)

![[01_UML/image/Pasted image 20250104194747.png]]




Main constituents are
− InstanceSpecifications
− Slots
− Links (InstanceSpecification of Associations)

![[01_UML/image/Pasted image 20250104194850.png]]

## 3.1 InstanceSpecifications

InstanceSpecifications specify an overview of a set of instance of a Classifier at a certain point in time
- An InstanceSpecification is not an instance, but a set of matching extensional instances
- Very often used in model-based testing approaches to reflect test data

InstanceSpecification of Associations are called links

![[01_UML/image/Pasted image 20250104195045.png]]


## 3.2 Object diagrams

Object diagrams can be used to visualize the metaclass instances of a UML (abstract syntax)

![[01_UML/image/Pasted image 20250104195620.png]]



![[01_UML/image/Pasted image 20250104195628.png]]


# 4 COMPOSITE STRUCTURE DIAGRAM

Composite structure diagrams visualize the internal structure of a Component or Class.

Main constituents are
− Parts and Roles (concepts)
− Connectors
− Ports (see Component Diagram)


![[01_UML/image/Pasted image 20250104200021.png]]

## 4.1 Role and Part 

- The term ==Role== describes an owned Property of Class that can be connected with each other
- Instances of the same Class can assume different roles in the context Classifier
- The term ==Part== refers to Properties with Aggregation kind composite
- Multiplicities on Roles/Parts determine the amount of instances that can be instantiated in the context Classifier

Role:  Wheel, Enginer 
Part:  Car 

![[01_UML/image/Pasted image 20250104201256.png]]

## 4.2 Connectors

Connectors represent links between roles/parts within the internal structure of a context Classifier

Similar to Associations
− Connectors only between roles/parts
− Associations between any suitabletyped instances

Connectors can be typed by an Association

![[01_UML/image/Pasted image 20250104201651.png]]

## 4.3 例子 

![[01_UML/image/Pasted image 20250104201851.png]]

Possible instances:
− Hybrid engine, front-wheel drive
− Combustion engine, front-wheel drive
− Combustion engine, all-wheel drive
− Electric engine, all-wheel drive

Issue: Number of engines must not be 0!
− Not possible with UML
− Constraint language required

# 5 COMPONENT DIAGRAMM

![[01_UML/image/Pasted image 20250104202007.png]]

Main building blocks of a component diagram are
− Component
− Interfaces

![[01_UML/image/Pasted image 20250104202024.png]]

## 5.1 Component

A Component is a modular, encapsulated (self-contained) part of a system
Offers interfaces to its environment (provided interfaces)
Expects interfaces from its environment (required interfaces)

A Component is substitutable
− Replaceable (design time/run-time) by compatible components
− Compatibility is calculated based on provided/required interfaces

![[01_UML/image/Pasted image 20250104202650.png]]

## 5.2 Interface 

Interfaces are displayed in ball/socket (lollipop) notation
- Provided Interfaces (via InterfaceRealization) shown as a ball with a label
	- 供别人使用的 Interfaces 
- Required Interfaces (via Usage) shown as socket with a label
	- 这个 Component 需要 input , 要让别人给入自己信息 

Offers interfaces to its environment (provided interfaces)
Expects interfaces from its environment (required interfaces)

![[01_UML/image/Pasted image 20250104202904.png]]

## 5.3 Ports 

Ports define interaction points through which encapsulated Classifiers (Components/Classes) can communicate with their environment
Port types (Class/Component) determine the set of required/provided Interfaces to connect the Port with other
(compatible) Ports
Ports can be conjugated, i.e., implicitly mirroring the provided/required Interfaces of a Port type → reduces modelling overhead

![[01_UML/image/Pasted image 20250104205023.png]]

− Compatible parts and Ports can be connected via Connectors
− Compatibility of Ports is defined compatibility of its Port types

![[01_UML/image/Pasted image 20250104205043.png]]

## 5.4 Connectors

- Connectors come in two flavours
	- Delegation connector
	- Assembly connector
- Delegation connectors links Ports with roles of the enclosing Encapsulating classifier roles with each other
- Requests can be routed through the internal structure of a StructuredClassifier (Class, Component)

# 6 PROFILE DIAGRAM

![[01_UML/image/Pasted image 20250104205645.png]]

**Profile diagram** is [structure diagram](https://www.uml-diagrams.org/uml-25-diagrams.html#structure-diagram) which describes **lightweight extension mechanism** to the UML by defining custom [stereotypes](https://www.uml-diagrams.org/stereotype.html), [tagged values](https://www.uml-diagrams.org/stereotype.html#tagged-value), and constraints. Profiles allow adaptation of the UML metamodel for different:

- _platforms_, such as Java Platform, Enterprise Edition (Java EE) or Microsoft .NET Framework, or
- _domains_, such business process modeling, service-oriented architecture, medical applications, etc.

For example, semantics of standard UML metamodel elements could be specialized in a profile. In a model with the profile "Java model," generalization of classes should be able to be restricted to single inheritance without having to explicitly assign a stereotype «Java class» to each and every class instance.

The profiles mechanism is not a first-class extension mechanism. It does not allow to modify existing metamodels or to create a new metamodel as MOF does. ==Profile only allows adaptation or customization of an existing metamodel with constructs that are specific to a particular domain, platform, or method==. It is not possible to take away any of the constraints that apply to a metamodel, but it is possible to **add new constraints** that are specific to the profile.

Metamodel customizations are defined in a profile, which is then applied to a package. [Stereotypes](https://www.uml-diagrams.org/stereotype.html) are specific metaclasses, [tagged values](https://www.uml-diagrams.org/stereotype.html#tagged-value) are standard metaattributes, and **profiles** are specific kinds of packages.

Profiles can be dynamically applied to or retracted from a model. They can also be dynamically combined so that several profiles will be applied at the same time on the same model.

Graphical nodes and edges used on profile diagrams are: [profile](https://www.uml-diagrams.org/profile.html), [metaclass](https://www.uml-diagrams.org/profile-metaclass.html), [stereotype](https://www.uml-diagrams.org/stereotype.html), [extension](https://www.uml-diagrams.org/profile-extension.html), [reference](https://www.uml-diagrams.org/profile-reference.html), [profile application](https://www.uml-diagrams.org/profile-application.html).

---


Profiling enables the creation of DSLs on top UML (by extending Elements)
− test concepts (UML Testing Profile)
− Real-time and embedded system concets (MARTE)

Every UML-compliant modeling environment is able to understand the extension

Main building blocks are
− Profile
− Stereotype
− Extension


![[01_UML/image/Pasted image 20250104205906.png]]


## 6.1 Profile

- Profile is a sub-class of Package
- Contains only Stereotypes, Classes and Extensions
- Has a unique URI to identify the Profile unambiguously
- ==Profile is applied to a Package to make the extensions accessible to all contained elements in the Package==
- Profile cannot be applied to another Profile, but Profiles can extend each other
- Profiles do not modify the UML metamodel

![[01_UML/image/Pasted image 20250105155008.png]]


## 6.2 Stereotype

- Stereotype is an extension of potentially any UML Element
- If applied to an Element, the Element turns semantically into the concept of the Stereotype
- Stereotypes may introduce additional Properties (historically: tag definitions)
- Stereotypes are instantiated when applied on an instance of the metaclass its extends
- Shapes of standard diagram elements notations can also be replaced by Stereotypes

![[01_UML/image/Pasted image 20250105155428.png]]

## 6.3 Extensions

− Extensions is kind of an Associtation among a metaclass and a Stereotype
− Extension link instances of Stereotypes to instances of the meta-class
− Extended element does not know that it was extended
− Multiple extension possible
− Application of a Stereotype only to exactly one! instance of the meta-class at any point in time

![[01_UML/image/Pasted image 20250105155459.png]]


## 6.4 UML Profiles adoption

- OMG has adopted quite a number of UML Profiles
	- For systems engineering (SysML)
	- For testing (UTP)
	- For GUI information flows (IFML)
	- For SOA/real-time embedded systems (SoaML, MARTE)
	- … (to be continued)
- An even higher number of proprietary UML Profiles exist
	- Reuse modelling environment and diagrams
	- Smoothly integrates with UML models




