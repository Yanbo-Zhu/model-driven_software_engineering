
# 1 use-ocl 的使用 

Wie auch auf dem letzten ¨Ubungsblatt k¨onnt ihr auch alle in dieser ¨Ubung entwickelten OCL Invarianten und Contracts mit dem Tool USE-OCL der Uni Bremen testen.

## 1.1 Invarianten mit USE-OCL zu evaluieren

Um Invarianten mit USE-OCL zu evaluieren, k¨onnt ihr den ”Create class invariant view” button verwenden (mit dem gelben Blitz). Dort seht ihr f¨ur jede Invariante, ob sie f¨ur euer aktuelles Objektdiagramm erf¨ullt ist. Durch Doppelklick auf eine Zeile k¨onnt ihr außerdem sehen, wie die einzelnen Teilbedingungen einer Invariante belegt sind. 


use .use datei 


![[01_02_UMl_OCL_例子/image/Pasted image 20250212102535.png]]


constrains 必须写入 .use 文件 才会对这个model自动生效 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112007.png]]


用 软件 载入 load .otl 文件 

然后 使用 class invariants 窗口去 检查 那些 invariants 北邮被满足  
![[01_02_UMl_OCL_例子/image/Pasted image 20250211112343.png]]



![[01_02_UMl_OCL_例子/image/Pasted image 20250212102548.png]]



Abbildung 1: Klassendiagramm der Autowerkstatt wie bei autowerkstatt.use
![[01_02_UMl_OCL_例子/image/Pasted image 20250211101526.png]]


## 1.2 Contracts mit USE-OCL zu evaluieren

Um Contracts mit USE-OCL zu evaluieren, k¨onnt ihr einzelne Operationen ¨uber die Kommandozeile ausf¨uhren. Hierzu werden in der Konsole die Operationen !openter und !opexit verwendet. Dazwischen muss die Operation simuliert werden!

Beispiel:
```
use> !openter m createCustomer(’bob’, ’strasse 7, 1000 berlin’, ’03012345678’)
precondition ‘pre3’ is true
precondition ‘pre4’ is true
use> !new Customer(’c3’)
use> !c3.name:=’bob’
use> !c3.address:=’strasse 7, 1000 berlin’
use> !c3.telefon:=’03012345678’
use> !c3.id:=4
use> !insert(m,c3) into creates
use> !opexit
postcondition ‘post3’ is true
```


打开 object diagramm 

打开 soil 文件 
![[01_02_UMl_OCL_例子/image/Pasted image 20250212103410.png]]


进而打开 soil 中 daten 对应的 object diagramm 
![[01_02_UMl_OCL_例子/image/Pasted image 20250212103456.png]]


在程序中 
会显示 出来 preconfition, postcondition 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114205.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250212111800.png]]

可以看 constaints 中那些违反了 
![[01_02_UMl_OCL_例子/image/Pasted image 20250212111920.png]]




在 .use 程序中 写下 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211113904.png]]


# 2 ocl 语法

implies  的意思
![[01_02_UMl_OCL_例子/image/Pasted image 20250212112128.png]]

a implies b: wenn a gilt,  dann d sollte auch geht 

# 3 Aufgabe 1: OCL Invarianten

## 3.1 知识储备 

![[01_02_UMl_OCL_例子/image/Pasted image 20250212102350.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250211111811.png]]

## 3.2 

==Invarianten sind Bedingungen, die zu jeder Zeit und f¨ur jedes Objekt einer Klasse gelten m¨ussen.== Formalisiert die folgenden Invarianten f¨ur das gegebene Klassendiagramm aus Abbildung 1. Welche Invarianten sind in dem Zustand der Abbildung 2 verletzt? Korrigiert den Zustand, damit alle Invarianten erf¨ullt sind.

a) Die ID jedes Mitarbeitenden muss gr¨oßer als 0 sein. 
b) Der Name jedes Mitarbeitenden darf kein leerer String sein. 
c) Die ID’s aller Mitarbeitenden m¨ussen eindeutig sein. 
d) Zu jedem Fahrzeug darf es h¨ochstens einen Inspektionsauftrag geben, der noch nicht abgeschlossen ist.
e) Der Besitzer/die Besitzerin eines Fahrzeugs hat dieses auch in der Menge seiner : Fahrzeuge.

f) Ein Auftrag muss immer vom Typ Reparatur, Inspektion oder Reifenwechsel sein. 
g) Jeder Auftrag, der beendet ist, muss einem Mitarbeitenden zugewiesen sein, der ihn beendet hat.
h) Zu jedem Fahrzeug gibt es h¨ochstens einen offenen Auftrag von jedem Typ. 
i) Alle Auftr¨age f¨ur Fahrzeuge vom Typ “jaguar” sollen mehr als 1000 Euro kosten. 
j) F¨ur jede beendete Inspektion existiert mindestens ein Reparaturauftrag f¨ur das gleiche Fahrzeug.

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112517.png]]



---


a) Die ID jedes Mitarbeitenden muss gr¨oßer als 0 sein. 
```
constraints
context EmployeeData inv: self.id > 0
```

在这个题设中 EmployeeData ist meine Klasse 



![[01_02_UMl_OCL_例子/image/Pasted image 20250211112115.png]]

在 .use 文件中被保存后, 在 app 中出现 对应的 invariants
![[01_02_UMl_OCL_例子/image/Pasted image 20250212103226.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112408.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112414.png]]

显示 false 信息, 因为 一个    EmployeeData 等于 0 了 


b) Der Name jedes Mitarbeitenden darf kein leerer String sein. 
```
constraints

context EmployeeData inv: self.user <> '' and self.user <> null
```

![[01_02_UMl_OCL_例子/image/Pasted image 20250212103724.png]]

不写 self 也可以 


Risiko , wenn ohne self 
不写  o:Order  会 变得 intuitiv 
![[01_02_UMl_OCL_例子/image/Pasted image 20250212104110.png]]

c) Die ID’s aller Mitarbeitenden m¨ussen eindeutig sein. 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112654.png]]

```
constraints
context EmployeeData inv C: EmployeeData.allInstances->select(e:EmployeeData | e.id = self.id) -> size() = 1

```


d) Zu jedem Fahrzeug darf es h¨ochstens einen Inspektionsauftrag geben, der noch nicht abgeschlossen ist.

```
constraints
context Car inv D: self.order->select(o:Order | not o.closed and o.oclIsTypeOf(Inspection)) -> size() <= 1

self 就是一个 Car

```


e) Der Besitzer/die Besitzerin eines Fahrzeugs hat dieses auch in der Menge seiner Fahrzeuge.

```
constraints
context Car inv E: self.customer.car -> includes(self)

self 就是一个 Car

```


f) Ein Auftrag muss immer vom Typ Reparatur, Inspektion oder Reifenwechsel sein. 

```
constraints
context Car inv E: self.oclTypeOf(Inspection) or self.oclTypeOf(Inspection) or self.oclTypeOf(TierChange) 

self 就是一个 Car

```


g) Jeder Auftrag, der beendet ist, muss einem Mitarbeitenden zugewiesen sein, der ihn beendet hat.

![[01_02_UMl_OCL_例子/image/Pasted image 20250212110111.png]]

wenn closed war  implies closing nicht null

![[01_02_UMl_OCL_例子/image/Pasted image 20250212110345.png]]

==什么用 self.closing, 因为 有 closing 这个 rollenbezeichnung  存在. 这时候 用 self.EmployeeData 会报错了 . 如果没有rollenbezeichnung in Kante. 可以直接用 self.employeeData ==

![[01_02_UMl_OCL_例子/image/Pasted image 20250212110450.png]]

```
constraints
context Car inv G: 


```



h) Zu jedem Fahrzeug gibt es h¨ochstens einen offenen Auftrag von jedem Typ. 

`let <temporary variable > in   xx Bedingung `
![[01_02_UMl_OCL_例子/image/Pasted image 20250211112841.png]]

```
constraints
context Car inv H1: 
self.order->select(o:Order | o.oclIsTypeOf(Repair)) -> size(1) <=1 and
self.order->select(o:Order | o.oclIsTypeOf(Inpsektion)) -> size(1) <=1 and
self.order->select(o:Order | o.oclIsTypeOf(TireChange)) -> size(1) <=1



context Car inv H2: 
self.order->select(o:Order | not o.closed) -> select(o:Order | o.oclIsTypeOf(Repair)) -> size(1) <=1 and
self.order->select(o:Order | not o.closed) -> select(o:Order | o.oclIsTypeOf(Inpsektion)) -> size(1) <=1 and
self.order->select(o:Order | not o.closed) -> select(o:Order | o.oclIsTypeOf(TireChange)) -> size(1) <=1


context Car inv H3: let
	auftraege = self.order->select(o:Order | not o.closed)
in 
	auftraege-> select(o:Order | o.oclIsTypeOf(Repair)) -> size(1) <=1 and
	auftraege -> select(o:Order | o.oclIsTypeOf(Inpsektion)) -> size(1) <=1 and
	auftraege -> select(o:Order | o.oclIsTypeOf(TireChange)) -> size(1) <=1
```




i) Alle Auftr¨age f¨ur Fahrzeuge vom Typ “jaguar” sollen mehr als 1000 Euro kosten. 

```
context Order inv I1: self.car.typ = 'jaguar' implies self.price > 1000
context Car inv I2: self.typ = 'jaguar' implies self.order->forAll(o:Order  | o.price >1000 )


```


j) F¨ur jede beendete Inspektion existiert mindestens ein Reparaturauftrag f¨ur das gleiche Fahrzeug.
![[01_02_UMl_OCL_例子/image/Pasted image 20250212111537.png]]

```
context Order inv J: self.closed implies
```
# 4 Aufgabe 2: OCL Contracts

Abbildung 2: Beispiel-Objektdiagramm wie bei autowerkstatt.soil
![[01_02_UMl_OCL_例子/image/Pasted image 20250211101657.png]]

## 4.1 知识储备 

![[01_02_UMl_OCL_例子/image/Pasted image 20250212112245.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250212112347.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250212112431.png]]


wpk.product@pre  contains  p1 und p2
p1 und p2 现在的 storedQuantity 是 18 和 19 


![[01_02_UMl_OCL_例子/image/Pasted image 20250212112948.png]]



## 4.2 ##

==Vor- und Nachbedingungen von Operationen beschreiben den Systemzustand und die Eingabe- bzw. Ausgabeparameter. Zusammen mit Invarianten l¨asst sich dadurch formal feststellen, ob z.B. eine bestimmte Abfolge von Operationen m¨oglich ist. ==

Formalisiert die folgenden Vor- und Nachbedingungen. Benutzt hierzu das gegebene Klassenmodell. Wie k¨onnte man sicherstellen, dass eine Implementierung die Contracts und Invarianten erf¨ullt?

Hinweis
Ihr k¨onnt davon ausgehen, dass kein Eingabeargument mit null belegt ist.

### 4.2.1 a

a) Die Operation createCustomer erh¨alt die Daten Name, Adresse und Telefonnummer des Kunden/der Kundin. Es d¨urfen keine leeren Daten gespeichert werden und der Kunde/die Kundin darf auch nicht mehrfach existieren. Außerdem muss eine eindeutige ID generiert werden.

PRE: Eingabeparameter sol len nicht leer sein
PRE: Customer soll noch nicht existieren
POST: Generierte ID soll eindeutig sei

```
context Employee :: createCustomer(cName: String, adr: String, tel: String)
pre:
	cName <> '' and adr <> '' and tel <> '' and
	not Customer.alllnstances() -> exists (k:Customer | k.name = cName)

post:
	self.customer -> exists (k:Customer | k.name = cName and k.address = adr and
	k. telefon = tel and k.oclIsNew() and
	Customer.allInstances() -> select (k1:Customer | k1.id = k.id) -> size() = 1 )
```


![[01_02_UMl_OCL_例子/image/Pasted image 20250211113142.png]]

pruefen id 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211113628.png]]


k: Customer 的作用就是  宣称一下 k 的 type 是 customer 类型的 


在 .use 程序中 写下 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211113904.png]]


测试 : 去创造一个 createCustomer 看看 这个 contracts 有没有生效 

![[01_02_UMl_OCL_例子/image/Pasted image 20250212114225.png]]

```
use> !openter m createCustomer(’bob’, ’strasse 7, 1000 berlin’, ’03012345678’)
precondition ‘pre3’ is true
precondition ‘pre4’ is true
use> !new Customer(’c3’)
use> !c3.name:=’bob’
use> !c3.address:=’strasse 7, 1000 berlin’
use> !c3.telefon:=’03012345678’
use> !c3.id:=4
use> !insert(m,c3) into creates
use> !opexit
postcondition ‘post3’ is true
```

1 测试  precondition 
使用 !openter 命令 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114337.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250212114506.png]]


2  去创造 一个 customner 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114438.png]]

3测试 post condition 
使用 !opexit
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114501.png]]


### 4.2.2 b

b) Die Operation addCar in der Klasse Customer wird zus¨atzlich ben¨otigt. Auch sie soll spezifiziert werden und sie erh¨alt ein Kennzeichen als String. Das Fahrzeug mit dem ¨ubergebenen Kennzeichen wird neu erstellt. Ein anderes Fahrzeug mit dem gleichen Kennzeichen darf vorher nicht existieren.


### 4.2.3 c


c) Die Operation hasCar in der Klasse Customer soll pr¨ufen ob ein Fahrzeug mit einem bestimmten Kennzeichen existiert und das Ergebnis als Bool zur¨uckgeben.

@pre  的意义是 找到之前 状态 这个 attribue 的值 
