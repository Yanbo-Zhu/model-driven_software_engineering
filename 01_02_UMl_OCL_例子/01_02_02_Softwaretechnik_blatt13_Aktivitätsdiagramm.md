


Es gilt wieder die aus den vorhergehenden ¨Ubungsbl¨attern bekannte textuelle Anforderungsspezifikation:

Eine Autowerkstatt m¨ochte die Abfertigung ihrer Auftr¨age komfortabel mit einer Software verwalten. Dazu k¨onnen Mitareiter:innen (Employee) im System Kunden und Kundinnen (Customer) anlegen und ihnen Fahrzeuge zuordnen. F¨ur neue Kunden und Kundinnen werden jeweils ein Name, eine Telefonnummer und eine Rechnungsadresse gespeichert und
die Fahrzeuge werden mit Kennzeichen und Typ registriert.

Ein Auftrag kann entweder eine Inspektion, ein Reifenwechsel oder eine Reparatur sein. Einem neuen Auftrag wird ein Preis, ein Fahrzeug und automatisch ein Datumsstempel zugewiesen. Eine Reparatur erh¨alt außerdem eine genaue T¨atigkeitsbeschreibung. Ein Auftrag kann von Mitarbeitenden als beendet markiert werden. In diesem Fall wird der Kunde/ die Kundin automatisch vom System benachrichtigt. Außerdem wird f¨ur den Auftrag vermerkt, welche/r Mitarbeiter:in ihn beendet hat. 

Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden. Ein/e Administrator:in kann Mitarbeitende anlegen und entfernen.


# 1 Beispiel von aktivitatsdiagramm 和这个作业无关 

![[01_02_UMl_OCL_例子/image/Pasted image 20250129101858.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250129102055.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250129102213.png]]
Entscheidlung 的连个 Guard 间 没有 schnittemenge 




![[01_02_UMl_OCL_例子/image/Pasted image 20250129102344.png]]



![[01_02_UMl_OCL_例子/image/Pasted image 20250129102433.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129102452.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129102613.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129102625.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129102646.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250129102734.png]]






# 2 Workflow-Modellierung


## 2.1 a
a) Erstellt ein Aktivit¨atsdiagramm zur Modellierung des Arbeitsablaufes (Workflows) : von Mitarbeiter:innen, wenn ein Kunde/eine Kundin die Autowerkstatt betritt und sein Fahrzeug zur Reparatur abgibt. Dabei sollen fehlende Kunden und Kundinnenund Fahrzeugdaten im Bedarfsfall direkt mit angelegt werden.


- 最左边的黑色的点 是 startkonte
- check customer ist schon im system  oder noch nicht
	- 使用 Entscheidung 
	- Graud 是  unknown customer or known customer 
	- 加入 failed Case 的控制 
- 两种 customer zusammenfuegen
- car  是否已经在系统了,  如果还没在系统里 就创造这个 car 
	- 然后两个 aktion zusammenfuegen
- 最后是 endknote
![[01_02_UMl_OCL_例子/image/Pasted image 20250129105322.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250129104517.png]]


## 2.2 b
b) Wie m¨usste das Aktivit¨atsdiagramm erweitert werden, um die Abarbeitung beliebig : vieler Kunden und Kundinnen an einem Arbeitstag zu modellieren?

就是 从 endknote 拓展出来一条线 

![[01_02_UMl_OCL_例子/image/Pasted image 20250129105020.png]]


## 2.3 c
c) Welche Aktionen k¨onnten sinnvoll als Aktivit¨atsaufrufe in eurem Aktivit¨atsdiagramm : modelliert werden? Welchem Zweck dient eine detailliertere Modellierung?




# 3 Kontrollfluss-Modellierung


## 3.1 a
a) Erstellt ein Aktivit¨atsdiagramm zur Modellierung des Kontrollflusses innerhalb einer : Aktion, die in Aufgabe 1 als Aktivit¨atsaufruf modelliert wird. Ggf. k¨onnen Designentscheidungen getroffen werden, z.B. wie man nach Kunden/Kundinnen, Fahrzeugen oder Auftr¨agen suchen kann und welche m¨ogliche Fehler ber¨ucksichtigt werden. Haben beide Modelle ein gleiches Detaillierungsgrad? 

针对 CreateCar 这个 action 画一个 Kontrollflusses  Aktivitaettsdiagramm 

![[01_02_UMl_OCL_例子/image/Pasted image 20250129105909.png]]

müsste es nicht einen rechteckigen kasten geben für car data?  also der mit den eckigen Kanten
可以加入 

![[01_02_UMl_OCL_例子/image/Pasted image 20250129110108.png]]




## 3.2 b 

b) Erstellt ein Aktivit¨atsdiagramm zur Modellierung des Kontrollflusses innerhalb der :  Operation auftragBeenden. Dabei sollen folgende Zusatzanforderungen ber¨ucksichtigt werden:
• Es soll eine Rechnung f¨ur die gegebene Auftrags-ID erstellt und verschickt werden.
• Beim Erstellen der Rechnung sollen die einzelnen Posten genau eingegeben werden.
Dabei soll die Arbeitszeit in Minuten eingegeben werden oder die Kosten f¨ur ein Ersatzteil bei der Warenverwaltung angefragt werden.
• Die Rechnung soll jeweils um 0 Uhr an den Kunden/die Kundin und an die Buchhaltung verschickt werden.


- Finde Order noch nicht gefunden
	- create ein Entscheidung 
	- wenn nicht gefunden, ganz aktivitat beendet 
	- wen gefunde werden kann, rechnung erzeugen 
- Create Invoice 后 使用 splitting (kann parallel gemacht), 或者 Entscheidung(Raute) 
	- 自己决定用那个自己决定 
- A 是 connector , transition von 上边的a 出来, 然后 从 底下的 a 进去 
- ZeitUhr 通过一个特殊符号添加 
- 用splitting . 就是说 send invoice 给不同人  . Abhangigen ausgefuhrt 
- - kontrollfluss-endknote ,  Aktivitätsendknoten
	- ![[01_02_UMl_OCL_例子/image/Pasted image 20250129112139.png]]
		- Kontrollfluss endknote  endet nur ein kontrollfluss.  Er endet die gesamte ganze Aktivität nicht 
		- 其中一个 Kontrollfluss 结束的时候,   其他的 splitted parallel kontrolfluss 可以 继续被执行
	- ![[01_02_UMl_OCL_例子/image/Pasted image 20250129112215.png]]
		- Aktivitätsendknote. Er endet die gesamte ganze Aktivität 
		- 其中一个 Kontrollfluss 结束的时候,   其他的 splitted parallel kontrolfluss 不可以 继续被执行. 因为 整个 Aktivität 都被结束了 



![[01_02_UMl_OCL_例子/image/Pasted image 20250129111441.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129111833.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250129112100.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250129112150.png]]



![[01_02_UMl_OCL_例子/image/Pasted image 20250129112452.png]]