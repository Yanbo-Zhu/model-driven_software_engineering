
Wie auch auf dem letzten ¨Ubungsblatt k¨onnt ihr auch alle in dieser ¨Ubung entwickelten OCL Invarianten und Contracts mit dem Tool USE-OCL der Uni Bremen testen.

Um Invarianten mit USE-OCL zu evaluieren, k¨onnt ihr den ”Create class invariant view” button verwenden (mit dem gelben Blitz). Dort seht ihr f¨ur jede Invariante, ob sie f¨ur euer aktuelles Objektdiagramm erf¨ullt ist. Durch Doppelklick auf eine Zeile k¨onnt ihr außerdem sehen, wie die einzelnen Teilbedingungen einer Invariante belegt sind.

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



Abbildung 1: Klassendiagramm der Autowerkstatt wie bei autowerkstatt.use
![[01_02_UMl_OCL_例子/image/Pasted image 20250211101526.png]]

# 1 Aufgabe 1: OCL Invarianten

## 1.1 知识储备 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211111811.png]]

constrains 必须嵌入 .use 文件 才会对这个model自动生效 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112007.png]]


用 软件 载入 load otl 文件 

然后 使用 class invariants 窗口去 检查 那些 invariants 北邮被满足  
![[01_02_UMl_OCL_例子/image/Pasted image 20250211112343.png]]
## 1.2 

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
`context EmployeeData inv: self.id > 0 `

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112115.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112408.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112414.png]]

显示 false 信息, 因为 一个    EmployeeData 等于 0 了 


b) Der Name jedes Mitarbeitenden darf kein leerer String sein. 
`context EmployeeData inv: user <> '' and user <> null  `

c) Die ID’s aller Mitarbeitenden m¨ussen eindeutig sein. 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211112654.png]]


d) Zu jedem Fahrzeug darf es h¨ochstens einen Inspektionsauftrag geben, der noch nicht abgeschlossen ist.
e) Der Besitzer/die Besitzerin eines Fahrzeugs hat dieses auch in der Menge seiner : Fahrzeuge.

f) Ein Auftrag muss immer vom Typ Reparatur, Inspektion oder Reifenwechsel sein. 
g) Jeder Auftrag, der beendet ist, muss einem Mitarbeitenden zugewiesen sein, der ihn beendet hat.

h) Zu jedem Fahrzeug gibt es h¨ochstens einen offenen Auftrag von jedem Typ. 

`let <temporary variable > in   xx Bedingung `
![[01_02_UMl_OCL_例子/image/Pasted image 20250211112841.png]]



i) Alle Auftr¨age f¨ur Fahrzeuge vom Typ “jaguar” sollen mehr als 1000 Euro kosten. 
j) F¨ur jede beendete Inspektion existiert mindestens ein Reparaturauftrag f¨ur das gleiche Fahrzeug.

# 2 Aufgabe 2: OCL Contracts

Abbildung 2: Beispiel-Objektdiagramm wie bei autowerkstatt.soil
![[01_02_UMl_OCL_例子/image/Pasted image 20250211101657.png]]

## 2.1 使用

在程序中 
会显示 出来 preconfition, postcondition 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114205.png]]

## 2.2 ##

==Vor- und Nachbedingungen von Operationen beschreiben den Systemzustand und die Eingabe- bzw. Ausgabeparameter. Zusammen mit Invarianten l¨asst sich dadurch formal feststellen, ob z.B. eine bestimmte Abfolge von Operationen m¨oglich ist. ==

Formalisiert die folgenden Vor- und Nachbedingungen. Benutzt hierzu das gegebene Klassenmodell. Wie k¨onnte man sicherstellen, dass eine Implementierung die Contracts und Invarianten erf¨ullt?

Hinweis
Ihr k¨onnt davon ausgehen, dass kein Eingabeargument mit null belegt ist.

a) Die Operation createCustomer erh¨alt die Daten Name, Adresse und Telefonnummer des Kunden/der Kundin. Es d¨urfen keine leeren Daten gespeichert werden und der Kunde/die Kundin darf auch nicht mehrfach existieren. Außerdem muss eine eindeutige ID generiert werden.

![[01_02_UMl_OCL_例子/image/Pasted image 20250211113142.png]]

pruefen id 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211113628.png]]


k: Customer 的作用就是  宣称一下 k 的 type 是 customer 类型的 


在 .use 程序中 写下 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211113904.png]]


测试 : 去创造一个 createCustomer 看看 这个 contracts 有没有生效 

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

2  去创造 一个 customner 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114438.png]]

3测试 post condition 
使用 !opexit
![[01_02_UMl_OCL_例子/image/Pasted image 20250211114501.png]]


b) Die Operation addCar in der Klasse Customer wird zus¨atzlich ben¨otigt. Auch sie soll spezifiziert werden und sie erh¨alt ein Kennzeichen als String. Das Fahrzeug mit dem ¨ubergebenen Kennzeichen wird neu erstellt. Ein anderes Fahrzeug mit dem gleichen Kennzeichen darf vorher nicht existieren.




c) Die Operation hasCar in der Klasse Customer soll pr¨ufen ob ein Fahrzeug mit einem bestimmten Kennzeichen existiert und das Ergebnis als Bool zur¨uckgeben.

@pre  的意义是 找到之前 状态 这个 attribue 的值 
