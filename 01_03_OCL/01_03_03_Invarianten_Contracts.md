


# 1 motivation 

Bisher haben wir mit OCL einen spezifischen Zustand überprüft

Haben zur Zeit alle Produkte einen Preis > 0?
`os.product->forAll(p | p.price > 0) ≡ true`

![[01_03_OCL/image/Pasted image 20250218180241.png]]


Aber eigentlich wollen wir ja generelleAnforderungen spezifizieren…

Wie können wir Aussagen über alle Instanzen machen,ohne über einen Controller zu navigieren?  (im Beispiel also ohne os zu verwenden) 


# 2 allInstances

Durch allInstances kann direkt auf alle Instanzen einer Klasse zugegriffen werden

```
[Klasse].allInstances() : Set(Klasse)
```

![[01_03_OCL/image/Pasted image 20250218180241.png]]

Wie können wir die Aussage ohne Controller-Instanz formulieren?

```
mit os.product->forAll(p | p.price > 0)≡ true
ohne Product.allInstances()->forAll(p | p.price > 0)≡ true
```



# 3 Invarianten
Invarianten enspricht ein Teilmenge von entire Menge  invariant 代表一个 Teilmenge, die die Bedingungen erfüllt 

预设一些bedingung , 随时看 整个系统某些class 的值 符不符合我们预设的条件 

Mit Invarianten können Modellen Bedingungen hinzugefügt werden
• Die Bedingung gilt dann für die gesamte Lebenszeit aller passenden Objekte

Da für die Invarianten keine konkreten Instanzen existieren, beginnt die Navigation direkt bei einer Klasse
`context [[Bezeichner :] Klasse] inv [Name]: [Boolescher OCL Ausdruck]`

Wie können wir die Aussage als Invariante formulieren?


```
Für alle Produkte gilt: sie haben einen positiven Preis.

Als Aussage über einen konkreten Zustand der Onlineshopinstanz
os.product->forAll(p | p.price > 0)≡ true

Als Aussage über einen konkreten Zustand
Product.allInstances()->forAll(p | p.price > 0)≡ true

Als Invariante für alle Zustände
context p : Product inv: (p.price > 0)

context Product inv:(self.price> 0) // Ohne Bezeichner gilt self

context Product inv:(price > 0)   // oder Kurzschreibweise

```


## 3.1 Beispiele

![[01_03_OCL/image/Pasted image 20250218180932.png]]

-  Name und Email von Kund:innen sind nicht leer
	- context Customer inv: self.name <> ‘‘ and self.email <> ‘‘
- Der Rabatt von Premiumkund:innen liegt zwischen 0 und einschließlich 50
	- context PremiumCustomer inv: discount > 0 and discount <= 50
- Die IDs der Kund:innen sind eindeutig
	- context Cusomer inv: Customer.allInstances().id->count(self.id) = 1    . die menge zählen wir, count(self.id) = 1 这个function 在这 Teilmenge 中被执行 
- Es sind maximal so viele Produkte in Warenkörben, wie im Lager sind
	- context Product inv: Cart.allInstances().product->count(self) <= storedQuantity


# 4 Contracts

Contracts stellen den reibungslosen Ablauf zwischen Funktionen sicher
• Die Vorbedingung muss vom Aufrufenden erfüllt werden
• Die Nachbedingung wird von der Funktion sichergestellt
• In JAVA als Assertions vorhanden


![[01_03_OCL/image/Pasted image 20250218181104.png]]


Eingeführt von Bertrand Meyer für die Programmiersprache Eiffel
“Correctness is clearly the prime quality. If a system does not do what it is supposed to do, then everything else about it matters little.”


---

Contracts in OCL

Contracts können auch direkt in OCL angegeben werden
```
context [Klasse]::[Operation(Parameter)] : [Rückgabetyp]
pre: [Vorbedingungen]
post: [Nachbedingungen]
```



![[01_03_OCL/image/Pasted image 20250218181146.png]]

```
context Onlineshop::changeEmail(customerId, email) : void
pre: self.customer.id->includes(customerId) and email <> ‘‘
post: self.customer->any(id = customerId).email = email   // any: Wähle den/die Kunden/Kundin mit customerId
```


## 4.1 Let und Any

Durch any wird ein passendes Element aus einer Collection ausgewählt

any: wahle irgend ein Elment 

```
Set{1,2,3}->select(i | i > 2) ≡ Set{3} : Set(Integer)
Set{1,2,3}->any(i | i > 2) ≡ 3 : Integer
```

Häufig ist es auch praktisch Zwischenergebnisse über let zu definieren

![[01_03_OCL/image/Pasted image 20250218181232.png]]


Beispiel
```
context Onlineshop::changeEmail(customerId, email) : void
post: let k = self.customer->any(id = customerId) in k.email = email
```

![[01_03_OCL/image/Pasted image 20250218181257.png]]


## 4.2 @Pre

Manchmal hängt die Nachbedingung von dem vorherigen Zustand ab
• Dafür gibt es in OCL Contracts die @Pre-Notation

Beispiele

Durch Ausführung der Operation hat sich an den Produkten nichts geändert
post: Product.allInstances() = Product.allInstances()@pre

Ein neues Produkt p ist dazu gekommen
post: Product.allInstances() = Product.allInstances()@pre->including(p)


Achtung: @pre bezieht sich direkt auf das vorangehende Objekt, nicht den gesamten Ausdruck

- wpk.product 指的是 post 中的product
- wpk.product@pre  contains  p1 und p2. p1 und p2 现在的 storedQuantity 是 18 和 43 

![[01_03_OCL/image/Pasted image 20250218181339.png]]

forAll(storedQuantity > 0 ) , alle product 需要 满足 storedQuantity  > 0 

![[01_03_OCL/image/Pasted image 20250218181355.png]]


## 4.3 Result und oclIsNew

Die Verwendung von result ermöglicht Aussagen über das Ergebnis
```
context: Customer::getId() : Integer
post: result = self.id
```



Die Erzeugung neuer Objekte kann per oclIsNew überprüft werden
```
context: Onlineshop::addProduct(…) : Product
Post: result.oclIsNew()  // ob das object neue ist und vorher nicht da war 
```

![[01_03_OCL/image/Pasted image 20250218181434.png]]


## 4.4 Contracts und Vererbung

Bei Vererbung von Methoden gilt für ihre Contracts das Substitutionsprinzip:
Wenn vor der Ausführung einer Methode in der Unterklasse die Vorbedingungen der entsprechenden Methode der Oberklasse gelten, dann muss die Methode der Unterklasse ausführbar sein und anschließend die Nachbedingungen der Oberklasse garantieren.
- Vorbedingungen dürfen nicht verschärft werden
	- Die VB von Superclass muss auch von Subclass erfullt 
- Nachbedingungen dürfen nicht aufgeweicht werden
	- Die NB von Subclass muss auch von Superclass erfüllt
	-  Ergebnis von Subclass 也符合 Superclass 的定义 

![[01_03_OCL/image/Pasted image 20250218181545.png]]

TypeIn′ ist ein Obertyp von TypeIn
- Kontravarianz
TypeOut′ ist ein Untertyp von TypeOut
- Kovarianz

Dabei gilt:
- Vorbedingungen von method aus SubClass werden von Vorbedingungen von method aus SuperClass impliziert
- Nachbedingungen von method aus SubClass implizieren Nachbedingungen von method aus SuperClass
- Diese Vererbungs-Konformität wird Kontra- und Kovarianz genannt
- Entsprechend lassen sich auch andere Varianten von Konformität definieren






