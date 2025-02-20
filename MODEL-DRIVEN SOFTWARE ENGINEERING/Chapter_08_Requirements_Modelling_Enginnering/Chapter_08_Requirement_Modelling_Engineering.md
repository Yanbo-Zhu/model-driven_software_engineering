

==(Model) Requirements Engineering = Unklare Anforderungen beseitigen!==

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220135432.png]]

# 1 Requirement_Modelling



aus Model-driven Requirement Modelling 

− Very Short Summary of Requirements Engineering Basics
− Requirements Modelling
− Modelling Approaches (not only for requirements)



## 1.1 REQUIREMENTS DEVELOPMENT


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220104801.png]]


## 1.2 GENERAL CONCEPTUAL MODELS FOR SOFTWARE SYSTEMS

− Conceptual Models (general views)
	− Structure
		− Structural decomposition
− Interaction
	− Between system parts or external systems
− (Internal) behaviour
	− Programming logic, algorithms

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220105232.png]]





− Structure Perspective
	− structure of input/output data
	− static structural aspects
− Functional Perspective
	− behavioral description of system functions (how the data is manipulated by the system)
− Behavioral Perspective
	− behavioral description of events and how the system reacts
	− System state changes


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220105256.png]]


## 1.3 Design viewpoint

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220110551.png]]


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220110655.png]]



## 1.4 Dynamic View

Program = data + algorithms! With this simple statement, Nicholas Wirth has summarized a complex fact in a memorable way. Applying this equation to requirements, in this chapter we will focus on the desired or required functionality of a system and its behavior (following the description of information models in Chapter 3).

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220110907.png]]



## 1.5 ARC42 (写 document 的内容模版)


ARC42 is a lightweight method for documenting and describing the architecture of software systems.
- ARC42 is a template or guide for documenting software architecture.
- It provides a structured approach to describe and document different aspects of the architecture.
- ARC42 covers various components, such as system context, functional and nonfunctional requirements, component and runtime views, distribution and data views, and user interface descriptions.
- The focus of ARC42 is on practicality and understandability of architecture documentation.
- It promotes effective communication and collaboration among project stakeholders.
- ARC42 can be used in conjunction with other software engineering practices and standards.


- Template for document structure
- There are also Viewpoints and Perspectives

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220111528.png]]


1. Einführung und Ziele
	1. Kurze Beschreibung und Extrakt der requirements, Die Top3 (bis maximal 5) Qualitätsziele für die Architektur, deren Erreichung für die wichtigsten Stakeholder kritisch ist. Eine Übersicht über die wichtigsten Stakeholder mit deren Erwartungen bezüglich der Architektur.
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112202.png]]
2. Randbedingungen
	1. Alles, was das Team beim Design und der Implementierung der Architektur einschränkt. Diese Einschränkungen sind manchmal auch außerhalb eines Projekts in der gesamten Organisation gültig.
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112209.png]]
3. Kontextabgrenzung
	1. Grenzt das System, an dem Sie arbeiten, von externen Kommunikationspartnern (Nachbarsystemen und Benutzern) ab. Spezifiziert die externen Schnittstellen aus Sicht des Business (immer) und aus Sicht der Technologie (optional)
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112400.png]]
4. 5. Bausteinsicht 
	1. Statische Zerlegung des Systems. Die Abstraktion des Sourcecodes, dargestellt als Hierarchie von 'White-Boxes• (die wiederum kleinere Black-30xes beinhalten), bis zu einem angemessenen Detaillierungsgrad.
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112432.png]]
5. Laufzeitsicht
	1. Das Verhalten der Bausteine in Form von dynamischen Szenarien, die die wichtigsten Prozesse Oder Features abdecken, Interaktionen an kritischen externen Schnittstellen Oder "interessante" interne Abläufe und kritische Ausnahme- Oder Fehlerfälle. 
6. Verteilungssicht
	1. Technische Infrastruktur mit (echten Oder virtuellen) Prozessoren, Systemtopologie, und die Abbildung der Software-Bausteine auf diese Infrastruktur.
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112641.png]]
7. Qualitätsanforderungen
	1. Qualitätanforderungen in Form von Szenarien, mit einem Qualitätsbaum für den Überblick. Die allerwichtigsten dieser Qualtätsanforderungen sollten schon im Kapitel 1.2. (Qualitätsziele) aufgeführt sein.
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112742.png]]
8. Glossar
	1. Wichtige Domänenbegriffe und technische Begriffe, die Stakeholder kennen sollten, wenn sie über die Architektur des Systems diskutieren. Manchmal auch Übersetzungstabellen, wenn in einer mehrsprachigen Umgebung gearbeitet wird. 
	2. ![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220112806.png]]


# 2 Requirement Engineering


− How to get the requirements
− How to document the requirements
− How to manage the requirements
− How to use models for requirement engineering (getting, documenting, managing)


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220114247.png]]



## 2.1 REQUIREMENTS MANAGEMENT

### 2.1.1 Documentation of requirements 
Recommended Practice for Software Requirements Specifications  (SRS)

It is based on a model in which the result of the software requirements specification process is an unambiguous and complete specification document


− IEEE Std 830-1998 provides background information
	− Nature of the SRS
	− Environment of the SRS
	− Characteristics of a good SRS
	− Joint preparation of the SRS
	− SRS evolution
	− Prototyping
	− Embedding design in the SRS
	− Embedding project requirements in the SRS


− Nature of a good SRS
	− Functionality - What is the software supposed to do?
	− External interfaces - How does the software interact with people, the systems hardware, other hardware, and other software?
	− Performance - What is the speed, availability, response time, recovery time of various software functions, etc.?
	− Attributes - What are the portability, correctness, maintainability, security, etc. considerations?
	− Design constraints imposed on an implementation - Are there any required standards in effect, implementation language, policies for database integrity, resource limits, operating environment(s) etc.?


− Environment of a good SRS
	− Should correctly define all of the software requirements. A software requirement may exist because of the nature of the task to be solved or because of a special characteristic of the project.
	− Should not describe any design or implementation details. These should be described in the design stage of the project.
	− Should not impose additional constraints on the software. These are properly specified in other documents such as a software quality assurance plan.

Characteristics of a good SRS
- Correct
	- if, and only if, every requirement stated therein is one that the software shall meet
- Unambiguous
	- if, and only if, every requirement stated therein has only one interpretation
- Complete
	- if, and only if, it includes the following elements
	- All significant requirements, whether relating to functionality, performance, design constraints, attributes, or external interfaces
	- Definition of the responses of the software to all realizable classes of input data in all realizable classes of situation
	- full labels and references to all figures, tables, and diagrams
- Consistent
	- if, and only if, no subset of individual requirements described in it conflict
- Ranked for importance and/or stability
	- if each requirement in it has an identifier to indicate either the importance or stability of that particular requirement
- Verifiable
	- if, and only if, every requirement stated therein is verifiable 
	- A requirement is verifiable if, and only if, there exists some infinite cost-effective process with which a person or machine can check that the software product meets the requirement
- Modifiable
	- if, and only if, its structure and style are such that any changes to the requirements can be made easily, completely, and consistently while retaining the structure and style
- Traceable
	- if the origin of each of its requirements is clear and if it facilitates the referencing of each requirement in future development or enhancement documentation
	- Backward traceability (i.e., to previous stages of development). This depends upon each requirement explicitly referencing its source in earlier documents
	- Forward traceability (i.e., to all documents spawned by the SRS). This depends upon each requirement in the SRS having a unique name or reference number.


Template 
- Einleitung
	- Zweck,
	- Stakeholder
	- Referenzen
- Übersicht
	- Systemumfeld
	- Architektur
	- Benutzer
	- Randbedingungen
- Anforderungen
	- Funktionalität
	- Qualitätsanforderungen
- Abnahme
	- Akzeptanzkriterien
	- Testszenarien
- Anhang
	- Glossar
	- Index



## 2.2 Requirements Development 

− Domain analysis: the environment for the system-to be is studied. The relevant stakeholders are identified and interviewed. Problems with the current system are discovered and opportunities for improvement are investigated. Objectives for the target system are identified.
− Elicitation 引出；诱出；抽出: alternative models for the target system are analyzed to meet the identified objectives. Requirements and assumptions on components of such models are identified. Scenarios could be involved to help in the elicitation process.
− Negotiation and agreement: alternative requirements and assumptions are evaluated; risks are analyzed by the stakeholders; the best alternatives are selected.
− Specification: requirements and assumptions are formulated precisely.
− Specification analysis: the specifications are checked for problems such as incompleteness, inconsistency, etc. and for feasibility.
− Documentation: various decisions made during the requirements engineering process are documented together with the underlying rationale and assumptions.
− Evolution: requirements are modified to accommodate corrections, environmental changes, or new objectives.


### 2.2.1 Eliitation

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220115916.png]]


# 3 Analysis/Specification/Verification 


Complete
− use of different types of requirements (functional, quality, interface, constraints)
− use of specific templates 

− e.g. Generic Requirements Types
	− Functional – what the system shall do
	− Quality (non-functional) – how the system shall perform the function
	− Constraints – Boundaries of the system context


Functional: The Rover shall calculate maximum operating time based on battery power state.
Non-Functional: The Rover shall perform all calculation operations in less then 10ms.
Constraint: The Rover is powered by a radioisotope thermoelectric generator (RTG).




---

Unambiguous
− requirement Specification Language – RSL
− glossary – defined set of terms

− Requirement shall be atomic
	− Bad:
	The Rover shall contact the orbiter every 5mins and the ground control every 10mins.
	As Administrator I want to monitor rover state so that I can react quickly if something is wrong with the rover.
	− Better:
	R1: The Rover shall contact the orbiter every 5mins
	R2: The Rover shall contact ground control every 10mins
− e.g. structure of sentences, templates, boilerplates, RSL
	− Boilerplates examples:
		User Story: `As <a role> I want to <activity> so that <benefit>`
		Requirement:` The <system> shall <action>`
		The Rover shall provide information about all system states 


---


Ranked for importance
− one-criteria `[IEEE 830-1998]`
− Kano-Classification
− Wiegers prioritization matrix

- Some requirements may be essential, especially for life-critical applications, while others may be desirable
- degree of stability
	- can be expressed in terms of the number of expected changes to any requirement based on experience or knowledge of forthcoming events that affect the organization, functions, and people supported by the software system
- degree of necessity
	- Another way to rank requirements is to distinguish classes of requirements as essential, conditional, and optional.




---




Consistent
− formal requirement models for automated consistency checking
− proving is almost tool supported


Correct
− formal `[complete + consistent]`
− practical `[meet business goals]`


Traceable
− use of authoring tools
− definition of types of dependencies (e.g. SysML)



Verifiable
− specification of validation/verification criteria
− quantification


---


− A (requirement) model is a representation of a system
− A (requirement) model is written in the language of its unique (requirement) metamodel
− A (requirement) metamodel is written in the language of its unique metametamodel
− The unique MMM of the MDA is the MOF
− A (requirement) model is a constrained directed labeled graph
− A (requirement) model may have a visual graphical representation (sometimes)

− Building (requirement) models as a help to understand or to build systems is an old activity, still very useful.
− (Requirement) Models are constrained by the language in which they are written; Metamodels are the central tool to define these languages.
− Advanced modeling frameworks also provide specific languages to write metamodels (like MOF) and specific languages to write transformations between models.

# 4 REQUIREMENTS INTERCHANGE FORMAT - REQIF


STRUCTURED DOCUMENTS
− structured documents are also models (they have a metamodel)
− e.g. Requirements ID, Description, Priority or Type
− natural language
− understandable


Requirements Interchange Format (ReqIF)    
将 STRUCTURED DOCUMENTS in natural language 转变为  schema 形式的 
− OMG Specification: http://www.omg.org/spec/ReqIF/1.2/
− defines a meta-model for requirements
	− contains both meta-data and data
	− defines XML-Schema
	− reference implementation - Requirements Modelling Framework (RMF)
− tool support available (e.g. Doors, ProR)

− ReqIF is root element and contains
	− ReqIFHeader
	− ReqIFContent
	− ReqIFToolExtension
		− describes some tool specific extensions if needed

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220135015.png]]

− ReqIFContent
	− key concept is Specification (which acts as a container for requirements)
	− requirements are represented as SpecObjects
	− traces, links, relations between requirements are represented as SpecRelation

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220135113.png]]

− hierarchical structure of specifications is represented by SpecHierarchy
− SpecRelation has source and target (SpecObjects)

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220135150.png]]



# 5 SYSTEMS MODELING LANGUAGE (SYSML)

− Systems Modeling Language (SysML)
	− supports the specification, analysis, design, verification, and validation of a broad range of complex systems
	− is defined as extension to UML standard

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220140434.png]]

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220140442.png]]

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220140451.png]]


# 6 REQUIREMENTS MODELLING APPROACHES

− IREP Requirements Modeling
− Goal-Oriented
	− Non-Functional Requirements (NFR)-Trees
− Scenario-Oriented
	− Feature-Oriented Requirements Modelling
	− Data Driven Requirements Engineering

## 6.1 Goal-oriented Approach

Scenario-Oriented Requirements Engineering example

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220141402.png]]

− Product Backlog
	− It’s all consumer and business needs that’ll be solved by the product. It’s kept and evaluated by the Product Owner. They’re not bound to use the User Story format for it, though.
− Epic
	− It’s a user story that’s not been detailed yet, or is really long, or is still packed full of uncertainty and thus cannot be turned into product incrementation.
− User Story
	− a User Story is a short format to write down the requirements to build a product.
− Tasks
	− Tasks are the necessary items for a user story to become an increment to a product

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_08_Requirements_Modelling_Enginnering/image/Pasted image 20250220141434.png]]


