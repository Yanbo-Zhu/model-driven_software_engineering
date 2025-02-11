

# 1 USE OCL application 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205102340.png]]

开一个 ocl 文件 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205102512.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205102543.png]]


help 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205102944.png]]

# 2 UML 

layout of used class Diagramm 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205102708.png]]


# 3 .soil 

用于创建 真实的object

![[01_02_UMl_OCL_例子/image/Pasted image 20250205103012.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205103208.png]]


---

load layout , 使用 另一个文件 .olt 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205103322.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205103247.png]]



# 4 aufgabe 1

## 4.1 a 

a) Wodurch unterscheidet sich das Objektdiagramm vom Klassendiagramm? Warum gibt es keine Multiplizit¨aten?

通过 menu bar 中 有个 check diagram 的功能 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205103442.png]]


# 5 Aufgabe 2: OCL Werte

Verwendet OCL um folgende Werte des Systemzustands zu erhalten.
a) Die Id des Kunden/der Kundin c1 :
b) Das Kennzeichen des Fahrzeugs f1 : 
c) Der Name des Besitzers/der Besitzerin des Fahrzeugs f1 :
d) Der Mitarbeiter/die Mitarbeiterin, der/die den Auftrag r2 beendet hat :
e) Diejenige Reparatur aus re3 und re4 mit dem h¨oheren Preis :
f) Den ersten Buchstaben des Kennzeichens von Fahrzeug f1

![[01_02_UMl_OCL_例子/image/Pasted image 20250205103819.png]]


查询某个 class 的attribute 的值 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205103956.png]]



![[01_02_UMl_OCL_例子/image/Pasted image 20250205104004.png]]


通过这个 可以查询值 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205104108.png]]


----

b) Das Kennzeichen des Fahrzeugs f1 : 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205104258.png]]


---


(c) Der Name des Besitzers/der Besitzerin des Fahrzeugs f1

使用 azoziation 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205104813.png]]


----

(d)
Der Mitarbeiter/die Mitarbeiterin, der/die den Auftrag r2 beendet hat

benutzt immer  raumBezeichnung 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205104529.png]]




![[01_02_UMl_OCL_例子/image/Pasted image 20250205104750.png]]


----

e) Diejenige Reparatur aus re3 und re4 mit dem h¨oheren Preis

![[01_02_UMl_OCL_例子/image/Pasted image 20250205105004.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205105039.png]]

?? 会得到更多信息 
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105110.png]]


----

(f) Den ersten Buchstaben des Kennzeichens von Fahrzeug f1


注意加上 ? 
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105311.png]]



因为 f1.licensePlate 是 string, 不是 collection 没法用 first()
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105433.png]]



# 6 Aufgabe 


Logische Ausdr¨ucke sind statisch verifizierbar und werden auf verschiedene Weise eingesetzt.
Formalisiert die folgenden logischen Aussagen f¨ur das gegebene Klassenmodell.

a) Sind die Namen der Kund:innen c1 und c2 gleich? :
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105535.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205105542.png]]

----

b) Gehoert das Fahrzeug f1 dem Kunden/der Kundin c1? : 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205105621.png]]


---

c) Hat das Kennzeichen des Fahrzeugs f1 einen nicht-leeren Wert? :

<> -> nicht 的意思
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105659.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205105745.png]]

?f1.licensePlate.size()
![[01_02_UMl_OCL_例子/image/Pasted image 20250205105834.png]]


---


d) Ist der Auftrag i1 eine Inspektion? :

 Ob ein Objekt Typ von einer Klasse ist,  wenn diese Klasse von einer anderen Klasse erbt sollte ich immer alles teilbar benutzen.


![[01_02_UMl_OCL_例子/image/Pasted image 20250205110112.png]]


---


e) Ist das Objekt i1 ein Auftrag?

![[01_02_UMl_OCL_例子/image/Pasted image 20250205110544.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205110531.png]]

oclIsTypeOf  使用来检验
direkt zugehorigkeit 
这个 class 就是 这个 type 的, 不是继承关系 


oclIskindof
这个 class 是否继承了某个 class 



# 7 Aufgabe 4: OCL Collections

OCL Collection: set, sequenz, Bag

![[01_02_UMl_OCL_例子/image/Pasted image 20250205110744.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205110911.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205110938.png]]



Definiert die folgenden Mengen:
a) Alle Fahrzeuge im System :
![[01_02_UMl_OCL_例子/image/Pasted image 20250205111117.png]]

bekommen a set wieder , da in car object, jedem object einzigartig , deshalb bekommen man eine Set 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211102808.png]]


----

b) Die Fahrzeuge von c1 :

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111143.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205111228.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205111252.png]]


---

c) Die Fahrzeuge von c1 und das Fahrzeug f4 :

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111451.png]]

---

d) Alle Fahrzeuge von c1 und c2 :

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111551.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205111613.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111629.png]]

Wie könnte man die Fahrzeuge von c1 und c2 und c3 bekommen? 
c1.car -> union(c2.car) -> union(c2.car) 

---


e) Die Fahrzeuge die gleichzeitig c1 und c2 geh¨oren. : 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111743.png]]



---


f) Die Typen aller Fahrzeuge des Customers c1. :

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111829.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111931.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205112024.png]]


---

g) Die Preise aller Auftr¨age im System. : 

collect gibe Bag zuruck 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112118.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250211105130.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211105134.png]]

---

h) Die Anzahl der Auftr¨age f¨ur das Fahrzeug f4. :

![[01_02_UMl_OCL_例子/image/Pasted image 20250211105312.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112232.png]]

würde s auch mit .size() funktionieren  oder muss dieses -> size() sein
Ne, ich glaube das wurde auf alle Instanzen in der Collection size anwenden
> collection 不能用 .size  , `.<funtionName>` 只能对 Objekt 使用 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112521.png]]

---

i) Die Anzahl der Kund:innen mit der ID 12

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112317.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211105334.png]]



Was ist Unterschied zwischen size() und count()?

size. Anzahl von elemente

count(12): Anzahl von elemente, die geleich zu werte x ist.   Count sollte auch bei Set funktionieren 

size() gibt Menge einer Collection an 
count(X) gibt an wie oft etwas X in der Collection vorkommt


## 7.1 一些说明

1

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111117.png]]

bekommen a set wieder , da in car object, jedem object einzigartig , deshalb bekommen man eine Set zuruck 

2 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205111316.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250211103421.png]]

2.1
c1.car  得到  一个 set 因为  
- car 那边是 小星星 . 就是说 一个 set 里面只有一个 object 也是set datentyp 
- 因为 每个 car 都是一个独立的 不同的 object 
- hier ist ein Stern bei Car auf de Linie. Also ein customer kann beliebig viele cars haben. Heißt c1 also customer 1 zu car kann ein set an cars haben und selbst wenn es nur ein car gibt, wird das dann als set ausgegeben
![[01_02_UMl_OCL_例子/image/Pasted image 20250211104203.png]]


2.2

![[01_02_UMl_OCL_例子/image/Pasted image 20250211172429.png]]

Wenn die Multiplizität 1 ist, dann kommt aber kein Set raus? 
YEs. sondern eine Object bekommen 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211103342.png]]
![[01_02_UMl_OCL_例子/image/Pasted image 20250211103332.png]]


2.4 
下面 0...1   Oder.EmployeeData 也不会得到一个 set, 只是得到一个object 或者 null 
liefert dann Datentyp EmployeeData oder OclAny?  
![[01_02_UMl_OCL_例子/image/Pasted image 20250211103621.png]]

2.5 
0...5 的话 可能会到 set, 或者 object m 或者 null 


---

3 set und bag 区别 
![[01_02_UMl_OCL_例子/image/Pasted image 20250211103800.png]]

c1.car.typ  kriegen  eine Bag zuruck liek {'A', 'B', 'C, }

Set: Element in Set sind nicht sortiert, nur jeden Object .  das wäre nicht möglich wenn ich zweimal den gleichen String in einem Set
Bag: dasselbe wert von attribute kann merhmals enthalten in Bag 


----
4 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211104854.png]]

-> Pfeil-Opeartor:    ausgefuhrt auf eine Set 

union: 两边都是 set
intersection: 两边 都是 set , 提取到 schnitte menge 


# 8 Aufgabe 5: OCL Aussagen

![[01_02_UMl_OCL_例子/image/Pasted image 20250211105745.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211105810.png]]

¨Uberpr¨uft mithilfe von OCL ob folgende Aussagen ¨uber den Systemzustand stimmen. Beginnt mit der Navigation immer bei der Mitarbeiter-Controller Instanz m.

## 8.1 基础知识


![[01_02_UMl_OCL_例子/image/Pasted image 20250211110015.png]]

m.allOrders 返回的是  一个 set , 不是 单独一个object,
因为  kante 上 一个 empoyee 对应多个 *  Order 
## 8.2 ##

a) Alle Auftr¨age sind beendet. 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112807.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205112815.png]]

`a:Order`  gibt es an, dass a immer in Type `Order`

 forAll 采取 a.closed 的 object.   但是我们要先假定  A ist in type Order 

![[01_02_UMl_OCL_例子/image/Pasted image 20250211110611.png]]


---

b) Mindestens ein Auftrag ist noch nicht beendet. 

![[01_02_UMl_OCL_例子/image/Pasted image 20250205112951.png]]

或者用 select 也行 
![[01_02_UMl_OCL_例子/image/Pasted image 20250205113047.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205113058.png]]


其实不用加()
![[01_02_UMl_OCL_例子/image/Pasted image 20250205113152.png]]



c) Alle Auftr¨age, die keine Inspektionen sind, sind beendet. 


![[01_02_UMl_OCL_例子/image/Pasted image 20250205113352.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205113437.png]]





d) Alle beendeten Auftra¨ge haben einenMitarbeiter/eineMitarbeiterin als Beender/Beende:rin vermerkt.

![[01_02_UMl_OCL_例子/image/Pasted image 20250205113646.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205113656.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205113708.png]]


e) Alle Auftr¨age die einen Mitarbeiter/eine Mitarbeiterin als Beender vermerkt haben sind auch beendet.

![[01_02_UMl_OCL_例子/image/Pasted image 20250205113857.png]]

答案是 false : Boolean 


f) Die IDs der Fahrzeuge sind eindeutig.

![[01_02_UMl_OCL_例子/image/Pasted image 20250205114850.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205114311.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205114557.png]]


![[01_02_UMl_OCL_例子/image/Pasted image 20250205114905.png]]

# 9 Aufgabe 6: Rekursion in OCL

![[01_02_UMl_OCL_例子/image/Pasted image 20250211110735.png]]

Extrahiert folgende Information mit iterate oder closure aus dem Systemzustand.

a) Wie viel Geld bringen alle Auftr¨age zusammen?

![[01_02_UMl_OCL_例子/image/Pasted image 20250205115028.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250211110911.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250205115122.png]]



b) Wie groß ist der Anteil der Inspektionen am gesamten Umsatz in Prozent? : 


c) Erstellt einen String, in dem die Kund:innen mit den Fahrzeugtypen aller ihrer Autos : 
aufgelistet werden.



d) Erstellt ein Set mit allen geraden positiven Zahlen bis 100.

mit closure 可以解决 