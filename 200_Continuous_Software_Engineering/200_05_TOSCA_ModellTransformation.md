
来自 MODEL-DRIVEN SOFTWARE ENGINEERING 这门课的 Chap19_MDQE-DevOps_VL

# 1 Einführung in TOSCA

- TOSCA: Topology and Orchestration Specification for Cloud Applications
- Offener Standard von OASIS
- TOSCA ist eine deklarative DSL basierend auf YAML
- Garantiert Portabilität für mehrere Cloud Provider und Plattformen
- Unterstützt alle Software-Lebenszyklus-Phasen, insbesondere Deployment und Maintenance


TOSCA Metamodell
67
- Templates werden bei der Instanziierung zu den
zugehörigen Entities
Topologie wird mit Entities modelliert
- Nodes haben Requirements und Capabilities
- Requirements müssen durch Relationships mit
anderen Nodes erfüllt werden
- Types können mit Profiles importiert werden
- Default Profile: TOSCA Simple Profile
Orchestrierung mit Plan und Operationen
- Interface einer Node definiert Operationen
- Artefakte implementieren Nodes und Operationen
- Plan modelliert Workflow mit z.B. BPMN

![[200_Continuous_Software_Engineering/image/Pasted image 20250408205850.png]]



# 2 Modelltransformationen

- Deployment mit technologieagnostischen Modellen modellieren
- Modelltransformationen zu technologiespezifischen Modellen von Deployment-Technologien ermöglichen Ausführen des Deployments in verschiedenen Technologien
- Beispiel: TOSCA oder EDMM spezifizieren Metamodell für technologieagnostische Modellierung

# 3 TOSCA Modelltransformationen


![[200_Continuous_Software_Engineering/image/Pasted image 20250408205941.png]]

![[200_Continuous_Software_Engineering/image/Pasted image 20250408205947.png]]


# 4 The Essential Deployment Metamodel (EDMM)

- EDMM nutzt nur eine essentielle Teilmenge der Entities von TOSCA


![[200_Continuous_Software_Engineering/image/Pasted image 20250408210021.png]]

![[200_Continuous_Software_Engineering/image/Pasted image 20250408210043.png]]


# 5 EDMM-Tool 

- Graphische Modellierung mit Eclipse Winery (Tool für TOSCA Modellierung)
- EDMM-CLI transformiert EDMM-YAML-Modelle zu technologiespezifischen Deployment-Modellen
- Unterstützt 13 populäre Deployment-Technologien, z.B. Docker Compose, Kubernetes, Ansible, Terraform


![[200_Continuous_Software_Engineering/image/Pasted image 20250408210059.png]]


