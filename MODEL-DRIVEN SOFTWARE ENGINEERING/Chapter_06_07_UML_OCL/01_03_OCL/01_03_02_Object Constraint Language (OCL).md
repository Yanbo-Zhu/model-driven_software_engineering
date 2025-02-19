
# 1 Werte


Auf Werte von Objekten kann mit der Punktnotation zugegriffen werden
`[Objektbezeichner].[Attributbezeichner] ≡ [Wert] : [Typ]`

Beispiele
```
pk.id≡ 128 : Integer
pk.name≡ 'Lisa' : String
pk.email≡ 'l*****@tu-berlin.de' : String
Pk.discount≡ 15 : Integer
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214110903.png]]


# 2 Primitive Typen

In OCL sind vier primitive Typen vordefiniert
• Außerdem sind einige spezifische Operationen bereits vorhanden

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214110927.png]]

Operationen auf primitiven Typen funktionieren wie gewohnt
•Dabei findet kein implizites Typcasting statt

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111031.png]]

```
p1.storedQuantity- 12 ≡ 6 : Integer
p1.productId > 0 ≡ true : Boolean
p1.productId = p2.productId ≡ false : Boolean
if p1.price > p2.price then 1 else 2 endif ≡ 1: Integer
p1.description.size() ≡ 7 : Integer
p1.description.concat(' Brille') ≡ 'Raybaem Brille' : String
p1.description.substring(1, 3)≡ 'Ray' : String
```


# 3 Navigation

Der `Zugriff über Assoziationen` erfolgt analog zum Zugriff auf Attribute
• Dabei gibt die Rollenbezeichnung den Attributnamen an
- ==当 是 0,1 关系的时候 wk.customer.id 出来的是一个单个的值 不是一个 set ==

```
wk.customer≡ k : Customer
wk.customer.id≡ 129 : Integer
wk.customer.name≡ 'John' : String
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111155.png]]

---

Navigation auf Collections

Beim Zugriff auf Assoziationen mit * gibt OCL eine Collection zurück 
• Operationen auf Collections verwenden als Notation den Pfeil (->)
- ==当 是 * 关系的时候 wk.customer.id 出来的是一个 set ==
- 
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111221.png]]

```
wpk.product≡ Set{p1,p2} : Set(Produkt)
wpk.product->size()≡ 2 : Integer
```

# 4 Collections: set/bag/orderedSet/Sequence

In OCL existieren vier unterschiedliche Collections…
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111526.png]]
…mit gemeinsamen, vordefinierten Operationen

```
c->size() : Integer Anzahl Elemente in Collection c
c->isEmpty : Boolean true, wenn c leer ist
c->includes(obj:OclAny) : Boolean true, wenn obj in c vorkommt
c->excludes(obj:OclAny) : Boolean false, wenn obj in c vorkommt
c->count(obj:OclAny) : Integer Häufigkeit von obj in c
```


----

Set/Bag Operationen
Collections ohne Beachtung der Reihenfolge erlauben Mengenoperationen

```
= (y : Set(T)) : Boolean Gleichheit
including(y : T) : Set(T) Hinzufügen
excluding(y : T) : Set(T) Entfernen
union(y : Set(T)) : Set(T) Vereinigung
intersection(y : Set(T)) : Set(T) Schnitt
−(y : Set(T)) : Set(T) Differenz (nur Set)
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111556.png]]

----

OrderedSet/Sequence Operationen
Collections mit Berücksichtigung der Reihenfolge erlauben Indexzugriff
```
at(y : Integer) : T Indexzugriff
append(y : T) : Sequence(T) Hinten anfügen
prepend(y : T) : Sequence(T) Vorne anfügen
insertAt(i : Integer, y : T) : Sequence(T) An i einfügen    Achtung: USE fügt nach i ein
first() : T ≡ S->at(1) Erstes Element
last() : T ≡ S->at(S->size()) Letztes Element
subSequence(y : Integer, z : Integer) : Sequence(T) Ausschnitt
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111753.png]]

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214111850.png]]

Ist von jedem Produkt min. eins in den Warenkörben von Lisa oder John?
k.cart.product->union(pk.cart.product) = os.product
k.cart.product->union(pk.cart.product) : Set(Product) = Set{p1,p2,p3} = os.product : Set(Product)

这两个 Product 组成一个 union, 这个 这个 product 同时在 os:onlineshop 中存在. alle Product, die in os:onlineshop gibt 

---

Umwandlung von Collections
Zwischen den verschiedenen Collections kann konvertiert werden
• Dabei gehen eventuell doppelte Elemente verloren oder werden umsortiert*
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214112005.png]]

Mengen von Mengen können in einfache Mengen umgeformt werden
```
flatten() : Set(T)
Set{Set{1,2},Set{3,4},Set{5,6}}->flatten() ≡ Set{1,2,3,4,5,6}
```

Die Reihenfolge ist in Sets nicht spezifiziert, aber abhängig vom Tool meist deterministisch. Wie hier zu
sehen, sortiert USE die Werte eines Set(Integer) vor der Ausgabe

# 5 Operator

beispiel 


![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214112432.png]]

Von welchen Produkten sind mehr als 30 Stück auf Lager?
Welche Produktnummern sind im System vergeben?
os.product->select(p : Product | p.storedQuantity > 30) ≡ Set{p2,p3} : Set(Product)
os.product->collect(p : Product | p.productId) ≡ Bag{1568,3663,8785} : Bag(Integer)

select: 从中选择符合添加到呢 

## 5.1 Auswahl select reject 

Aus Collections können gewünschte Werte gefiltert werden

```
c->select(x : T | P(x)) 𝑥 ∈ 𝐶 𝑃(𝑥)}
c->reject(x : T | P(x)) 𝑥 ∈ 𝐶 ¬𝑃 𝑥 }
```

```
Set{1,2,3}->select(i : Integer | i > 1) ≡ Set{2,3} : Set(Integer)
Set{1,2,3}->reject(i : Integer | i <= 1) ≡ Set{2,3} : Set(Integer)
```

```
T[] c;
T[] select() {
	T[] result;
	for (T x : c) {
		if (P(x)) result.add(x);
	}
	return result;
}
```


## 5.2 Sammlung

Auf Werte in Collections können auch Ausdrücke angewandt werden
• Dabei kann sich der Typ der resultierenden Collection ändern!
collect gibt immer ein Bag zurück!

```
c->collect(x : T | E(x)) 𝑒 ∃ 𝑥 ∈ 𝐶. 𝑒 = 𝐸(𝑥)}


Set{1,2,3}->collect(i : Integer | i+1) ≡ Bag{2,3,4} : Bag(Integer)
Set{1,2,3}->collect(i : Integer | i.toString()) ≡ Bag{'1','2','3'} : Bag(String)
```

```
T[] C;
T2[] collect() {
	T2[] result;
	for (T x : C) {
		T2 e = E(x); result.add(e);
	}
	return result;
}
```

## 5.3 Iteration

Werte in Collections können beliebig zusammengefasst werden
• Viele vordefinierte Collection-Operationen sind über iterate definiert

```
c->iterate(x : T; acc : T2 = startwert | E(acc, x))

Beispiele
Set{1,2,3}->iterate(x : Integer; acc : Integer = 0 | acc + x) ≡ 6 : Integer
Set{1,2,3}->iterate(x : Integer; acc : String = ‘‘ | acc + x.toString()) ≡ '321' : String  Set hat keine
Reihenfolge, 可以使 321 , 也可以是 123, 213, 

T[] c;
T2 iterate(T2 startwert) {
	T2 acc = startwert;
	for (T x : c) {
		E(acc, x);
	}
	return acc;
}
```


## 5.4 Quantoren forAll exists includesAll exludesAll

Durch Quantoren werden Bedingungen auf Collections übertragen
```
c->forAll(c : T | P(c)) ∀ 𝑐 ∈ 𝐶. 𝑃(𝑐)
c->exists(c : T | P(c)) ∃ 𝑐 ∈ 𝐶. 𝑃(𝑐)
```

Damit lässt sich z.B. der Teilmengenoperator spezifizieren
```
c->includesAll(cy : Col(T)) : Boolean ≡ cy->forAll(e | c->includes(e))
c->exludesAll(cy : Col(T)) : Boolean ≡ cy->forAll(e | c->excludes(e))
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214112735.png]]

Wieviel kostet Lisas Warenkorb zur Zeit?
Sind alle Produkte darin günstiger als 180€?
```
wpk.product->iterate(p : Product ; acc : Integer = 0 | acc + p.price) ≡ 280 : Integer
wpk.product->forAll(p : Product | p.price < 180) ≡ false : Boolean

wpk.product->iterate(p : Product; acc : Integer = 0 | acc + p.price) * (100-pk.discount)/100 ≡ 238.0 : Real
wpk.product->forAll(p : Product | p.price * (100-pk.discount)/100 < 180) ≡ true : Boolean
```


# 6 Tupel 

Durch Tupel können Werte strukturiert werden
- Tupel sind keine Collections, eher temporäre Klassen/Objekte
- `Tuple{[Bezeichner [: Typ]] = [Wert] [, …]}`

Auf Werte in Tupeln wird analog zu Werten in Objekten zugegriffen



```
Tuple{sensor='Temp',wert=12} : Tuple(sensor:String,wert:Integer)
Tuple{sensor='Temp',wert=12}.sensor≡ 'Temp' : String
Tuple{sensor='Temp',wert=12}.wert≡ 12 : Integer
```


# 7 Closure

Die Closure-Operation bildet die transitive Hülle einer Beziehung, z.B. einer Assoziation oder Generalisierung
	C->closure(c : T | P(c))
Akkumulation der Ergebnisse der rekursiven Anwendung von P(c) auf alle Elemente der Menge
Terminiert wenn sich die Menge nicht mehr ändert
Hinzugefügt 2011 in Version 2.3

---

Nützlich zur Akkumulation von rekursiven Daten

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214113136.png]]


```
bart.parents->closure(parents) ≡ Set{homer,abe} : Set(Person)
Set{abe}->closure(children) ≡ Set{abe,bart,homer,lisa} : Set(Person)
```

Die Closure kann auch für allgemeine Schleifen verwendet werden:
- Set{1}->closure(x| if x < 5 then x+1 else x endif) ≡ Set{1,2,3,4,5} : Set(Integer)


Zum Knobeln: Wie berechne ich die Fakultät der ersten n Zahlen mit closure und Tupeln?


# 8 OCL Typhierarchie

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214113300.png]]


# 9 AnyType

Alle Objekte in OCL erben von OclAny
Ähnlich dem generellen java.lang.Object in Java

Dadurch können Collections verschiedene Typen enthalten
`Set{'a',1.0,true} : Set(OclAny)`

Werte können auch explizit gecastet werden
`obj.oclAsType(t : OclType) : T`

Gleichheit auf OclAny überprüft die Referenz (wie in Java)
`=(obj2 : OclAny) : Boolean`


# 10 Generalisierung/Spezialisierung  oclIsTypeOf  oclIsKindOf 
 
Typen lassen sich per oclIsTypeOf oder oclIsKindOf bestimmen…

```
// Type Überprüft den tatsächlichen Typ des Objekts
pk.oclIsTypeOf(Customer)≡ false
pk.oclIsTypeOf(PremiumCustomer)≡ true
k.oclIsTypeOf(Customer)≡ true
k.oclIsTypeOf(PremiumCustomer)≡ false

// Kind Überprüft ob das Objekt dem Typ entspricht (Typ oder Subtyp)

pk.oclIsKindOf(Customer)≡ true
pk.oclIsKindOf(PremiumCustomer)≡ true
k.oclIsKindOf(Customer)≡ true
k.oclIsKindOf(PremiumCustomer)≡ false
```

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250214113429.png]]

# 11 VoidType als Nullwert

Es gibt einen speziellen Nullwert vom Typ OclVoid
• Kann als null oder Undefined verwendet werden
• Kann mit allen anderen Objekten verglichen werden

```
null : OclVoid
1/0 ≡ Undefined : OclVoid
1/0 = Undefined ≡ true : Boolean
1/0 = null ≡ true : Boolean
(1/0).oclIsUndefined ≡ true : Boolean
Sequence{1,2,null,4,5} : Sequence(Integer)
Sequence{1,2,null,4,5}->size() ≡ 5 : Integer
```

# 12 Dreiwertige Logik in OCL


Durch den Nullwert ergibt sich in OCL eine dreiwertige Logik

![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250218170723.png]]



# 13 Abkürzungen

OCL kann in vielen Fällen den Typ ableiten

```
Set{-1,2,-3}->select(i : Integer | i.abs() > 0)
Set{-1,2,-3}->select(i | i.abs() > 0)   // OCL „weiß“, dass i ein Integer sein muss
Set{-1,2,-3}->select(abs() > 0)    // Und dass die Werte als Argument dienen
```


Aus praktischen Gründen gibt es auch eine Kurzschreibweise für collect
• Wird immer bei Anwendung der Punknotation auf Collections angenommen

```
Set{-1,2,-3}->collect(i | i.abs()) ≡ Bag{1,2,3} : Bag(Integer)
Set{-1,2,-3}.abs() ≡ Bag{1,2,3} : Bag(Integer)
```

Beispiele
![[MODEL-DRIVEN SOFTWARE ENGINEERING/Chapter_06_07_UML_OCL/01_03_OCL/image/Pasted image 20250218171026.png]]

Wie viele Produkte sind zur Zeit im Lager?
os.product.storedQuantity->iterate(m; s : Integer = 0 | s + m)≡ 163 : Integer

Wer kauft Produkte über 80€?
os.product->select(price > 80).cart.customer.name->asSet()≡ Set{'Lisa'}



# 14 Collect und CollectNested

```
os.kunde.cart->collect(product)    // Eigentlich müsste das eine Menge von Mengen sein…

os.customer.cart≡ Bag{wk,wpk} : Bag(Cart)
Bag{wk,wpk} ->collect(product)≡ Bag{p1,p2,p3} : Bag(Product)   // …ist es aber nicht.

collect beinhaltet immer auch flatten - collectNested nicht:
Bag{wk,wpk}->collectNested(product) ≡ Bag{Set{p3},Set{p1,p2}} : Bag(Set(Product))


Bag{wk,wpk}->collect(product) = Bag{wk,wpk}->collectNested(product)->flatten()
```

- Use `collect` when you want a **flat collection** of transformed elements.
	-  就是说 返回的值 是一个 collection, 里面每个element 都只包含一个 单一的值
- Use `collectNested` when you need to **preserve the structure** of nested collections.
	-  返回的 collection 中 , 里面每个element 都可以包含 一个 collection (一个collection 中可以包含多个元素 )  , 这些就被成为 nestedCollection 


---

The `collect` operator is used to create a new collection by applying a specific property or computation to each element in an existing collection.
- Produces a **flat** (non-nested) collection.
- Transforms elements of the original collection but does not keep them as nested collections.

```
context Person
inv: self.friends->collect(p | p.name)
```

Explanation
- `self.friends` is a collection of `Person` objects.
- `collect(p | p.name)` extracts the `name` property of each `Person`.
- The result is a **flat collection** of names.

---

The `collectNested` operator works similarly to `collect`, but it **preserves** the structure of nested collections instead of flattening them.
- Produces a **nested** collection.
- If each element in the original collection produces another collection, `collectNested` keeps these subcollections instead of merging them into one.

```
context Person
inv: self.friends->collectNested(p | p.hobbies)
```

Explanation
- `self.friends` is a collection of `Person` objects.
- Each friend has multiple hobbies (a collection).
- ==`collectNested(p | p.hobbies)` results in a **nested collection of hobbies**, where each subcollection corresponds to a specific friend.==












