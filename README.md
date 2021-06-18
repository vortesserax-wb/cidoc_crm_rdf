# CIDOC-CRM implementation in RDF

This project branch will keep track of draft CIDOC-CRM v7.1.1 RDF implementation 


## Statistics 

Draft release date: 14/6/2021

Version: 7.1.1	CIDOC Classes: 81 CIDOC Properties: 160 (Symmetric 4 / Transitive 13 / Reflexive 2)
- CIDOC Classes definitions in RDF: 78 (3 interpreted as rdfs:Literal)
- CIDOC Properties forward definitions in RDF: 160
- CIDOC Properties backwards definitions in RDF: 145
- RDF Triple Store extension Classes: 0
- RDF Triple Store extension Properties: 4


## RDF Generation Policies 
### Sources
Policies followed for the RDF implementation of CIDOC v.7.1.1 were created w.r.t.:
- http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf
- http://www.cidoc-crm.org/sites/default/files/cidoc_crm_v6.2.1-2018April.rdfs
- Word parsing discussions

### Policies

**A. CIDOC Classes implementation in RDF**

CIDOC-CRM model defines some classes, that in Triple stores are typically implemented as primitive values. Such Model Classes are all subclasses of E59 Primitive and in RDF they are interpreted as Literals. There are some classes that are not only subclasses of E59 Primitives and this creates a question mark as to whether they should be implemented as rdfs:Class or still be interpreted as rdfs:Literal. 
In this DRAFT version we followed the following policies regarding CIDOC Classes implementation in RDF:

> A1. CIDOC Classes that are exclusively subclasses of E59 Primitive are interpreted as rdfs:Literal. 

> A2. CIDOC Classes that are also subClasses of a Class that is not recursive subClassOf E59 Primitive are still implemented as rdfs:Class.

**B. CIDOC Properties implementation in RDF**

The decisions in policy A affect the implementation of Cidoc Properties in RDF. A property would never define an rdfs:Literal as rdfs:domain, while the backwards direction of a Property with rdfs:range an rdfs:Literal cannot be created for the exact same reason. Regarding the backwards direction of a CIDOC property there were followed different policies in previous rdf implementations depending on whether the property was defined as symmetric or not, whether it defined an inverse name in parentheses and whether property had the same class defined as both rdfs:domain and rdfs:range. 
In this DRAFT version we followed the following policies regarding CIDOC Properties implementation in RDF:

> B1. CIDOC Property forward direction is **not** defined when domain is interpreted as rdfs:Literal (A.1). Similarly, CIDOC Property backwards direction is not defined if range is interpreted as rdfs:Literal (A.1) 

> B2. CIDOC Property backwards direction is **not** defined if a) no inverse name has been specified b) domain is **different** from range

> B3. CIDOC Property backwards direction is **not** defined if a) no inverse name has been specified b) domain is the same as range c) the property is **non-symmetric**

> B4. CIDOC Property backwards direction is **replaced** by the forward direction Property if a) no inverse name has been specified b) domain is the same as range c) the property is **symmetric**


**C. RDF Triple Store extensions**

Common practice in Triple stores leads to specific extensions/addtions in Classes or Properties. More information in [Implementing the CIDOC Conceptual Reference Model in RDF](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf)
In this DRAFT version we followed the following policies regarding RDF Triple Store extensions:

> C1. addition of 2 additional subPropertiesOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive` for the exact time specification of E61 Time Primitive 

> C2. addition of 2 additional subPropertiesOf `E52 Time-Span. P82 at some time within: E61 Time Primitive` for the exact time specification of E61 Time Primitive


### Policies outcome
   
* Did not define 3 Classes due to A1:

  * `E59 Primitive`
  * `E60 Number`
  * `E62 String`  	  

Did not also define the isA relationships that these classes participate in while their references in rdfs:range of Properties were replaced by rdfs:Literal

* Did not define inverse Properties for 4 Cidoc Properties due to B1 (literal domain/range)

  * `E1 CRM Entity. P3 has note: E62 String`
  * `E19 Physical Object. P57 has number of parts: E60 Number`
  * `E52 Time-Span. P79 beginning is qualified by: E62 String`
  * `E52 Time-Span. P80 end is qualified by: E62 String`
  * `E54 Dimension. P90 has value: E60 Number`
  * `E90 Symbolic Object. P190 has symbolic content: E62 String`


* Did not define inverse Properties for 4 Cidoc Properties due to B2 (no inverse name, domain different from range)

  * `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`
  * `E52 Time-Span. P82 at some time within: E61 Time Primitive`
  * `E53 Place. P171 at some place within: E94 Space Primitive`
  * `E53 Place. P172 contains: E94 Space Primitive`


* Did not define inverse Properties for 1 Cidoc Properties due to B3 (no inverse name, domain same as range, non-symmetric)

  * `E41 Appellation. P139 has alternative form: E41 Appellation`	  


* Did not define inverse Properties for 4 Cidoc Properties and replaced with the reference to the forward direction due to B4 (no inverse name, domain same as range, symmetric)

  * `E53 Place. P121 overlaps with: E53 Place`
  * `E53 Place. P122 borders with: E53 Place`
  * `E92 Spacetime Volume. P132 spatiotemporally overlaps with: E92 Spacetime Volume`
  * `E92 Spacetime Volume. P133 is spatiotemporally separated from: E92 Spacetime Volume`


* Added the following 4 RDF property definitions due to C1 and C2

  * `E52 Time-Span. P81a end of the begin: xsd:datetime` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  * `E52 Time-Span. P81b begin of the end: xsd:datetime` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  * `E52 Time-Span. P82a begin of the begin: xsd:datetime` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  * `E52 Time-Span. P82b end of the end: xsd:datetime` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)




   
