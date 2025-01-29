
# 1 Aufgabestellung 

Eine Autowerkstatt m¨ochte die Abfertigung ihrer Auftr¨age komfortabel mit einer Software verwalten. Dazu k¨onnen Mitareiter:innen (Employee) im System Kunden und Kundinnen (Customer) anlegen und ihnen Fahrzeuge zuordnen. F¨ur neue Kunden und Kundinnen werden jeweils ein Name, eine Telefonnummer und eine Rechnungsadresse gespeichert und die Fahrzeuge werden mit Kennzeichen und Typ registriert.

Ein Auftrag kann entweder eine Inspektion, ein Reifenwechsel oder eine Reparatur sein. Einem neuen Auftrag wird ein Preis, ein Fahrzeug und automatisch ein Datumsstempel zugewiesen. Eine Reparatur erh¨alt außerdem eine genaue T¨atigkeitsbeschreibung. 
Ein Auftrag kann von Mitarbeitenden als beendet markiert werden. In diesem Fall wird der Kunde/die Kundin automatisch vom System benachrichtigt. Außerdem wird f¨ur den Auftrag vermerkt, welche/r Mitarbeiter:in ihn beendet hat.

Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden. Ein/e Administrator:in kann Mitarbeitende anlegen und entfernen.

# 2 Use-Case-Diagramm
a) Erstellt aus den Anforderungen ein Use-Case-Diagramm, in dem beteiligte Akteure/Aktuerinnen und Anwendungsfaelle enthalten sind.

b) Ueberlegt, ob Beziehungen zwischen Use-Cases die Beschreibung der Anforderungen sinnvoll erweitern wuerden.


---

![[01_02_UMl例子/image/Pasted image 20250123220558.png]]

- `<<include>>`: erzwungende gemacht  .  create customers 后必须 create car
- `<<extend>>`:  create car 后 不一定create order
- Create Order 这个 use case 是 oberklasse von 其他三个
- get informed 是一个 note, 用于表述 edge 的作用 


# 3 Klassendiagramme

a) Erstellt aus dem Text ein Klassendiagramm, das alle Informationen aus dem Anforderungstext enthaelt, sofern sie abbildbar sind.

b) An welchen Stellen hattet ihr verschiedene Moeglichkeiten zur Modellierung des Klassendiagramms? Diskutiert alternative Entwurfsentscheidungen.

c) Ueberlegt, wie Stereotype nach dem entity-boundary-control Pattern auf unser Beispiel angewendet werden koennten.

![[01_02_UMl例子/image/d21973bcee8377c44fdf92e79053b2d.jpg]]


![[01_02_UMl例子/image/cb7b526847e381a54e2803707180664.jpg]]


![[01_02_UMl例子/image/f9e35338e5ce49d1c6c984ddd931d92.jpg]]


---

整理前 

![[01_02_UMl例子/image/Pasted image 20250123223251.png]]


整理后 

![[01_02_UMl例子/image/Pasted image 20250123223544.png]]

![[01_02_UMl例子/image/Pasted image 20250123223559.png]]

![[01_02_UMl例子/image/Pasted image 20250123223616.png]]



--- 

解释 
``
- 箭头 便是 erben.  -> 所在一端 是 kindklasse,   没有箭头的一端是 eltern class 
- closeOrders: 在OCL中要用到, Employe:closedOrder 代表 从 employe 1 到 Order 
- create, belonges, has close 都是 beziehbung/aktion  , `->` 左边是 主语. 右边是 宾语 

## 3.1 entity, controller, boundary 

- `<<entity>>`: enthaekt nur daten/attribute/property
- `<<controller>>`: enthaelt nur method/fucntion
- `<<boundary>>`:  like Interface,   通过 boundary 来 controller aufrufen 
- empolyee 这个 class 分裂为 controller, boundary, entity 

## 3.2 notation of kardinalitaet

- Customer - Car : 1 代表 Ein customer zumindest ein car haben (1 car  kann maximal zu einer Customer zugeordnet ), 
- Employee - Car : * 代表 Ein Employee kann beliebig viel car betreuen 
- Customer -  Employee : 1 代表Ein Employer can beliebig Customer haben . X 代表   vom Costumer kann nicht ableiten, die zugehorigekeit von Employe .   Von Customer kommt nicht zu Employee .  就是没有 0...* 的关系 


## 3.3 schwarze und hohle Route


- hohle Route in Customer 1:  等价于  1 (notation of kardinalitaet)   
	- wenn customer weg, muss car nicht weg sein
	- wenn car weg, customer stehen noch da 
- schware Raute: wen customer weg, muss addesser noch weg 



# 4 Implementierung
Diskutiert wie eine m¨ogliche Implementierung eures Klassenmodells in Java aussehen k¨onnte.