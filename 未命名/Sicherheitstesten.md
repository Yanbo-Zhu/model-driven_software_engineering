
am 2024-01-16 10 -12 

![[未命名/image/Pasted image 20250116114420.png]]

# 1 Grundlagen Sicherheitstesten

![[未命名/image/Pasted image 20250116103549.png]]

![[未命名/image/Pasted image 20250116103626.png]]

# 2 Gängige Schwachstellenarten

![[未命名/image/Pasted image 20250116103821.png]]

![[未命名/image/Pasted image 20250116104110.png]]

# 3 Arten von Sicherheitstesten

![[未命名/image/Pasted image 20250116104204.png]]


## 3.1 Statische Analyse 

Grundlagen der statischen Analyse
• Methode zur Überprüfung von Softwarecode ohne dessen Ausführung
• Dient zur Identifizierung von Fehlern, Sicherheitslücken und Qualitätsproblemen
• Analysiert das System auf strukturelle und syntaktische Fehler, um potenzielle Probleme frühzeitig zu erkennen.

Code Review
• Manuelle Uberprüfung des Codes durch Entwickler (Oder Tester), um Sicherheitslücken und Qualitätsprobleme zu identifizieren.

Alle Softwareartefakte können reviewed werden
• Anforderungsspezifikation, Systemdesign, Modelle, Quellcode, ...

Einsatz von Tools zur automatisierten Analyse
- Erkennung von typischen Fehlermustern: z.B. Fehlende Oder unzureichende Validierung von Benutzereingaben. 是否按照 muster 执行 
- Analyse von Kontroll- und Datenflüssen

Coding phase (IDE Approach)
• Bereitstellung von Echtzeit-Feedback für Entwickler
• Behebung von Problemen, bevor sie den Code committen
• Empfehlungen zur Behebung entdeckter Probleme

Building phase (Cl Approach)
• Entwickler integrieren den Code in ein gemeinsames Repository
• Jede Integration Wird durch automatisierte Analyse überprüft.


Typische statische Analye Finding
![[未命名/image/Pasted image 20250116105012.png]]


Gängige statische Analyse Werkzeuge
- sonarqube
- Checkmarx
- GitHub
- ESLint
- Gitlab 

Herausforderungen bei der Statischen Analyse
- Komplexität des Code
	- Die Komplexität des Codes kann die statische Analyse erschweren, insbesondere bei großen und verschachtelten Codebasen.
- Dies erfordert leistungsstarke Tools.

Abstraktion führt zu Falschpositive Ergebnisse
- Falschpositive Ergebnisse sind ein häufiges Problem bei der statischen Analyse und können zu unnötigen und hohen Analyseaufwand führen.



### 3.1.1 Beispiel: Implementierung eines Passwortwechsels 


![[未命名/image/Pasted image 20250116105257.png]]

Sicherheitslicke: 
- Passwort-leak in Zeile 5: (passwort aus Zeile 2 )

![[未命名/image/Pasted image 20250116105559.png]]



## 3.2 Dynamische Analyse 

Überprüfung der Software während der Ausführung, um reale Angriffe zu simulieren.
• Erkennen von Sicherheitslücken in einer Anwendung
• Ausführung simulierter Angriffe
• Beobachtet, wie die Anwendung auf speziell gestaltete Anfragen reagiert


![[未命名/image/Pasted image 20250116110034.png]]

---


![[未命名/image/Pasted image 20250116110301.png]]


Testmethoden
• Vulnerabilities scans
• Port scans
• Fuzzing
• (Distributed)-DeniaI-of-Service attacks - (D)DOS
• Code injection attacks
• Brute force attacks


---

Nachteile von Dynamische Analyse 
- Abhängigkeit von Testfällen
- Fehlende Abdeckung von Code-Pfaden
- Umgebungsabhängigkeit
- Schwierigkeit bei der Reproduzierbarkeit
- Zeitintensiv
- Höhere Kosten
- Sicherheitsrisiken


![[未命名/image/Pasted image 20250116110557.png]]


# 4 Fuzzing (如何产生 eingabe 数据 , 用于测试 modell)

绒毛 , 模糊化 


![[未命名/image/Pasted image 20250116111234.png]]


![[未命名/image/Pasted image 20250116111335.png]]


![[未命名/image/Pasted image 20250116111405.png]]

![[未命名/image/Pasted image 20250116112515.png]]


## 4.1 Fuzzing Techniken 

- Mutation-Based Fuzzing
	- Mutation-Based Fuzzing modifiziert bestehende Eingabedaten, um neue Testfälle zu generieren. E
	- infach zu implementieren, aber weniger effektiv bei komplexen Systemen.
- Generation-Based Fuzzing
	- Generation-Based Fuzzing erstellt neue Testdaten basierend auf spezifischen Regeln (z.B. Grammatiken Oder Modelle) 
	- Effektiver bei der Erkennung von tief liegenden Fehlern, erfordert jedoch mehr Aufwand.
	- (Genetieren die Daten, die Modell nicht vorgesehen sind, invalide Daten)
- Coverage-Guided Fuzzing
	- Coverage-Guided Fuzzing verwendet Code- Abdeckungsmetriken, um gezielte Tests durchzuführen. 
	- Besonders effektiv bei der Identifizierung von schwer zu findenden Fehlern.



Mutation-Based Fuzzing

![[未命名/image/Pasted image 20250116112119.png]]

---


Generation-Based Fuzzing

![[未命名/image/Pasted image 20250116112157.png]]

![[未命名/image/Pasted image 20250116112324.png]]


# 5 Trends und Ausblick 


Gutenergebnisseprasentation: statusche Analyse 能直接显示 代码哪里错了, 所以更好

![[未命名/image/Pasted image 20250116112957.png]]


最好是 两个方法一起使用 kombininieren
![[未命名/image/Pasted image 20250116113604.png]]

Interaktives Testen
- Kombination aus statischer Analyse und dynamisches Testen
- Analyse des Quellcodes
	- Analyseergebnisse -> Generierung geeigneter Testdaten
- Testausführung
	- Laufzeitinformationen -> Zusätzliche Informationen für statischen Analyse


## 5.1 Beispiel: 两个测试方法 相结合 

![[未命名/image/Pasted image 20250116113306.png]]



![[未命名/image/Pasted image 20250116113401.png]]


![[未命名/image/Pasted image 20250116113414.png]]

![[未命名/image/Pasted image 20250116113439.png]]

---

![[未命名/image/Pasted image 20250116113854.png]]


![[未命名/image/Pasted image 20250116114041.png]]



# 6 Zusammenfassung

- Sicherheitstesten ist ein Prozess zur Identifizierung von Schwachstellen in Software, um deren Vertraulichkeit, Integrität und Verfügbarkeit zu gewährleisten.
- Sicherheitstest können sowohl statisch als auch dynamisch erfolgen Bei der statischen Analyse Wird der Quellcode analysiert ohne ihn auszuführen
- Dynamische Analyse führt simulierte Angriffe gegen das laufende System aus





