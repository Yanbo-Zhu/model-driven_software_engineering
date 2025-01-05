
![[01_UML/image/Pasted image 20250105160525.png]]

- Behavior diagrams express dynamic aspects of the system (Activities, State Machines etc.)
- The following diagrams types are briefly introduced
	- Use Case diagram
	- Activity diagram
	- State Machine diagram
	- Sequence diagram (as most used example of Interaction diagrams

Rule of thumb in UML: No Behavior without structure


- Structural modelling: Instances of and relations between StructuredClassifiers
- Behavioral modelling: Change of the instances over time
- Two kinds of behaviour occurrences:
	- directly executed: isolated behaviour of a single instance
	- emergent (新兴的，处于发展初期的): Results from the interaction of two or more instances
- Behaviors can be
	- Prescriptive 规范的 : Specifies the allowed sequence of events (execution trace)
	- Descriptive: Specifies (shows) emergent behaviour
	- Illustrative 图解的: Visualizes a snippet of what is actually going on



# 1 USE CASE DIAGRAM


![[01_UML/image/Pasted image 20250105161459.png]]

**Use case diagrams** are usually referred to as [behavior diagrams](https://www.uml-diagrams.org/uml-25-diagrams.html#behavior-diagram) used to describe a set of actions ([use cases](https://www.uml-diagrams.org/use-case.html)) that some system or systems ([subject](https://www.uml-diagrams.org/use-case-subject.html)) should or can perform in collaboration with one or more **external users** of the system ([actors](https://www.uml-diagrams.org/use-case-actor.html)). Each use case should provide some observable and valuable result to the actors or other stakeholders of the system.

- A Use Case diagram describes how external Actors are intended to interact with the system to achieve a (business) goal
- Main constituents are
	- Use Case
	- Actor
	- Extension/Inclusion


## 1.1 UseCase

- A UseCase is a specification of behavior. UseCases are a means to capture the requirements of systems, i.e., what systems are supposed to do.
- UseCases define the offered Behaviors of the system without reference to its internal structure.
- May include possible variations of its basic behavior, including exceptional behavior and error handling.

![[01_UML/image/Pasted image 20250105162129.png]]



## 1.2 Actor

- UseCases are defined in a system context
- Boundarys of that system context are shown by the solid rectangle
- Actors represent interaction partners outside of the context
- Actor might be a human being or another external system
- Associations between Actor and UseCase describe the system interface at very high level

![[01_UML/image/Pasted image 20250105162335.png]]


## 1.3 Extension/Inclusion

- UseCases can be extended (Extend) and included (Include)
- An extending UseCase might be executed during the execution of the extended UseCase (under certain condition)
- An included UseCase is always executed during the execution of the including UseCase
- Use Case can also be specialized

"Withdraw" includes "Card Identification"
card identification is always executed during the execution of Withdraw 
card identification: An included UseCase
Withdraw: An including UseCase


![[01_UML/image/Pasted image 20250105162459.png]]

![[01_UML/image/Pasted image 20250105162807.png]]


# 2 STATE MACHINE DIAGRAM

![[01_UML/image/Pasted image 20250105163611.png]]

---

State machine diagrams visualize a system‘s behavior expressed as state-transition system

Main constituents are
− StateMachine
− State
− Transition
− Trigger
− Guard
− Effect/Entry/Do/Exit behavior

![[01_UML/image/Pasted image 20250105163725.png]]

## 2.1 StateMachine

− StateMachine is one explicit behavior kind of the UML
− A prescriptive specification of a system‘s behavior depending on triggering conditions in a certain State (or many)
− Always expressed in the context of a system (e.g. an ATM)
− Events trigger the transition from one state to another state
− External or internal Events possible

![[01_UML/image/Pasted image 20250105163847.png]]

## 2.2 States

− States represent distinguishable situations during the lifecycle of a system that can be subsumed under a logical label (the state)
− The system is expected to behave and operate semantically equivalent in the very same state
− Conditions and invariants for a State are often only implicitly defined (by label)
− States can be further decomposed into composite states

![[01_UML/image/Pasted image 20250105164128.png]]

## 2.3 Transition, Event, Trigger

Transitions describe changes in the system‘s state due to triggering Events

Events of a Trigger can be one of
− CallEvent (Operation was called)
− SignalEvent (Signal was received)
− ChangeEvent (in parallel region, something changed)
− TimeEvent (a certain amount of time elapsed after a State was entered)
− AnyReceiveEvent (any other invokation)
− CompletionEvent (otherwise)

![[01_UML/image/Pasted image 20250105164500.png]]


## 2.4 guard

- Transitions may have a guard (not a metaclass)
	- Must evaluate to true to allow the transition to fire
	- Otherwise the Event will be discarded (and is gone)
- A set of further Behavior might be executed when firing a transition
	- Transition effect
	- State exit/entry/do Behavior

![[01_UML/image/Pasted image 20250105164811.png]]

## 2.5 Effect/Entry/Do/Exit behavior

- Triggering a transition may execute State-related Behaviors as well
	- Exit Behavior: Executed when the current state(s) is left
	- Entry Behavior: Executed when the target State is entered
	- Do Behavior: Executed after any potential entry-Behavior is finished
- Do-Behavior can be interrupted by subsequent triggering Events and will terminate its execution immediately

![[01_UML/image/Pasted image 20250105165102.png]]

---

- Transition effect can be described with any UML Behavior
- If triggered by a Call-/SignalEvent
	- implicitly all actual parameter values of the invocation are passed to the behavior
	- Represents just a fragment of the invoked Operation (CallEvent) or Reception (SignalEvent) Behavior
- Usually used to do calculations and send responses to the environment

![[01_UML/image/Pasted image 20250105165203.png]]

## 2.6 Run-to-Completion semantics

− State change is an atomic, non-interruptible process, called run-to-completion

Run-to-Completion semantics (abstracted)
− If a triggering Event is dispatched, the Transition guard will be evaluated
− If it evaluates to true, the source State is left, any running Do-Behavior is interrupted and the Exit-Behavior is executed
(potentially nested)
− When the exit-Behavior has finished, the Transition‘s effect-Behavior is executed, iff existing
− The target State is entered and any existing entry-Behavior is executed (potentially nested)
− Next Event in event queue can be dispatched



# 3 ACTIVITY DIAGRAM

![[01_UML/image/Pasted image 20250105165844.png]]


An Activity is a Behavior specified as sequencing of subordinate executable Actions, using a control and data flow model

Main building blocks are
− Actions
− ControlFlow/ObjectFlow
− ControlNode/ObjectNode
− Structured Actions

![[01_UML/image/Pasted image 20250105165930.png]]


## 3.1 ActivityNode

![[01_UML/image/Pasted image 20250105170101.png]]




Flow of execution in an Activity is modeled as ActivityNodes connected by ActivityEdges

Execution of an ActivityNode/ExecutableNode happens because
− Other ActivityNodes in the model finish executing;
− Objects and data become available for a particular ActivityNode; or
− Events occur externally to the flow

Semantics of execution flow of Activities resembles the token flow semantics of Petri Nets


## 3.2 Action 

− An Action is an atomic (or composite) executable building block
− An Action executes once it receives the execution token
− In the presence of InputPins, according values (ObjectNodes) must be present at runtime
− Actions may emit values on OutputPins
− Actions are either predefined (OO-related) or user-defined (opaque) Actions

![[01_UML/image/Pasted image 20250105170258.png]]

## 3.3 ControlFlows and ObjectFlows

− ControlFlows carry at most a single control flow token from one Action to another one
− ObjectFlows exchange at least one object token among Actions over Pins
− Actions can only execute if they receive a token (control or object token)
− ControlFlows are humilated ObjectFlows
− Actions can only execute if they receive a token (control or object token) on all incoming flows


![[01_UML/image/Pasted image 20250105170258.png]]


## 3.4 ControlNodes

ControlNodes control the routing of control and object tokens
− DecisionNode routes the control on mutually exclusive/alternative paths
− Fork duplicates control nodes and initiates concurrent execution flows
− FlowFinalNode destroys a control flow
− JoinNode synchronizes multiple concurrent execution flows
− MergeNode combines multiple flows without synchronization (similar to Join)

![[01_UML/image/Pasted image 20250105170832.png]]

## 3.5 Structured Action: Invocation Actions 


UML defines a set of predefined Actions that are related to the object-oriented paradigm

![[01_UML/image/Pasted image 20250105171432.png]]


Invocation Actions  调用，启用

- Invocation Actions used to call Operation, send Signals and invoke Behaviors
	- Call Operation Action: Calls an operation an a object/Class
	- Call Behavior Action: Calls a standalone Behavior
	- Send Signal Action/Send Object Action: Sends a Signal to a receiver
	- Start Object Behavior Action: Starts the classifier Behavior of an Object (active Class)

![[01_UML/image/Pasted image 20250105171638.png]]


## 3.6 Structured Action: Accept Event Actions

![[01_UML/image/Pasted image 20250105171840.png]]

− Accept Event Actions are Actions that are triggered by the dispatching of an external Event within the context Classifier
− Similar to the Trigger of a Transitions (same Events
− ==AcceptEventActions start when a matching Event is dispatched; no ControlFlow activation needed, but possible nonetheless==

![[01_UML/image/Pasted image 20250105172005.png]]

![[01_UML/image/Pasted image 20250105172013.png]]


## 3.7 Structured Action:  Other 

![[01_UML/image/Pasted image 20250105172216.png]]



# 4 SEQUENCE DIAGRAM

![[01_UML/image/Pasted image 20250105172246.png]]

- Sequence diagrams describe the interaction of roles of a system
- Sequence diagrams are the mostly used Interaction diagram
	- Timing Diagram
	- Communication Diagram
	- Interaction Overview Diagram
- Inspired by ITU Message Sequence Charts
- Heavily used diagram for communication purposes due to intuitive notation

![[01_UML/image/Pasted image 20250105172922.png]]



Sequence diagram are the only diagram kind that describe the exchange of Messages from a bird‘s perspective
Usually used to describe emergent behaviour or illustrate snippets of interactions
Main constituents are
− Interaction
− Lifeline
− OccurrenceSpecification
− Message
− CombinedFragment
− InteractionUse

![[01_UML/image/Pasted image 20250105173016.png]]

## 4.1 Lifelines and OccurrenceSpecifications

− Lifelines (usually) represent roles within a context of loosely coupled instances
− OccurrenceSpecifications are executable parts of Interactions (e.g., sending a Message etc.)
− OccurrenceSpecifications that cover a Lifeline are ordered from top to down and supposed to be executed accordingly

![[01_UML/image/Pasted image 20250105173155.png]]

## 4.2 Messages

− Messages establish information exchange between two Lifelines
− Similar but not identic to InvocationActions and ReceiveEvents (because of the bird’s perspective)
− Messages have a signature and arguments that are exchanged between Lifelines



![[01_UML/image/Pasted image 20250105173426.png]]

## 4.3 ExecutionSpecifications

- ExecutionSpecifications enable the execution of additional Behaviors at a certain point in time of the Lifeline‘s execution
- Similar to a Transition‘s effect
- Rectangles are often used to indicate the execution duration of Operations
	- Misuse of the concept for visualization purposes
	- Often there is no Behavior referenced
- ExecutionSpecifications can be completely omitted if not required

![[01_UML/image/Pasted image 20250105173543.png]]


## 4.4 CombinedFragment

- CombinedFragment are InteractionFragments that may contain other InteractionFragments
- Different operators for CombinedFragments
	- Alternative/Optional (cóndition)
	- Loop (while/for)
	- Par (parallel)
	- Neg (negation)
	- …
- Used to express complex situations over the entire communication scenario

![[01_UML/image/Pasted image 20250105173736.png]]

## 4.5 InteractionUse

− InteractionUse represents invocations of other Interactions
− Similar to CallBehaviorAction in Activities
− Enables the possibility to decompose Interactions
− Enables the pass-on of actual parameter to the invoked Interaction

两个 Lifelines  Web Customer and Online Bookshop
一个 InteractionUse:  Checkout 

![[01_UML/image/Pasted image 20250105173820.png]]


![[01_UML/image/Pasted image 20250105174053.png]]






