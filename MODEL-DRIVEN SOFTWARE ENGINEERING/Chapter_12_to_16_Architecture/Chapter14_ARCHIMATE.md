



# 1 Introduction


Archimate 
• Open, independent Enterprise Architecture standard for describing, analyzing and visualizing business architecture
• Hosted by The Open Group, aligned with TOGAF
• Facilitates stakeholder communication and impact assessments for architecture design and changes
• Provides concepts for specifying interrelated architectures and offers viewpoint and customization mechanisms
• Lightweight, general set of architectural concepts, but also applicable for individual architecture domains
• Consists of a core framework and several model extensions

# 2 Structure and Concepts

## 2.1 Core Framework 

Element Classification
• Nine cells to classify elements
• Dimensions: Aspects and Layers
• Elements are not strictly confined to one aspect or layer

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203142.png]]


### 2.1.1 Aspects
• Active Structure Aspect: structural elements (actors, components, devices) that display actual behavior
• Behavior Aspect: behavior (processes, functions, services…) performed by actors
• Passive Structure Aspect: objects on which behavior is performed (information / data objects)


### 2.1.2 Layers

Business Layer: business services offered to customers, realized by business processes and performed by business actors
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203316.png]]


Application Layer: application services supporting the Business Layer, realized by software applications
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203333.png]]


Technology Layer: technology that offers infrastructure services to run applications, realized by softand hardware components
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203353.png]]


Additional Layers and Aspects
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203535.png]]



## 2.2 Elements, Relationships and their Notation

就是如何画图  详细的见 课件 
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203631.png]]

# 3 Stakeholders, Views and Customization

## 3.1 VIEWS AND VIEWPOINTS

• Tailored Views: architects and stakeholders can create their own views of the Enterprise Architecture to address a specific set of concerns
• Viewpoints: frame stakeholder concerns and set conventions for constructing views, determining which parts of the architecture are relevant and visible
• Views: specific representations that convey information about architecture areas, customized for particular stakeholders and defined by their viewpoints
• Viewpoint Mechanism: helps architects select and classify appropriate viewpoints based on two dimensions (purpose and content)
• Creating Viewpoints and Views: involves selecting relevant concepts from the metamodel and defining a representation to effectively communicate these concepts to stakeholders

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408203915.png]]

## 3.2 LANGUAGE CUSTOMIZATION

• General and Specialized Use: basic elements for general modeling and customization for specific needs (without complicating the language with unnecessary concepts)
• Adding Attributes: users can enrich model concepts with attributes using a profiling mechanism that allows creating predefined or custom profiles with various data types
• Specialization of Concepts: users may create specialized elements and relationships (with inherited core properties) to introduce new attributes, restrictions or notations that meet specific organizational needs




# 4 Tools


Bitte installieren Sie sich dafür schon mal das Open Source Tool Archi:
https://www.archimatetool.com/

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_12_to_16_Architecture/image/Pasted image 20250408204218.png]]




# 5 Modelling Task

