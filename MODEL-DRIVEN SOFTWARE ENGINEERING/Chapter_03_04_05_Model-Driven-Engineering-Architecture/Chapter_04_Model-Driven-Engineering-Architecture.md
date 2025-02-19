

# 1 Model Driven Engineering


## 1.1 FUNDAMENTAL PRINCIPLES

FUNDAMENTAL PRINCIPLES USED IN MDE TO COPE WITH COMPLEXITY

Formalisation
• Precise description with syntax and semantics


Automation
• Doing routine jobs


Supporting abstraction, aspects and views
• Concentration on certain relevant aspects


Traceability
• Supports consistency

− Traceability of links between model elements
	− Within a model
	− Between models
− Traceability between model elements and artefacts
− Supports navigation through the system model
− Supports consistency of models


## 1.2 PRINCIPLES, STANDARDS AND TOOLS

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219150133.png]]




# 2 Model-Driven-Architecture

## 2.1 MDA Definition 

MDA is a way to look at software development, from the point of view of the models.

Separates the operational specification of a system from the details such as how the system uses the platform on which it is developed.

MDA provides a means to:
- Specify a system independently of its platform
- Specify platforms
- Chose a platform for the system
- Transform the system specifications into a platform dependent system
- Three fundamental objectives: portability, interoperability and reuse.

## 2.2 MDA FUNDAMENTALS

− Abstraction:
	− CIM: Computation Independent Model
	− PIM: Platform Independent Model
	− PSM: Platform Specific Model
− Transformations:
	− Between different levels of abstraction
	− Enriched models: notes, composition,…
− Everything is a Model:
	− Metamodel and Meta-metamodel = Models of Models

## 2.3 BENEFITS OF MDA

− Allows implementation flexibility regarding platform choice. Reducing the impact of technological changes
− Reuse
− Improves software development process:
	− Expressing the solution in terms of the domain specific problem.
	− Earlier detection of problems.
	− Automation of parts of the development process.
− Improves development maintenance: Models are an active part of the design process not solely documentation.
− Eases requirement traceability:
	− Improving change control
	− Improving solution validation


## 2.4 MDA BASIC ELEMENTS

− MODELS: cornerstone of MDA.
	− Metamodels: everything is a model. MOF. EMF (Eclipse).
	− UML profiles: Adapted modelling language.


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219144729.png]]




− Transformations
	− Models with notes
	− Metamodels mapping
	− MOF QVT
	− Code generation: transformation


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219144809.png]]


− Model composition
	− Composite solutions (federated systems, multiplatform systems,...)
	− Non functional aspects


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219144821.png]]


## 2.5 GENERAL MDA ORGANIZATION

− Software development considered as chain of transformations

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219144916.png]]


## 2.6 SOME MDA STANDARDS

UML MM : description of OO software artifacts
SPEM MM: how to use and produce them
QVT MM: how to generate models from other models


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_03_04_05_Model-Driven-Engineering-Architecture/image/Pasted image 20250219145047.png]]


# 3 OBJECT ORIENTED ANALYSIS


## 3.1 BASIC CONCEPTS OF OBJECT-ORIENTED ANALYSIS

− The essential aspects of object-oriented modeling are:
	− Abstraction: Complex details are hidden, and only relevant information is presented.
	− Encapsulation: Data and methods are grouped together in a class and are only accessible through defined interfaces.
	− Inheritance: Classes can inherit properties and behavior from other classes.
	− Polymorphism: Objects of a derived class can be treated as objects of the base class.

− Means used in object-oriented analysis
	− Objects: Everything is considered as an object that contains data and has behavior.
	− Classes: Objects are defined by classes that act as blueprints.
	− Communication: Objects interact
	− Modularity: The system is divided into smaller, reusable modules.
	− Relationships: Objects can have relationships like associations, aggregations, and compositions.



## 3.2 OOA – OBJECT ORIENTED ANALYSIS


The object-oriented analysis is a process for examining and understanding a system or problem from an object-oriented perspective.

Basic steps:
- Identify basic objects:
	- Identify the relevant objects in the system or problem being studied. This is done through analysis of requirements, documentation, and discussions with stakeholders.
- Defining classes:
	- Based on the identified objects, define classes that describe the common properties and behavior of the objects. This includes considering the hierarchy of classes and the relationships between them.
- Identification of attributes:
	- Identify the attributes or data that describe the properties of the objects. These attributes are associated with the respective classes.

- Identification of methods:
	- Identify the methods or operations that describe the behavior of the objects. These methods are associated with the respective classes.
- Establishing relationships:
	- Identify the relationships between the classes, such as associations, aggregations, or compositions. These relationships are used to represent the interactions between the objects.
- Refinement and iteration:
	- The analysis process is iterated to refine the identification of objects, classes, attributes, methods, and relationships. This is done through feedback and validation of the results with stakeholders.










