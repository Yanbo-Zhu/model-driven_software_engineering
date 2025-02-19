

# 1 Aufgabe 1: Terminierung


## 1.1 trinumber(int n):


![](image/Pasted%20image%2020250115102534.png)


任务是 t auswahlen, sodass 1 und 2 Bedingung erfullt werden 
![](image/Pasted%20image%2020250115102831.png)


---
对于 1. Bedingung 的证明过程 

选 t =  n -i  = m 

![](image/Pasted%20image%2020250115103056.png)



---

对于 2. Bedingung 的证明过程 

![](image/Pasted%20image%2020250115103420.png)

## 1.2 rest


![](image/Pasted%20image%2020250115103607.png)

 
根据regel 7 (Terminierung) 改写 
![](image/Pasted%20image%2020250115103733.png)

![](image/Pasted%20image%2020250115103650.png)


然后任务是 找到 t


---

Bedingung 1 
![](image/Pasted%20image%2020250115103813.png)

对应 
![](image/Pasted%20image%2020250115103821.png)



![](image/Pasted%20image%2020250115103834.png)


可以得到 r-y immer >= 0 



----

Bedingung 2 

![](image/Pasted%20image%2020250115103901.png)

对应 
![](image/Pasted%20image%2020250115103955.png)

![](image/Pasted%20image%2020250115104144.png)

![](image/Pasted%20image%2020250115104155.png)

![](image/Pasted%20image%2020250115104247.png)

die Implikation r-y = m gilt, nur y = 0 


![](image/Pasted%20image%2020250115104459.png)




# 2 Aufgabe 2: Nicht-funktionale Anforderungen


Die folgende Anforderungsbeschreibung wurde euch f¨ur die Entwicklung eines Programms : geliefert.

Eine Autowerkstatt m¨ochte die Abfertigung ihrer Auftr¨age komfortabel mit einer Software verwalten. Dazu k¨onnen Miterarbeitende im System Kunden und Kundinnen anlegen und ihnen Fahrzeuge zuordnen. F¨ur neue Kunden und Kundinnen werden Name, Telefonnummer und Rechnungsadresse gespeichert und die Fahrzeuge werden mit Kennzeichen und Typ registriert.

Ein Auftrag kann entweder eine Inspektion, ein Reifenwechsel oder eine Reparatur sein. Einem neuen Auftrag wird ein Preis, ein Fahrzeug und automatisch ein Datumsstempel zugewiesen. Eine Reparatur erh¨alt außerdem eine genaue T¨atigkeitsbeschreibung. Ein Auftrag kann von Mitarbeitenden als beendet markiert werden. In diesem Fall wird der Kunde bzw. die Kundin automatisch vom System benachrichtigt. Außerdem wird f¨ur den Auftrag vermerkt, welche/r Mitarbeiter:in ihn beendet hat.

Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden. Ein/e Administrator:in kann Mitarbeitende anlegen und entfernen.

![](image/Pasted%20image%2020250115104947.png)

![](image/Pasted%20image%2020250115105621.png)

a) Was ist der Unterschied zwischen funktionalen und nicht-funktionalen Eigenschaften?

b) ¨Uberlegt, welche nicht-funktionalen Anforderungen aus dem Text oben hervorgehen.
- Sicherheit: Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden.
- 


c) Welche Eigenschaften sind f¨ur dieses System wahrscheinlich auch noch wichtig?
- Dependability requirements: Autowerkstatt wird ja jetzt wahrscheinlich durch erstellen von software komplett umgestellt. Software sollte zuverlassig sein, 
- Organizational requirements
- Legislative Requirements: datenschultz von Kunden Daten 
- Usability requirements: easy to use 

# 3 Aufgabe 3: Requirements Engineering

a) Ermittelt die im Text von Aufgabe 2 enthaltenen Use-Cases.
Dazu k¨onnen Miterarbeitende im System Kunden und Kundinnen anlegen und ihnen Fahrzeuge zuordnen.
- Kunden anlgen 
- Fahrzeug Kunden zuordenen
- Fahrezeug registrien 

Ein Auftrag kann entweder eine Inspektion, ein Reifenwechsel oder eine Reparatur sein.
- Auftrag anlegen und andern

Ein/e Administrator:in kann Mitarbeitende anlegen und entfernen.
 - Miterbeiter anlegen und entfernen 

Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden.
- Als Mitarbeitende anmelden


b) Diskutiert, welche Anforderungen Kunden und Kundinnen wahrscheinlich zus¨atzlich an das System haben werden bzw. welche der Beschreibungen unklar sind.

Fehlende user cases: 
-  Kunden Uber Stand benachrichtigen 
- Auftragshistorie
- Rechnnung erstellen 
- Passwort andern 
- Auftrag stornieren 


c) Erstellt aus zwei Use-Cases User Stories. Sind Vorteile der User Stories ersichtlich?

![](image/Pasted%20image%2020250115110740.png)


![](image/Pasted%20image%2020250115110747.png)

- Als Mitarbeiter mochte ich `Kund*innen` anlegen koennen
- Als Mitarbeiter moechte ich Rechnung erstellen koennen 



# 4 Aufgabe 4: Strukturierte Anforderungsspezifikation

Modelliert einige der in Aufgabe 3 beschriebenen Anwendungsfaelle (Use-Cases) in Form : von strukturierten Spezifikationen. Uberlegt euch sinnvolle Attribute zur Strukturierung.

- Use case : Auftrag anlegen 
- Funktion: Auftrag anlgen
- Beschriebung: was muss passieren, 
    - Ein neuer Auftrag wird im System erstellt 
    - Der Auftrag hat einen validen Typ. 
    - Dem Auftrag wird eine Kunde zugeordnet 
    - Dem Auftrag wird ein Fahrzeug zugeordnet 
    - Dem Auftrag wird ein Preis zugewiesen
- Input 
    - Auftragstyp, Kunde* in , Fahrzeug, Preis, ggf. Beschriebung(Defektdetail und repoaratur)
- Output
    - Fehelr/Erfolgsmeldung
- Aktion 
    - Auftrag anlegen
- Vorbedingung
    - Kunden muss exisitieren in Sytem 
    - System muss exisitieren 
    - Das Fahrzeug muss zu Kunden gehoeren 
    - Preis > 0 
- Nachbedingung
    - Auftrag existiert im System
    - Auftrag ist dem Fahrzeug zugewiesen









