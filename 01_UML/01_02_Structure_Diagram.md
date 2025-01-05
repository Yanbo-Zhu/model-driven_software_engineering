
- Structure diagrams to describe structural aspects of the systems (Classes, Components, Properties etc.)
- The following diagrams types are briefly introduced
	- Package diagram
	- Class diagram
	- Object diagram
	- Composite structure diagram
	- Component diagram

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

![[01_UML/image/Pasted image 20250104175705.png]]

![[01_UML/image/Pasted image 20250104175845.png]]


Class diagrams enable a view on structural information of the system at Class level

Main constituents (among others) are
− Classes, DataTypes, Signals or Interfaces
− Generalizations
− InterfaceRealizations
− Attributes (Properties)
− Operations, and
− Associations

## 2.1 Classifier

- Classifier is the common super metaclass for all elements that may posses a structure.
- Abstract specializations (among others): StructuredClassifier, BehavioredClassifier, EncapsulatedClassifier
- Concrete specializations (among others): Class, Component, Interface, Signal, DataType, PrimitiveType, Association


## 2.2 基础 

A Class is an structured, complex type that is able to own Attributes (Properties), Operations and nested Classes
Class may have an internal structure
Other kinds of Classes are DataType, Signal and Interface
Formal derivation rule may be described with OCLD

![[01_UML/image/Pasted image 20250104180604.png]]

## 2.3 Property

- Property is kind of MultiplicityElement
- Lower and upper bound determine multiplicity
- Fundamental collection types defined by metaattributes isOrdered and isUnique

![[01_UML/image/Pasted image 20250104181219.png]]

## 2.4 GENERALIZATION

- All Classifier may participate in Generalizations
- A Generalization specifies that a Class (or more generally, a Classifier) is a specialization of another, more general Classifier
	- Similar to inheritance in programing languages
- An InterfaceRealization specifies that a Class/Component realizes one or more certain Interfaces
	- Similar to interface implementation in programming languages (e.g. Java)

![[01_UML/image/Pasted image 20250104181516.png]]

## 2.5 ASSOCIATION

- Associations specify semantic relationships (called links) that can occur between typed instances.
- It has at least two association ends (binary association) but could be n-ary
- Association ends have a specific multiplicity, can be ordered, unique, supersets, subsets, derived, unions … because they are Properties
- ==`*`的意义是 Multiplicity== 就是 notation 的值 


![[01_UML/image/Pasted image 20250104191134.png]]

- Each Association end has a particular aggregation kind associated
	- None (Association)
	- Shared (Aggregation)
	- Composite (Composition)
- Determine lifecycle dependencies among instances of Classifiers that are associated
- ==None and Shared means instances are rather loosely coupled==
- Composite describes a whole-part coupling (lifecycle dependency of the part)
- ==注意实心和虚心三角的意义==, 虚心三角代表 loosely coupled, 代表 

![[01_UML/image/Pasted image 20250104191439.png]]

---

- Associations ends are Properties of either the Class at the other end of the Association or, in case of non-navigable association ends, of the Association itself
- Navigability is always possible – also for nonnavigable Association ends
- ==Dot-Notation== was introduced to determine Classifier- or Association ownership of Association ends
	- 实心点代表 这个 association end 属于  另外一边的 association end, 也属于 Association itself

![[01_UML/image/Pasted image 20250104191743.png]]



### 2.5.1 Shared aggregation

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

### 2.5.2 Composition

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


# 3 Object Diagram 

![[01_UML/image/Pasted image 20250104194747.png]]


Object diagrams are used to visualize partial/complete instances (called InstanceSpecifications) of Classifier and Associations

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




