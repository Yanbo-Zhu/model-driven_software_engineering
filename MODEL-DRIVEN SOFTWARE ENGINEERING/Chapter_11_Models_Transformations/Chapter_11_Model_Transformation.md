
Model transformations describe mappings between instances of metamodels or other conceptual spaces

There are two general kinds of model transformations
− Model-to-model (M2M): An important part of the model-driven software engineering is the creation of models
out of existing models
− Model-to-text (M2T)


# 1 M2M OVERVIEW


## 1.1 Introduction

− Mapping between metamodels
- Execution maps instances of the source metamodel to an instance of the target metamodel
- Source and target metamodel could be the same
- Source and target model could be the same (in-place transformation)


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401213300.png]]


## 1.2 Classification


### 1.2.1 EXECUTION LOGIC


Imperative
− Describes “how” it shall be done
− Procedures, sequences of steps
− Control flows and structures (e.g. loops)
− Examples: QVT-OM, Xtend2


Declarative
− Describes “what” shall be done, “how” is only an implementation detail of the transformation engine
− In particular the order of the execution of the mapping rules is irrelevant
− Can be used for rule inference and implicit rule evaluation
− A rule inference is done based on needed type mapping
− (e.g. a mapping of type “A” to type “B” is need and a corresponding mapping is automatically selected and used.
− Example: QVT-R, ATL, Triple Graph Grammar based technologies (e.g. MoTE / Fujuba, GReAT, EMorF)

Example of a declarative transformation rule using QVT-R, graphical notation
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401213957.png]]

### 1.2.2 TRANSFORMATION RULES

Bi-directionality
− Rule may be executable in the inverse direction
− Feature of declarative languages
− Many = multi directionally (QVT-R)
− QVT-R rules must be explicitly written in a way to be multi-directional
− Useful for model synchronization

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401214134.png]]


---

RELATIONSHIP BETWEEN SOURCE AND TARGET

− New target
	− New target model is created each time
− In-Place transformation
	− Source and target are the same
	− Destructive update
		− Changes the model freely
	− Extension only
		− Allows only extension and ensures termination



![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401214213.png]]



## 1.3 Concepts OF M2M LANGUAGES


Cyclic model references

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401214614.png]]

How to deal with cyclic model dependencies
− Check explicitly for already created/existing instance (via transformation traces)
	− Imperative style (QVT-OM)
− Implicitly resolving existing instances via transformation trace
	− “Cached rules” same result of the input is the same (Xtend, ATL)
	− Key concept for declarative languages (QVT-R)
		− Instance of a metamodel element are the same if their value of the key attribute are the same


----


polymorphic dispatch / multiple dispatch / dynamic dispatch


polymorphic dispatch / multiple dispatch / dynamic dispatch
− Function calls: When using function overloading (same name but different signature), called overload is chosen on the parameter value runtime type, instead of the static type of the expression
− This is not specific for transformation languages but here it is particularly useful
− Semantically similar to instance method overriding in OO languages


So why useful for transformation languages?
→ Avoids case handling for types.

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401214945.png]]

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401214954.png]]


---

− Conservative transformations
	− Changes only what's necessary – leaves much as possible untouched
	− Advantage of declarative languages
		− Rules define the state of source and target
		− If the target is in a different state, it will get modified until the declared state is reached

− Incremental transformations
	− Like conservative transformation but with minimum required computation
	− Does not iterate over complete (‘reacts’ only to changes, e.g. RETE based)

− Model synchronisation (bidirectional and multi-directional transformations)
	− Rule definition is multidirectional (from each model to each other model)
	− Conservative or incremental transformation in all directions


## 1.4 Introduction into QVT Operational Mappings: QVT-OM


„Meta Object Facility (MOF) 2.0 Query/View/Transformation“, or QVT for short, defines three languages:
− QVT Core (QVTc / QVT-C)
− QVT Relational (QVTr / QVT-R)
− QVT Operational Mappings (QVTo / QVT-OM)


− QVT Operational Mappings
	− Is a imperative language for unidirectional M2M transformations
	− QVT-OM is based on Object Constraint Language (OCL)
		− Extended by imperative concepts
		− Extended by class library (including „mutable collections“)
	− Supports explicit lookup in the implicitly created transformation trace
− There are two existing implementations
	− Eclipse QVT Operational
	− Smart QVT (?)

之后的没看  自己去看课件 








# 2 MODEL 2 TEXT

## 2.1 Introduction


Translate models into textual representations
− Usually lowering the abstraction level (target mostly code or configuration file)

Mostly similar approach to template languages (e.g. JSP, ASP or PHP)
− Static text with inserts of constructs and dynamic output
− Include construct for compact querying of data structures (data collection)


## 2.2 Augmenting Generated Code

Problem: Generated source code often not functionally complete, since model too abstract
Hence: Concepts for manual enrichment of generated data is necessary Generation-Gap Pattern (attributed to John Vlissides)
1. Generate a super class which does not need to be changed
2. Generate a sub class which can then be enriched by the user
3. On re-generation only generate the non changed super class, compiler will let you know if something is wrong with the sub class

Other destination language specific mechanisms
− Merge mechanism (e.g. JMerge, as used by EMF Ecore code gen)
− „Weaving“ of manual code via aspect orientation (e.g. AspectJ, AspectC)



Other solutions for manual extensions:
Protected regions
− Special indicated areas in the source code which will not be overridden on re-generation

Variations of the Generation-Gap Pattern
− Using partial classes if supported (e.g. C#)
− Sub classes with protected regions

Using three-way-merge, as known from source control systems


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_11_Models_Transformations/image/Pasted image 20250401220807.png]]



## 2.3 Challenges with Tracing

Tracing challenges
− Necessity to understand relation between from source model and generated source (in a chain even over multiple M2M transformations) and vice versa (how does it come that this particular code has been generated/ why is something missing)

Alternatives to M2T: only working on meta models using M2M (CST/AST)
− Formal projection to concepts of the destination language
− Consistency check for valid output
− Serialization in syntax of the destination language
− Traces based on meta model -> independent from textual representation
− Usually implementation overhead
− For protected regions incremental M2M transformation necessary
− Sometimes used in conjunction with “projectional editing”


## 2.4 Challenges with Output Formatting


Formatting („Whitespace“ Handling)
− Formatting template vs. formatting output
− Usage:
	− Formatting according to readability of templates and automatic formatting after generation (problematic with Traces)
− Possibilities of handling „Whitespace“ in template languages
	− Indent through template elements for control flow not considered on output (e.g. Xtend)
	– Line begin indicator, controls the begin of a line in the output (e.g. MOF M2T)
	– Complete template-output is indented according to template indent (e.g. Acceleo)




## 2.5 Introduction into MOF M2T (Acceleo)

− Implementation of the OMG standard for M2T transformations
„MOF Model to Text Transformation Language”
	− Based on side effect free OCL
	− Acceleo supports proprietary extensions
− Project is hosted and developed within the Eclipse Foundation


具体的没有看 自己看课件 
