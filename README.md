# CIDOC-CRM implementation in RDF

This project branch will keep track of draft CIDOC-CRM v7.1.1 RDF implementation

## Statistics

Draft release date: 25/6/2021

Version: 7.1.1 CIDOC Classes: 81 CIDOC Properties: 160 (Symmetric 4 / Transitive 13 / Reflexive 2)

- CIDOC Classes definitions in RDF: 75 (6 interpreted as rdfs:Literal)
- CIDOC Properties forward definitions in RDF: 158
- CIDOC Properties backwards definitions in RDF: 144
- RDF Custom Multiple Isa declaration Classes: 1
- RDF Implemention of Value Interval Properties: 6

## RDF Generation Policies

### Sources

Policies followed for the RDF implementation of CIDOC v.7.1.1 were created w.r.t.:

- [Implementing the CIDOC Conceptual Reference Model in RDF](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf)
- CIDOC-CRM SIG feedback and the general guidelines:
  - Each property is declared twice: forward and backwards
  - All Primitive Values become rdf literal
  - All IsA declarations from/to and between Primitive values are ignored

### Policies

&nbsp;

#### **A. CIDOC Classes implementation in RDF**

CIDOC-CRM model defines some classes, that in Triple stores are typically implemented as primitive values. Such Model Classes are all subclasses of `E59 Primitive` and in RDF they are interpreted as Literals. There are some classes that are not only subclasses of `E59 Primitive` and this creates a question mark on whether they should be implemented as rdfs:Class or still be interpreted as rdfs:Literal.
In this DRAFT version we followed the following policies regarding CIDOC Classes implementation in RDF:

> A1. CIDOC Classes that are subClasses of `E59 Primitive` are interpreted as rdfs:Literal regardless of whether they are also subClasses of another Class. 

&nbsp;

#### **B. CIDOC Properties implementation in RDF**

The decisions in policy A affect the implementation of Cidoc Properties in RDF. A property would never define an rdfs:Literal as rdfs:domain, while the backwards direction of a Property with an rdfs:Literal as rdfs:range cannot be created for the exact same reason. 
In this DRAFT version we followed the following policies regarding CIDOC Properties implementation in RDF:

> B1. CIDOC Property backwards/inverse direction is **not** defined if range is interpreted as rdfs:Literal (A.1). Similarly, CIDOC Property forward/direct direction is **not** defined when domain is interpreted as rdfs:Literal (A.1). 

>> B1a. In the latter case, the scope note of such properties `Pxyz`, is transfered to the scope note definition of the inverse property `Pxyzi` and prefixed with `Scope note for 'Pxyz':`.

>> B1b. Following the guidelines from [`RDF implementation tests`](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf) {1,2} properties with range interpreted as Literal (A.1) are all declared as subproperties of `rdfs:label` while their superproperties should define an additional range of `rdfs:Literal` whenever their range is a class not included in A.1. Similarly for the inverse/backwards direction.

> B2. Whenever no backwards/inverse property name is specified and property domain matches with property range, then the forward/direct property should be used for **both** directions and **no inverse/backwards property** needs to be defined.

&nbsp;

#### **C. Implementing Value Intervals**

The following are property pairs needed to simulate interval-valued primitive values foreseen by the CRM:

> C1. addition of 2 additional subPropertiesOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

> C2. addition of 2 additional subPropertiesOf `E52 Time-Span. P82 at some time within: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

> C3. addition of 2 additional subPropertiesOf `E54 Dimension. P90 has value: E60 Number` for the upper and lower boundaries specification of `E60 Number`.

&nbsp;

#### **D. Custom Multiple Isa declarations**

The following are declarations of frequently used multiple instatiations as classes with respective multiple IsA:

> D1. addition of 1 additional subClass of `E41 Appellation` and `E33 Linguistic Object` for all Appellations being regarded
specific to or characteristic for a language group and being directly described by a literal and not indirectly via a URI.

&nbsp;

### Policies outcome

Following the aforementioned policies resulted in the following RDF generation decisions:

&nbsp;

- Did not define 6 Classes due to **A1**:

  - `E59 Primitive`
  - `E60 Number` when used as property range, mapped to `xsd:integer` (as example datatype) and `rdfs:Literal` (in general)
  - `E61 Time Primitive` when used as property range, mapped to `xsd:dateTime` (as example datatype) and `rdfs:Literal` (in general)
  - `E62 String` when used as property range, mapped to `xsd:string` (as example datatype) and `rdfs:Literal` (in general)
  - `E94 Space Primitive`
  - `E95 Spacetime Primitive`

     Did not also define the isA relationships that these classes participate in while their references in rdfs:range of Properties were replaced by rdfs:Literal

&nbsp;

- Did not define direct/forward Properties for 2 Cidoc Properties due to **B1** (literal domain/range)

  - `E95 Spacetime Primitive. P169 defines spacetime volume: E92 Spacetime Volume`
  - `E61 Time Primitive. P170 defines time: E52 Time-Span`

     **Note:** scope notes of P169 and P170 that cannot be defined are used as scope notes of `P169i` and `P170i` with the prefix defined in **B1a**.  

&nbsp;

- Did not define inverse/backwards Properties for 11 Cidoc Properties due to **B1** (literal domain/range)

  - `E1 CRM Entity. P3 has note: E62 String`
  - `E19 Physical Object. P57 has number of parts: E60 Number`
  - `E52 Time-Span. P79 beginning is qualified by: E62 String`
  - `E52 Time-Span. P80 end is qualified by: E62 String`
  - `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`
  - `E52 Time-Span. P82 at some time within: E61 Time Primitive`  
  - `E54 Dimension. P90 has value: E60 Number`
  - `E53 Place. P168 place is defined by: E94 Space Primitive`
  - `E53 Place. P171 at some place within: E94 Space Primitive`
  - `E53 Place. P172 contains: E94 Space Primitive`
  - `E90 Symbolic Object. P190 has symbolic content: E62 String`

&nbsp;

- Did not define the backwards/inverse direction for 5 properties due to **B2** (no inverse name defined, and same domain/range)

  - `E53 Place. P121 overlaps with: E53 Place`
  - `E53 Place. P122 borders with: E53 Place`
  - `E92 Spacetime Volume. P132 spatiotemporally overlaps with: E92 Spacetime Volume`
  - `E92 Spacetime Volume. P133 is spatiotemporally separated from: E92 Spacetime Volume`
  - `E41 Appellation. P139 has alternative form: E41 Appellation`

&nbsp;

- Used the backwards/inverse direction in place of the redundant inverse one due to **B2** (no inverse name defined, and same domain/range)

  - Hierarchical link of property `P10i` with `E92 Spacetime Volume. P132 spatiotemporally overlaps with: E92 Spacetime Volume` instead of the inverse one P132i.

&nbsp;

- Added the following 6 RDF property definitions due to **C1, C2 and C3**

  - `E52 Time-Span. P81a end of the begin: xsd:dateTime & rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  - `E52 Time-Span. P81b begin of the end: xsd:dateTime & rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  - `E52 Time-Span. P82a begin of the begin: xsd:dateTime & rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  - `E52 Time-Span. P82b end of the end: xsd:dateTime & rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  - `E54 Dimension. P90a has lower value limit: xsd:integer & dfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)
  - `E54 Dimension. P90b has upper value limit: xsd:integer & rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)  

&nbsp;

- Added the following 1 RDF class definition due to **D1**

  - `E41_E33_Linguistic_Appellation` subClass of `E41_Appellation` and `E33_Linguistic_Object` and `rdf:langString`

&nbsp;

- Defined the following as subclasses of `rdfs:label` due to **B1b**

  - `E1 CRM Entity. P1 is identified by: E41 Appellation`
  - `E1 CRM Entity. P3 has note: E62 String`
  - `E19 Physical Object. P57 has number of parts: E60 Number`
  - `E52 Time-Span. P79 beginning is qualified by: E62 String`
  - `E52 Time-Span. P80 end is qualified by: E62 String`
  - `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`
  - `E52 Time-Span. P82 at some time within: E61 Time Primitive`  
  - `E54 Dimension. P90 has value: E60 Number`
  - `E53 Place. P168 place is defined by: E94 Space Primitive`
  - `E92 Spacetime Volume. P169i spacetime volume is defined by: E95 Spacetime Primitive`
  - `E52 Time-Span Primitive. P170i time is defined by: E61 Time`
  - `E53 Place. P171 at some place within: E94 Space Primitive`
  - `E53 Place. P172 contains: E94 Space Primitive`
  - `E90 Symbolic Object. P190 has symbolic content: E62 String`
  - `E52 Time-Span. P81a end of the begin: xsd:dateTime & rdfs:Literal`
  - `E52 Time-Span. P81b begin of the end: xsd:dateTime & rdfs:Literal`
  - `E52 Time-Span. P82a begin of the begin: xsd:dateTime & rdfs:Literal`
  - `E52 Time-Span. P82b end of the end: xsd:dateTime & rdfs:Literal`
  - `E54 Dimension. P90a has lower value limit: xsd:integer & dfs:Literal`
  - `E54 Dimension. P90b has upper value limit: xsd:integer & rdfs:Literal`

&nbsp;

- Added the following additional range to `rdfs:Literal` due to **B1b** 

  - `E1 CRM Entity. P1 is identified by: E41 Appellation & rdfs:Litereal`
