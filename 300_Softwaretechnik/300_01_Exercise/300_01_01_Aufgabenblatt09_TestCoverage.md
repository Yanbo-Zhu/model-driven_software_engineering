
# 1 知识准备 

![](image/Pasted%20image%2020241218102219.png)


---

可以在 eclispse 上显示 coverage rate 的工具 
Eclipse mit JUnit, EclEmma und Metrics
![](image/Pasted%20image%2020241218125000.png)

# 2 Anforderungstext

Die Funktion createOrder(int customerID, int productID, Instant created) soll im System einen Auftrag erstellen. Als erstes wird gepr¨uft, ob die Kunden- und Kundinnennummer (customerID) im System vorhanden ist. Ist dies nicht der Fall wird nur die Fehlermeldung ”Customer nicht im System“ ausgegeben. Andernfalls wird die Waren-
nummer (productID) gepr¨uft. 
Existiert die Warennummer nicht im System, wird nur die Fehlermeldung ”Es existiert keine Ware mit der gegebenen Warennummer“ ausgegeben. Ansonsten wird gepr¨uft, ob ein Erstelldatum (created) angegeben wurde. Ist dies nicht der Fall wird nur die Meldung ”Kein Erstellungsdatum angegeben“ ausgegeben. Sowohl customerId als auch productId werden als fortlaufende Nummern beginnend bei 0 vergeben und zur Vereinfachung gehen wir davon aus, dass einmal angelegte Kunden/Kundinnen und Waren niemals aus dem System gel¨oscht werden. Wurden alle Daten korrekt eingegeben, wird ein Auftrag im System erstellt.



# 3 Aufgabe 1: Funktionsorientierter Test

Ermittelt mit den folgenden funktionsorientierten Verfahren Testf¨alle:
a) ¨Aquivalenzklassenbildung :
b) Grenzwertanalyse :
c) Entscheidungstabellentest :
d) optimierte Entscheidungstabelle 


## 3.1 a) ¨Aquivalenzklassenbildung :


`#customer-1`  为 gesamte Anzahl von customer  -1 

![](image/Pasted%20image%2020241218103419.png)

## 3.2 b) Grenzwertanalyse




![](image/Pasted%20image%2020241218103157.png)

![](image/Pasted%20image%2020241218103433.png)

warum -1
因为只测试 grenzwert , 有下图可知  -1 为一个 边界值 
![](image/Pasted%20image%2020241218103419.png)


## 3.3 Entscheidungstabellentest

![](image/Pasted%20image%2020241218103543.png)



t1 =  testcase 1 


![](image/Pasted%20image%2020241218104243.png)


根据条件合并  = d) optimierte Entscheidungstabelle 
t2 , t4, t6, t8 只要 customer ID nicht ok 为 ja, 得到的结果都一样 
t3 和 t7 可以合并 


![](image/Pasted%20image%2020241218104455.png)


# 4 #


Implementierung
Ihr habt nun den Code der Funktion erhalten. Dabei stellt ihr fest, dass die Kunden und Kundinnen f¨ur bestimmte Waren Rabatte sammeln k¨onnen. Handelt es sich bei den Bestellenden um einen Premium-Kunden oder eine Premium-Kundin und bei der bestellten Ware um rabattierf¨ahige Ware, so wird der Preis der Ware um alle bisher gesammelten Rabatte des Kunden und der Kundin reduziert. Zu bestimmten Jubil¨aumsaktionen wer-den auch bei allen anderen Kunden und Kundinnen und rabattierf¨ahiger Ware die bisher gesammelten Rabatte eingel¨ost.


```
1 public Order c r e a t e O r d e r ( int customerID , int productID ,
2 I n s t a n t c r e a t e d ) throws ShopInputException {
3
4 // h a n d l e i n v a l i d i n p u t s
5 i f ( ! c u s t o m e r s . containsKey ( customerID ) ) {
6 throw new ShopInputException (
7 ShopInputException . INVALID CUST NR) ;
8 }
9 i f ( ! p r o d u c t s . containsKey ( productID ) ) {
10 throw new ShopInputException (
11 ShopInputException . INVALID PROD NR) ;
12 }
13 i f ( c r e a t e d == null ) {
14 throw new ShopInputException (
15 ShopInputException . INVALID DATE) ;
16 }
17
18 // g e t costumer and p r o d u c t f o r g i v e n IDs
19 Customer c = c u s t o m e r s . g e t ( customerID ) ;
20 Product p = p r o d u c t s . g e t ( productID ) ;
21 // c r e a t e new o r d e r
22 Order o r d e r = new Order ( customerID , productID ,
23 p . g e t P r i c e ( ) , c r e a t e d ) ;
24
25 i f ( ( c . i s P r i o r i t y C u s t o m e r ( ) & p . i s D i s c o u n t a b l e ( ) )
26 | | i s A n n i v e r s a r y ( c r e a t e d ) ) {
27 o r d e r . s e t D i s c o u n t ( true ) ;
28 while ( ! c . g e t D i s c o u n t ( ) . isEmpty ( ) ) {
29 o r d e r . d i s c o u n t ( c . g e t D i s c o u n t ( ) . r e m o v e F i r s t ( ) ) ;
30 }
31 }
32 return o r d e r ;
33 }
```


Aufgabe 2: Strukturorientierter Test
F¨ur diese Aufgabe kann die Code-Vorgabe um passende JUnit-Tests erweitert werden.
Die Anweisung- und Zweig¨uberdeckung k¨onnen mit dem EclEmma Java Code Coverage
Plug-In ¨uberpr¨uft werden.
a) Installiert und testet die Eclipse Plugins EclEmma und Metrics mit der Code-Vorlage. 

b) Ermittelt die mit den bisherigen Testf¨allen erreichte Anweisungsuberdeckung. Erstellt : gegebenenfalls weitere Testf¨alle bis 100% Anweisungs¨uberdeckung erreicht ist.

c) Ermittelt die mit den bisherigen Testf¨allen erreichte Entscheidungsuberdeckung (Zweiguberdeckung). Falls diese unter 100% liegt, erarbeitet weitere Testf¨alle um vollst¨andige Entscheidungs¨uberdeckung zu erreichen.

d) Ermittelt die mit den bisherigen Testf¨allen erreichte (einfache) Bedingungsuberdeckung. Falls diese unter 100% liegt, erarbeitet weitere Testf¨alle um vollst¨andige Bedingungs¨uberdeckung zu erreichen.

e) Ermittelt die mit den bisherigen Testf¨allen erreichte Pfaduberdeckung. Falls diese : unter 100% liegt, erarbeitet weitere Testf¨alle um vollst¨andige Pfad¨uberdeckung zu erreichen.

![](image/Pasted%20image%2020241218104940.png)



## 4.1 Anweisungsuberdeckung

![](image/Pasted%20image%2020241218105252.png)


![](image/Pasted%20image%2020241218105610.png)




## 4.2 Entscheidungsuberdeckung


![](image/Pasted%20image%2020241218105244.png)


![](image/Pasted%20image%2020241218110310.png)

## 4.3 (einfache) Bedingungs¨uberdeckung

(a &b) 必须一次 true. 一次 false 

![](image/Pasted%20image%2020241218105312.png)

## 4.4 Pfaduberdeckung



![](image/Pasted%20image%2020241218111054.png)



绿色代表 bedingungen 为 ja, 
红色代表 bedingung 为 false 
黑色的线代表  这里没有bedingung 判断 ,没有 false oder true 的判断 

40 代表的意思为 Zeile 40


![](image/Pasted%20image%2020241218111558.png)







