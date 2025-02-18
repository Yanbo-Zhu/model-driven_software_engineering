
# 1 Aufgabestellung 

Eine Autowerkstatt m¨ochte die Abfertigung ihrer Auftr¨age komfortabel mit einer Software verwalten. Dazu k¨onnen Mitareiter:innen (Employee) im System Kunden und Kundinnen (Customer) anlegen und ihnen Fahrzeuge zuordnen. F¨ur neue Kunden und Kundinnen werden jeweils ein Name, eine Telefonnummer und eine Rechnungsadresse gespeichert und die Fahrzeuge werden mit Kennzeichen und Typ registriert.

Ein Auftrag kann entweder eine Inspektion, ein Reifenwechsel oder eine Reparatur sein. Einem neuen Auftrag wird ein Preis, ein Fahrzeug und automatisch ein Datumsstempel zugewiesen. Eine Reparatur erh¨alt außerdem eine genaue T¨atigkeitsbeschreibung. 
Ein Auftrag kann von Mitarbeitenden als beendet markiert werden. In diesem Fall wird der Kunde/die Kundin automatisch vom System benachrichtigt. Außerdem wird f¨ur den Auftrag vermerkt, welche/r Mitarbeiter:in ihn beendet hat.

Um Missbrauch vorzubeugen, m¨ussen sich Mitarbeitende am Browser mit ID und Passwort sicher anmelden. Ein/e Administrator:in kann Mitarbeitende anlegen und entfernen.

# 2 Use-Case-Diagramm

a) Erstellt aus den Anforderungen ein Use-Case-Diagramm, in dem beteiligte Akteure/Aktuerinnen und Anwendungsfaelle enthalten sind.

b) Ueberlegt, ob Beziehungen zwischen Use-Cases die Beschreibung der Anforderungen sinnvoll erweitern wuerden.


---

![[01_02_UMl_OCL_例子/image/Pasted image 20250123220558.png]]

- `<<include>>`: erzwungende gemacht  .  create customers 后必须 create car
- `<<extend>>`:  create car 后 不一定create order
- Create Order 这个 use case 是 oberklasse von 其他三个
- get informed 是一个 note, 用于表述 edge 的作用 

![[01_02_UMl_OCL_例子/image/Pasted image 20250218204101.png]]

# 3 Klassendiagramme

## 3.1 a

a)  Erstellt aus dem Text ein Klassendiagramm, das alle Informationen aus dem Anforderungstext enthaelt, sofern sie abbildbar sind.

Es sollen schrittweise Elemente aus dem Text extrahiert werden und dabei m¨oglichst schon sichtbar werden, was wichtig ist und was nicht. Einige Aspekte werden im Video zur Nachbereitung besprochen, insbesondere zur Bewertung von Testaufgaben (was ist Pflicht, was ist falsch). Diesen k¨onnen gerne auch im Tutorium besprochen werden, aber f¨ur Details kann man auf die Videos verweisen. 

Klassen: 
Alle Substantive (mit Pr¨ufung ob sie f¨ur das System eine Rolle spielen.)

Generalisierung: 
Alternativen auszeigen - was kann durch Gen/Spec eigentlich verallgemeinert werden

Attribute: 
Alternativen bei simplen Klassen/Attibuten z.b. Rechnungsaddresse=String oder Klasse diskutieren. Ein bisschen auf die Typen eingehen

Operationen: 
Use-Cases in den entsprechenden (Actor-)Klassen als Operationen unterbringen. Dazu sagen: Hier muss nicht jede Hilfsfunktion stehen. Wichtig sind Use-Cases, Szenarien, wesentliche Operationen die sich aus sp¨ateren Diagrammen
ergeben. Wir sagen dazu wenn wir Operationen sehen wollen oder wenn sie f¨ur die ¨Ubersichtlichkeit weggelassen werden k¨onnen.

Assoziationen: 
Verbinden was zusammen geh¨ort. Beziehungen zwischen Datenobjekten stehen meist direkt im Text. Leserichtung und Verb sind sch¨on aber nicht Pflicht. Rollen sind nur Pflicht, wenn mehrdeutige Assoziationen existieren.

Multiplizit¨aten: 
F¨ur jedes Assoziationsende diskutieren. Weglassen bedeutet “unterspezifiziert”, kann also in der Implementierung entsprechend wie beliebige Multiplizit¨aten umgesetzt oder auch ganz weggelassen werden. Generell nicht erforderlich f¨ur ein Modell, bei uns k¨onnen sie aber in der Aufgabenstellung gefordert werden.

Aggregation/Komposition: 
Stellen finden die Sinn machen und gut erkl¨aren. Klar machen, dass es keine Pflicht ist diese Features unterzubringen und dass das auch durch Multiplizit¨aten ¨aquivalent beschrieben werden kann. Deutlich machen, dass es (zumindest in Java) keine Entsprechung gibt, die sich von Multiplizit¨aten unterscheidet. Also Zusatz-Info.

Navigationsrichtung: 
Weglassen ist eine Unterspezifikation, d.h. beides ist m¨oglich. Von Controller-artigen Klassen aus einseitig einschr¨anken ist h¨aufig gut. (Eingeschr¨ankte Enden und gleichzeitig Multipliz¨aten widersprechen sich zumindest
aus Implementierungs-Sicht; kann aber sinnvoll sein um zu beschreiben dass ein Produkt z.B. in mehreren Warenk¨orben sein kann, die es nicht kennt oder dass es nur einen OnlineShop gibt, auf den vom Kunden aus kein Zugriff besteht). Navigation in Use-Cases (was brauche ich daf¨ur) erarbeiten. Generell nicht erforderlich f¨ur ein Modell, bei uns kann es aber an der Aufgabenstellung erfordert werden.


Operationen k¨onnen weggelassen werden um nicht zu viel tippen zu m¨ussen. Customer ist hierbei mit Absicht control und entity. Damit wir das sp¨ater sinnvoll trennen k¨onnen. Wenn sich das aber in der Mitarbeit anders ergibt ist das ok.

![[01_02_UMl_OCL_例子/image/Pasted image 20250218211907.png]]




## 3.2 b

b) An welchen Stellen hattet ihr verschiedene Moeglichkeiten zur Modellierung des Klassendiagramms? Diskutiert alternative Entwurfsentscheidungen.

![[01_02_UMl_OCL_例子/image/Pasted image 20250218211921.png]]

## 3.3 c

c) Ueberlegt, wie Stereotype nach dem entity-boundary-control Pattern auf unser Beispiel angewendet werden koennten.

![[01_02_UMl_OCL_例子/image/d21973bcee8377c44fdf92e79053b2d.jpg]]


![[01_02_UMl_OCL_例子/image/cb7b526847e381a54e2803707180664.jpg]]

Beispiel-L¨osung mit Stereotypen und Aufteilung der Customer-Klasse in Daten und Controller (Um dem ECB-Pattern zu gen¨ugen und zu persistierende Daten sauber von der Controller-Schicht zu trennen):
- Noch einmal dr¨uber reden wie das ECB-Pattern gemeint ist: Nicht dogmatisch nur Methoden in Controller und Daten in Entities auftrennen. Sondern: Die Entities beinhalten (persistente) Daten und die Controller ¨ubergeordnete Funktionalit¨at, also insbesondere die Funktionen, die als Einsprung f¨ur die Use-Cases gelten.
- Entities k¨onnen aber auch Operationen haben (sonst w¨are das keine Objektorientierung), diese arbeiten aber dann auf der lokalen Datensicht des jeweiligen Objekts. Controller d¨urfen auch (transiente) Attribute haben wenn das hilft. Assoziationen `(mit 1 → *)` haben sie ja schließlich auch.

![[01_02_UMl_OCL_例子/image/Pasted image 20250218212213.png]]

![[01_02_UMl_OCL_例子/image/f9e35338e5ce49d1c6c984ddd931d92.jpg]]


---

整理前 

![[01_02_UMl_OCL_例子/image/Pasted image 20250123223251.png]]


整理后 

![[01_02_UMl_OCL_例子/image/Pasted image 20250123223544.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250123223559.png]]

![[01_02_UMl_OCL_例子/image/Pasted image 20250123223616.png]]



## 3.4 解释 
``
- 箭头 便是 erben.  -> 所在一端 是 kindklasse,   没有箭头的一端是 eltern class 
- closeOrders: 在OCL中要用到, Employe:closedOrder 代表 从 employe 1 到 Order 
- create, belonges, has close 都是 beziehbung/aktion  , `->` 左边是 主语. 右边是 宾语 

### 3.4.1 entity, controller, boundary 

- `<<entity>>`: enthaelt nur daten/attribute/property
- `<<controller>>`: enthaelt nur method/fucntion
- `<<boundary>>`:  like Interface,   通过 boundary 来 controller aufrufen 
- empolyee 这个 class 分裂为 controller, boundary, entity 

### 3.4.2 notation of kardinalitaet

- Customer - Car : 1 代表 Ein customer zumindest ein car haben (1 car  kann maximal zu einer Customer zugeordnet ), 
- Employee - Car : * 代表 Ein Employee kann beliebig viel car betreuen 
- Customer -  Employee : 1 代表Ein Employer can beliebig Customer haben . X 代表   vom Costumer kann nicht ableiten, die zugehorigekeit von Employe .   Von Customer kommt nicht zu Employee .  就是没有 0...* 的关系 


### 3.4.3 schwarze und hohle Route


- hohle Route in Customer 1:  等价于  1 (notation of kardinalitaet)   
	- wenn customer weg, muss car nicht weg sein
	- wenn car weg, customer stehen noch da 
- schware Raute: wen customer weg, muss addesser noch weg 



# 4 Implementierung
Diskutiert wie eine m¨ogliche Implementierung eures Klassenmodells in Java aussehen k¨onnte.



Teilweise zusammen in Java entwickeln, siehe Beispiel-Implementierung (Java). Was dabei klar werden soll:
• es gibt nur entweder Attribut oder Assoziation
• Ein Attribut kann einfach mit Typ als Member-Variable in die Klasse. Kurz erw¨ahnen dass auch Sichtbarkeit ungef¨ahr von UML ¨ubernommen werden kann.
• Eine Assoziation ist je nach Spezifikation der Navigation durch Attribute/Member- Variablen in einer oder beiden beteiligten Klassen darzustellen. Namen sind entweder frei w¨ahlbar oder benannte Assoziationsenden. Multiplizit¨aten bestimmen Typ:
• Bei 1 und 0..1 entspricht der Typ der Klasse auf der anderen Seite der Assoziation. bei 1 muss der Entwickler sicherstellen, dass das Attribut nie null wird.
• Bei Multiplizit¨aten > 1 muss ein Collection-Typ oder ein Array erstellt werden. In Java muss der Entwickler darauf achten, dass m..n Multiplizit¨aten eingehalten werden.
• Generalisierung z.B. durch Vererbung. Abstrakte Klassen oder Interfaces verwenden, wenn die Oberklasse nicht selbst instanziiert werden kann. Enums funktionieren nur in Ausnahmef¨allen.