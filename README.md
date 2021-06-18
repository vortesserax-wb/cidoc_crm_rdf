# CIDOC-CRM implementation in RDF

This project branch will keep track of draft CIDOC-CRM v7.1.1 RDF implementation 


## Statistics 

Draft release date: 18/6/2021

Version: 7.1.1	CIDOC Classes: 81 CIDOC Properties: 160 (Symmetric 4 / Transitive 13 / Reflexive 2)
- Classes interpreted as Literals: 75 (6 interpreted as rdfs:Literal)
- CIDOC Properties forward definitions in RDF: 158
- CIDOC Properties backwards definitions in RDF: 149
- RDF Triple Store extension Classes: 1
- RDF Triple Store extension Properties: 6

## RDF Generation Policies 
### Sources
Policies followed for the RDF implementation of CIDOC v.7.1.1 were created w.r.t.:
- http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf
- CIDOC-CRM SIG feedback and the general guidelines:
  - Each property is declared twice: forward and backwards
  - All Primitive Values become rdf literal
  - All IsA declarations from/to and between Primitive values are ignored


### Policies

**A. CIDOC Classes implementation in RDF**

CIDOC-CRM model defines some classes, that in Triple stores are typically implemented as primitive values. Such Model Classes are all subclasses of `E59 Primitive` and in RDF they are interpreted as Literals. There are some classes that are not only subclasses of `E59 Primitive` and this creates a question mark on whether they should be implemented as rdfs:Class or still be interpreted as rdfs:Literal. 
In this DRAFT version we followed the following policies regarding CIDOC Classes implementation in RDF:

> A1. CIDOC Classes that are subClasses of `E59 Primitive` are interpreted as rdfs:Literal regardless of whether they are also subClasses of another Class. 


**B. CIDOC Properties implementation in RDF**

The decisions in policy A affect the implementation of Cidoc Properties in RDF. A property would never define an rdfs:Literal as rdfs:domain, while the backwards direction of a Property with an rdfs:Literal as rdfs:range cannot be created for the exact same reason. 
In this DRAFT version we followed the following policies regarding CIDOC Properties implementation in RDF:

> B1. CIDOC Property forward direction is **not** defined when domain is interpreted as rdfs:Literal (A.1). Similarly, CIDOC Property backwards direction is not defined if range is interpreted as rdfs:Literal (A.1). In case forward direction of `Pxyz` is not defined but the backwards direction `Pxyzi` is defined then scope note of the forward direction is used as the scope note of `Pxyzi` and prefixed with `Scope note for 'Pxyz':`


> B2. CIDOC Property backwards direction **is defined** using the forward property name and the `i` identifier suffix if a) no inverse name has been specified. Previous exceptions followed in [Rdf v6.2.1](http://www.cidoc-crm.org/sites/default/files/cidoc_crm_v6.2.1-2018April.rdfs) depending on whether property is symmetric or not have been removed.


**C. RDF Triple Store extensions**

Common practice in Triple stores leads to specific extensions/addtions in Classes or Properties. More information in [Implementing the CIDOC Conceptual Reference Model in RDF](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf)
In this DRAFT version we followed the following policies regarding RDF Triple Store extensions:

> C1. addition of 2 additional subPropertiesOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

> C2. addition of 2 additional subPropertiesOf `E52 Time-Span. P82 at some time within: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

> C3. addition of 2 additional subPropertiesOf `E54 Dimension. P90 has value: E60 Number` for the upper and lower boundaries specification of `E60 Number`.

> C4. addition of 1 additional subClass of `E41 Appellation` and `E33 Linguistic Object` for all Appellations being regarded
specific to or characteristic for a language group and being directly described by a literal and not indirectly via a URI.

### Policies outcome
   
* Did not define 6 Classes due to A1:

  * `E59 Primitive`
  * `E60 Number`
  * `E61 Time Primitive`
  * `E62 String`
  * `E94 Space Primitive`
  * `E95 Spacetime Primitive`   

Did not also define the isA relationships that these classes participate in while their references in rdfs:range of Properties were replaced by rdfs:Literal

* Did not define direct/forward Properties for 2 Cidoc Properties due to B1 (literal domain/range)
  * `E95 Spacetime Primitive. P169 defines spacetime volume: E92 Spacetime Volume`
  * `E61 Time Primitive. P170 defines time: E52 Time-Span`

**Note:** scope notes of P169 and P170 that cannot be defined are used as scope notes of P169i and P170i with the prefix defined in B1.
  
* Did not define inverse/backwards Properties for 11 Cidoc Properties due to B1 (literal domain/range)

  * `E1 CRM Entity. P3 has note: E62 String`
  * `E19 Physical Object. P57 has number of parts: E60 Number`
  * `E52 Time-Span. P79 beginning is qualified by: E62 String`
  * `E52 Time-Span. P80 end is qualified by: E62 String`
  * `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`
  * `E52 Time-Span. P82 at some time within: E61 Time Primitive`  
  * `E54 Dimension. P90 has value: E60 Number`
  * `E53 Place. P168 place is defined by: E94 Space Primitive`
  * `E53 Place. P171 at some place within: E94 Space Primitive`
  * `E53 Place. P172 contains: E94 Space Primitive`
  * `E90 Symbolic Object. P190 has symbolic content: E62 String`


* Used the direct/forward name for the definition of 5 inverse properties due to B2 (no inverse name defined)
  * `E53 Place. P121i overlaps with: E53 Place`
  * `E53 Place. P122i borders with: E53 Place`
  * `E92 Spacetime Volume. P132i spatiotemporally overlaps with: E92 Spacetime Volume`
  * `E92 Spacetime Volume. P133i is spatiotemporally separated from: E92 Spacetime Volume`
  * `E41 Appellation. P139i has alternative form: E41 Appellation`	  


* Added the following 6 RDF property definitions due to C1, C2 and C3

  * `E52 Time-Span. P81a end of the begin: xsd:datetime` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  * `E52 Time-Span. P81b begin of the end: xsd:datetime` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  * `E52 Time-Span. P82a begin of the begin: xsd:datetime` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  * `E52 Time-Span. P82b end of the end: xsd:datetime` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  * `E54 Dimension. P90a has lower value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)
  * `E54 Dimension. P90b has upper value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)  


* Added the following 1 RDF class definition due to C4
  * `E41_E33_Linguistic_Appellation` subClass of `E41_Appellation` and `E33_Linguistic_Object`