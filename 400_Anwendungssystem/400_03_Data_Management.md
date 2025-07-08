
• discuss the concept of (de)serialization
• make persistence decisions (file system vs. database system)
• read and write XML and JSON documents
• read and modify XML schema documents
• determine results of XPath queries
• use protocol buffers in Java programs
• decide between different database options (e.g., column store vs. time series database)
• write simple SQL queries
• conceptually discuss ORM concepts including JPA and JDBC
• use JPA annotations
• Conceptually discuss transactions, ACID properties, and 2PC


# 1 Data serialization

Serialization is used to transform the programming language-specific in-memory representation of data ==into a sequence of bits.==

Example: Java object => data for network transmission (e.g., via ObjectOutputStream)

You can easily create your own custom serialization format but should aim for the following:
• Structured data
• Space-efficient binary format or human-readable
• Platform-independent

Simple example: CSV

![[image/Pasted image 20250515144508.png]]


Serialization formats
There are literally hundreds of serialization formats, many of them deprecated:
• CSV: invented in the 1970s, still in-use due to its simplicity (no libraries needed) and good fit for table-format data.
• XML: published 1998, still in-use due to legacy systems and rich ecosystem but not the typical choice for a new project.
• JSON: started in early 2000s in the web context, widely used wherever space is not an issue.
• YAML: started in early 2000s, JSON alternative.
• Protocol Buffers: developed in 2001 at Google, published in 2008. Origin as communication format, today used as general data format.
• Thrift: Facebook‘s protobuf alternative, mainly used as communication format.
• Avro, Parquet: serialization formats mainly used in big data frameworks.


Elements of serialization formats
- Syntactic rules (“how to separate bits of information“)
• Semantic structure (“where which bit of information is expected“)
• Query and transformation capabilities



![[image/Pasted image 20250515150812.png]]



## 1.1 XML

![[image/Pasted image 20250515144842.png]]

![[image/Pasted image 20250515144849.png]]


![[image/Pasted image 20250515145619.png]]


### 1.1.1 XML Schema

Schema language for validating XML documents:
• Does the document fit a certain pattern?
• Does the document have a certain structure?
XSD files are referenced from XML documents so that those documents can be validated
against the schema
Often used for Internet technologies, e.g., in WSDL documents (=> Part IV) or in specifications
to ”define“ exact XML structure and types expected by a service etc.

![[image/Pasted image 20250515145658.png]]

Basic structure of XSD

The root element is always <xs:schema/>.
Child nodes of the root are of two types:
• xs:element defines the root node for all XML documents which can be validated with this XSD
• xs:simpleType and xs:complexType define the types which are used in the document
Java analogy: (does not fit completely but is close enough)
• Types are equivalent to class Foo {…}
• Elements are equivalent to Foo f = new Foo();

---

下面的 东西  没有去看 

Elements 
Self-defined types: <xs:simpleType>




#### 1.1.1.1 题目

Ordnen Sie die folgenden Fragmente aus XML-Schema- und XML-Dokumenten einander zu.
Hinweis: Zum Teil gibt es mehrere M¨oglichkeiten oder auch keine richtige L¨osung

```
<xs:element name="dataentry" type="data"/>
<xs:simpleType name="data" type="xs:string"/>
```


```
<xs:element name="dataentry" type="data"/>
<xs:simpleType name="data">
	<xs:restriction base="xs:string">
		<xs:maxLength value="6"/>
	</xs:restriction>
</xs:simpleType>
```

```
<xs:element name="dataentry" type="data"/>
<xs:simpleType name="data">
	<xs:restriction base="xs:string">
		<xs:enumeration value="Monday"/>
		<xs:enumeration value="Tuesday"/>
		<xs:enumeration value="Wednesday"/>
		<xs:enumeration value="Thursday"/>
		<xs:enumeration value="Friday"/>
		<xs:enumeration value="Weekend"/>
	</xs:restriction>
</xs:simpleType>
```

```
<xs:element name="dataentry" type="data"/>
<xs:complexType name="data">
	<xs:choice>
		<xs:element name="value" type="xs:string" maxOccurs="1"/>
		<xs:element name="value2" type="xs:integer" maxOccurs="1"/>
	</xs:choice>
</xs:complexType>
```

```
<xs:element name="dataentry" type="data"/>
<xs:complexType name="data">
	<xs:sequence>
		<xs:element name="value" type="xs:string" minOccurs="0" maxOccurs="5"/>
		<xs:element name="value2" type="xs:integer" minOccurs="0" maxOccurs="5"/>
	</xs:sequence>
</xs:complexType>
```


A)
`<dataentry>some value</dataentry>`

passt zu 1 

B)
`<data>some value</data>`
zu gar nicht gehoren 

C)
`<dataentry>value</dataentry>`

zu 1 und 2 



D)
`<dataentry>Monday</dataentry>`

zu 1, 2,3 

E)
`<data>Monday</data>`

zu gar nicht gehoren 

`
F)
```
<dataentry>
<value>some value</value>
<value2>12345</value2>
</dataentry>
```

zu 5 


G)
```
<data>
	<value>some value</value>
</data>
```


zu 4 oder 5 passen 



H)
```
<dataentry>
<value2>12345</value2>
</dataentry>
```

zu 4 und 5 


I)
```
<dataentry>
<value2>123</value2>
<value2>456</value2>
<value2>789</value2>
<value2>0</value2>
</dataentry>
```


zu 

J)
```
<dataentry>
<value>Friday</value>
</dataentry>
```

zu 4 5 

warum nicht 1 ,2,3 passt , `weil <value> ist the kindelemnt von <datenentry> `


### 1.1.2 XPath

通过 这个 xPath 去选中一个 element 

An XPath expression consists of a sequence of location steps. Each location step can have three components:
• an axis (if none is provided it defaults to child): describes the direction from the set of nodes selected in the preceding location step
• a node test: describes which nodes to select as a result of this location step
• and a predicate (optional): describes which nodes to remove from the result set

Starting from a current (set of) node(s):
• The axis describes the direction for the next step.
• The node test describes which nodes (type and name) shall be selected.
• The optional predicate describes which nodes not to drop from that set




Every location step defines a node (or a set of nodes) within the XML tree. The subsequent location step is then evaluated relative to that node.
The axis (cf. next slide) defines the direction which is taken from the currently selected node.
The node test defines which nodes shall be selected next (can result in an empty set) The predicate can remove nodes from the selected node set.

![[image/Pasted image 20250515150237.png]]


![[image/Pasted image 20250515151138.png]]

1 Axis abbreviations
Instead of /child::A one can also use /A
/A//Z is the short version of /A/descendant-or-self::Z
=> Example: If it does not matter where a specific node is located within the document //Z will
select all occurences of Z (no matter where in the document)
. selects the current node
.. selects the parent of the current node


2 Node tests
Starting from a set of nodes, following an axis leads to another set of potential nodes. Out of
these, the node test describes which shall be selected.
Node tests may comprise specific node names or more general expressions:
    • …/foo/… selects all foo elements
    • …/*/… selects all elements regardless of their name
    • …/@bar selects all bar attributes (also /@* selects all attributes)

Other node test formats are:
    • text() selects a node of type text, e.g. the “hello” in `<k>hello</k>`
    => don’t confuse this with text content of attributes!
    • node() selects all nodes (no matter what their type may be)
    • comment() selects an XML comment node, e.g., `<!-- Comment -->`
    • processing-instruction() selects XML processing instructions such as `<?php echo $a; ?>`



Predicates
Predicates remove all nodes from the node set returned by the node test which do not meet the
predicate’s condition.
There are no limits regarding the predicate’s complexity.
Predicates are always embedded in square brackets
For example, `//a[@href=“help.html”]` will select all a elements which include an href attribute with
value “help.html”.

May include any operators and functions defined within XPath (cf. next slides)
Boolean Operators: and, or, not(arg), boolean(arg), true(), false()
Arithmetic Operators: +, -, *, div, mod
Comparison Operators: =, !=, <, >, <=, >=
Existence Operator: A node name, e.g., `/A/B/C[@D]`
Union Operator: expr1 | expr 2
Position Operators:` [position() = 5] (short: [5]), [last()], [last() -1], …`

Numeric values: number(arg), abs(num), ceiling(num), floor(num), round(num),…
Strings: string(arg), concat(string,string,…), matches(string,pattern), substring(string,start,length), contains(string,string), replace(string,pattern,replace), …
Node sets: distinct-values(nodeSet), index-of((nodeSet),item), count(nodeSet), avg(nodeSet), max(nodeSet), min(nodeSet), sum(nodeSet), …


---


```
<?xml version=“1.0” encoding=“utf-8”?>
<wikimedia>
<projects>
<project name=“Wikipedia” launch=“2001-01-05”>
<editions>
<edition language=“English”>en.wikipedia.org</edition>
<edition language=“German”>de.wikipedia.org</edition>
<edition language=“French”>fr.wikipedia.org</edition>
<edition language=“Polish”>pl.wikipedia.org</edition>
<edition language=“Spanish”>es.wikipedia.org</edition>
</editions>
</project>
<project name=“Wiktionary” launch=“2002-12-12”>
<editions>
<edition language=“English”>en.wiktionary.org</edition>
<edition language=“French”>fr.wiktionary.org</edition>
<edition language=“Vietnamese”>vi.wiktionary.org</edition>
<edition language=“Turkish”>tr.wiktionary.org</edition>
<edition language=“Spanish”>es.wiktionary.org</edition>
</editions>
</project>
</projects>
</wikimedia>
```


![[image/Pasted image 20250515151922.png]]


## 1.2 Json 

![[400_Anwendungssystem/image/Pasted image 20250610145214.png]]

```
//1. Objekt (unordered set of key/value pairs, Java map)
{
  "firstName": "John",
  "lastName": "Smith"
}

//2. Array (ordered list of values). Speichert nur values, keine keys
[ "a", "b", "c" ]



{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25,
  "address": {
    "streetAddress": "21 2nd Street",
    "city": "New York"
  },
  "phone": [
    { "type": "home", "number": "212 555-1234" },
    { "type": "fax", "number": "646 555-4567" }
  ]
}
```


## 1.3 Protocol Buffers

![[400_Anwendungssystem/image/Pasted image 20250610145302.png]]

Developed (and used) at Google as
• platform-independent,
• highly efficient (space, performance),
• extensible
serialization format targetting inter-machine communication.

![[image/Pasted image 20250515152436.png]]


### 1.3.1 Wie funktionieren Protocol Buffers?

1. Proto-Definition schreiben
	1. Ähnlich wie ein XML-Schema (XSD) definiert man in .proto Dateien die Struktur der Nachrichten.
2. Code generieren
	1. Mit dem Tool protoc werden für die Zielprogrammiersprache Klassen (Stubs) erzeugt.
3. Serialisieren / Deserialisieren
	1. Die generierten Klassen werden verwendet, um Daten zu (de)serialisieren.


### 1.3.2 Aufbau einer .proto Datei und Proto syntax

Proto files look similar to OOP but, e.g., don‘t do inheritance. Generated stubs look similar to Java Beans.

```
message MessageName {
  <Modifier> <Type> field_name = <TagNumber>;
}
```




![[image/Pasted image 20250515152559.png]]

Message: Grundbaustein (ähnlich einer Java-Klasse)


Modifiers can be:
• optional: may or may not be set, if not default value is used
• repeated: can be repeated zero or more times, order is preserved
• required: if not set, message building will throw exceptions. Don‘t
use it because “required is forever“. (deprecated in proto3)


Types:
• Standard basic types (numbers, boolean, string, byte [])
=> https://protobuf.dev/programming-guides/proto3/#scalar
• Composite types or enums
=> (one can tell that it was inspired by XSD)

Tag number:
• Describes the unique tag number used to identify the item in binary
format.
• Numbers 1-15 are smaller and should be used for frequent fields.

Style:
• Use camel case as in Java class names for message names
• Use lowercase characters and separate concatenated words with
underscores for fields
=> Generated code adheres to best practices of target language

### 1.3.3 Genetierter Java-Code

Für jede Message wird eine Klasse erzeugt, ggf. mit verschachtelten Klassen.
![[400_Anwendungssystem/image/______________2025-05-26_154833.webp]]

Eine statische BuilderKlasse erlaubt das Erstellen von Instanzen im Builder-Pattern.
![[400_Anwendungssystem/image/______________2025-05-26_154925.webp]]


# 2 Database systems



## 2.1 Object-Relational Mapping & JPA  Jakarta Persistence API


JPA  Java Persistence API contains more features
• Transactions (supported through JTA for the Java EE implementation of JPA, for Java SE environments, a simplified transaction API is provided)
• Additional Metadata APIs for Entities and ORM (many more Annotations!)
• Instead of Annotations, XML descriptors can be used for basically everything

JPA is a standard, not an implementation. It is used by many other frameworks:
• Hibernate, another popular ORM provides also an implementation of JPA
• EclipseLink also implements JPA
• Spring Data JPA builds on top of JPA (Lower level) database access in Java is typically done via JDBC which provides a direct connection to a database from Java applications


### 2.1.1 Java Database Connectivity (JDBC)

![[image/Pasted image 20250515153326.png]]

JPA started out as a Java EE standard (=> Part VI) but now also works stand-alone.
It builds on top of JDBC to provide a high-level API for interacting with RDBMS.

==JPA is a standard, not an implementation.==
JPA introduced the concept of (persistent) entities.

==Durch JPA  definiert Man Entitäten als Java-Klassen mit Metadaten (Annotationen). Dadurch ist keine SQL noetig==

==In contrast to JDBC, where developers write SQL queries, JPA allows developers to write regular Java objects (the entities) which use annotations to define the ORM.==
Entities are typically POJOs, often Java Beans.

```
// JDBC
Connection conn = DriverManager.getConnection(url);
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM Kunden");
```


---

### 2.1.2 Using JPA Entities 

![[image/Pasted image 20250515155938.png]]


Entities are managed by the entity manager (jakarta.persistence.EntityManager)

Entity manager instances can be requested, e.g., from the Java EE runtime or the implementation of choice.

The EntityManager API is used to create and remove persistent entity instances, to find entities by their primary key, and to query over entities.

Code example:
myEntitymanager.persist(myEntityInstance);



![[400_Anwendungssystem/image/Pasted image 20250610144938.png]]

---

### 2.1.3 Querying and modifying entities: JPQL

Interaction with entities is provided by the Query and TypedQuery APIs, which accept various types of queries:
Query in drei Form schreiben: 
• Jakarta Persistence Query Language (JPQL): a fully-fledged query language with syntax similar to SQL. Uses the abstract persistence schemas of entities, including their relationships, for its data model, and it defines operators and expressions based on this data model.
• Native SQL: Forwards the textual query “as is” to the underlying database. Required if the database uses custom or non-compatible SQL.
• Criteria API: Queries are constructed by object-based query definition objects, rather than the string-based approach of the Jakarta Persistence query language Queries are executed by an EntityManager.  (nicht mehr sql, sondern OOP Sache )

==Queries are executed by an EntityManager.==
```java
@PersistenceContext
EntityManager em;

Query q = em.createQuery("select c from Customer c
    where c.name = :name")
    .setParameter("name", "Joe Smith");

Customer c = (Customer)q.getSingleResult();  // q.getSingleResult()   get Object back 
```


The query is supposed to return a single customer, so the query method getSingleResult() is used to execute the query. This method would throw an exception if there are none or more than one matching customer.



### 2.1.4 JPA 的 Annotation

#### 2.1.4.1 mappedBy

在 JPA（Java Persistence API）中，mappedBy 是用在双向关联关系中的一个关键属性，表示关系的“被拥有方”，用于指定由哪一端来维护数据库表中的外键。


为什么需要 mappedBy
- 防止 **重复维护外键**：避免两个表都试图更新外键，造成冲突。
- 由拥有方（没有 `mappedBy` 的一方）负责维护数据库中的外键列。
- 被拥有方（写了 `mappedBy`）只反映这种关系，不更新外键。

实体类 1：Student
```
@Entity
public class Student {
    @Id
    private Long id;

    @OneToMany(mappedBy = "student")  // 指的是 Enrollment 中的 student 字段
    private List<Enrollment> enrollments;
}

```
这里的 mappedBy = "student" 意思是：
由 Enrollment 实体中的 student 字段来维护这段关系，当前实体（如 Student）只是被动一方。


实体类 2：Enrollment
```
@Entity
public class Enrollment {
    @Id
    private Long id;

    @ManyToOne
    @JoinColumn(name = "student_id")  // 外键真正存在的位置
    private Student student;
}

```


| 一端            | 多端            | 是否需要 `mappedBy`                    |
| ------------- | ------------- | ---------------------------------- |
| `@OneToMany`  | `@ManyToOne`  | 是，写在 `One` 端                       |
| `@ManyToMany` | `@ManyToMany` | 一个需要 `mappedBy`，另一个不要              |
| `@OneToOne`   | `@OneToOne`   | 也是一端写 `mappedBy`，一端写 `@JoinColumn` |








### 2.1.5 例子 


![[image/Pasted image 20250515155144.png]]

![[image/Pasted image 20250515155216.png]]



Writing entity classes 
```java
@Entity
public class Customer {
    @Id
    private int id;
    private String name;



	// customer is not column name, but the field name in the related java class 
	// 代表 在 order 这个 table 中 有一个 column 名字是 customer  . 这个 column 是一个 foreign key which refers to the id column in Table Customer 
	// 在 Customer 这个 table 中 一个 column 名字叫 order . 这个 column 是一个 foreign key which refers to the id column in Table Order  
	 
    @OneToMany(mappedBy = "customer")   // 找到 Order table 中 那个 Column 和 Customer Table 有关系 
    private Collection<Order> orders;
    // + Getters and Setters
}
```




The following annotations are required for
every entity:
• @Entity: Specifies that the class is a persistence entity
• @Id: Specifies the primary key property or field of an entity

To persist the one-to-many association between the class Customer and the class Order (next slide) the following annotation is used:
• @OneToMany: Defines a many-valued association with one-to-many multiplicity.
==一个customer 有很多的order, 很多个order 来自于同一个 customer ==


• mappedBy="customer“: specifies that the field customer in class Order owns the relationship. The owner side of a relationship takes care of the foreign key column the relationship is mapped to
```
CREATE TABLE Course (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    course_id INT,
    FOREIGN KEY (course_id) REFERENCES Course(id)
);
```

---


```java
@Entity
@Table(name = "ORDER_TABLE")
public class Order {
    @Id
    @Column(name = "ORDER_ID")
    private int id;
    
    @Column(name = "SHIPPING_ADDRESS")
    private String address;
    
    @ManyToOne
    @JoinColumn(name = "CUSTOMER_ID")
    private Customer customer;
    // + Getters and Setters
}
```


• @Table: Specifies the table for the annotated entity. Default table name = class name.
• @Column: Is used to specify a mapped column for a persistent property or field. Default column name = property/field name.

• @ManyToOne: Defines a single-valued association to another entity class that has many-to-one multiplicity.
==一个customer 有很多的order, 很多个order 来自于同一个 customer ==


• @JoinColumn: Specifies the name of the foreign key (join) column (column name in this table, not another table ). Default join column name: relationship name in the owner side + “_” + name of primary key column(s) in the owned side.

• @Table, @Column and @JoinColumn are particularly useful if the database schema already exists.


![[image/Pasted image 20250515154630.png]]


n the Java entities, a bi-directional link exists.
Foreign key placement in database tables:
• One-to-one association: foreign key at any of both sides
• One-to-many association: foreign key at many-side
• Many-to-many association: additional table required


### 2.1.6 例子 

Why is `@Id` placed above the **getter** (`getVorlesungsnummer`) and not directly above the **field** (`vorlesungsnummer`)?

This is because **JPA allows two styles of access** for mapping Java classes to database tables:

1 Property Access (via Getters/Setters) — what you're using
If you put annotations like `@Id`, `@Column`, etc. **on the getters**, JPA uses **property access**. That means JPA accesses the data **via the getter/setter methods**, not the fields directly.

```
@Id
@Column(name="VorlNr", unique=true)
public int getVorlesungsnummer() { return this.vorlesungsnummer; }

```
In this case, JPA knows `vorlesungsnummer` is the primary key **because it's annotated on the getter**.




2 Field Access — alternative
If you place the annotations directly **on the fields**, like:
```
@Id
@Column(name="VorlNr", unique=true)
private int vorlesungsnummer;

```

==Then JPA will use **field access**, and it will bypass the getters/setters during ORM mapping.==

----


Important Rule:

JPA **does not mix** these two access types within a single class. The access type is determined by **where you place the first JPA annotation** (`@Id`, `@Column`, etc.) in that class:

- First annotation on a **getter** → JPA uses **property access**
    
- First annotation on a **field** → JPA uses **field access**



## 2.2 Transactions

Database Management Systems support users and application developers with the following
functions:
• Data security, data safety, data integrity,…
• Concurrent multi-user access to data

…
Transactions (tx) are essential to enable concurrent multi-user access to the data
• Transactions are an abstraction and a contract between a transactional software and users/application regarding consistency of data in the case of concurrent data access
• A transaction has the ACID properties (next slides)


### 2.2.1 ACID properties

Atomicity:
A transaction is executed completely or not at all. The effects of the executing program on the
data will only become visible if and when the transaction reaches its “commit” point. If the
executing program does not reach this point, the transaction will rollback the system to the state
at the start of the transaction.

Consistency:
A transaction leads from one consistent state to another.

Isolation:
A transaction is isolated from other transactions, in the sense that each transaction behaves as
if it were operating alone with resources to itself. Each transaction will “see” only consistent data
(only modifications that result from committed transactions).



Welche quality of seriver werden von Isolation am meisten beeinflusst 
Availabilty, Latenz 


Durability:
When the executing program is notified that a transaction has been successfully completed
(transaction commit) all updates that the transaction has made in the data are guaranteed to
survive certain failures/errors. The updated data is persisted.



### 2.2.2 What does a transaction guarantee?

With transactions, developers can be sure
• that a group of operations is either executed completely or not at all
• that a completed transaction (and its result) will never be rolled back
• that operations of multiple tenants do not interfere with each other (in terms of correctness but not in terms of performance)

And database-specific: that the integrity constraints in a database will always be preserved.
⇒ Can be guaranteed in a single-machine setup but can be costly there (as concurrent threads/processes may be blocked.
⇒ Hard to guarantee in distributed setups (only with severe performance and availability penalties)



Guaranteeing atomicity with 2-Phase-Commit (2PC)

Page 130 Anwendungssysteme
One node is designated as the coordinator (can be a
participant of the tx or can be independent)

Phase 1:
• Coordinator sends tx to participants
• Participants respond with “ready“ or “abort“

Phase 2:
• If “ready“ from all participants, coordinator sends “commit“
to all participants (else sends “rollback“)
• Participants acknowledge
⇒ Similar to (Christian) wedding
⇒ Downside: blocking protocol
⇒ Alternatives: Paxos, Raft, …


![[image/Pasted image 20250515160524.png]]

