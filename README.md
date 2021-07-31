# CIDOC-CRM implementation in RDF Schema 1.1 (RDFS)

This project branch will keep track of the draft RDFS implementation of the CIDOC-CRM v7.1.1 

## Statistics

Draft release date: 14/7/2021

Version: 7.1.1 CIDOC Classes: 81 CIDOC Properties: 160 (Symmetric 4 / Transitive 13 / Reflexive 2)

- CIDOC Classes definitions in RDFS: 75 (another 6 interpreted as rdfs:Literal)
- CIDOC Properties forward definitions in RDFS: 158
- CIDOC Properties backwards definitions in RDFS: 144
- RDFS Implementation of Value Interval Properties: 6
- RDFS Multiple ISA custom declaration Classes: 1

## RDFS Generation Policies

### Sources

Policies followed for the RDFS implementation of CIDOC v.7.1.1 were created w.r.t.:

- [Implementing the CIDOC Conceptual Reference Model in RDF](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf)
- CIDOC-CRM SIG feedback and the general guidelines:
  - Each property is declared twice: forward and backwards
  - All Primitive Values become rdfs Literal
  - All IsA declarations from/to and between Primitive values are ignored

### Policies

&nbsp;

#### **A. CIDOC Classes implementation in RDFS**

The CIDOC-CRM model defines classes, that in Triple Stores are typically implemented as primitive values. These classes are all subclasses of `E59 Primitive` and in RDFS they are interpreted as Literal. There are some classes in the CIDOC-CRM model that are both subclasses of `E59 Primitive` and E41 Appellation. The question whether they should be implemented as rdfs:Class or still be interpreted as rdfs:Literal was decided by CRM-SIG in "Issue 394: Solution for Dualism of E41 Appellation and rdfs:label" in 28-11-2018.
In this version we have followed the following policy regarding CIDOC Classes implementation in RDFS:

&nbsp;

> A1. CIDOC Classes that are subClasses of `E59 Primitive` are interpreted as rdfs:Literal regardless of whether they are also subClasses of another Class.

As a result, the following 6 CIDOC classes were not defined in RDFS:

- `E59 Primitive`
- `E60 Number`
- `E61 Time Primitive`
- `E62 String`
- `E94 Space Primitive`
- `E95 Spacetime Primitive`

Additionally, the following isA relationships were not defined in RDFS:

- `E59 Primitive` subClassOf `E1 CRM Entity`
- `E60 Number` subClassOf `E59 Primitive`
- `E61 Time Primitive` subClassOf `E59 Primitive`, `E41 Appellation`
- `E62 String` subClassOf `E59 Primitive`
- `E94 Space Primitive` subClassOf `E59 Primitive`, `E41 Appellation`
- `E95 Spacetime Primitive` subClassOf `E59 Primitive`, `E41 Appellation`

&nbsp;

#### **B. CIDOC Properties implementation in RDFS**

The decisions in policy A affect the implementation of Cidoc Properties in RDFS. A property cannot define an rdfs:Literal as rdfs:domain, while the backwards direction of a property with an rdfs:Literal as rdfs:range cannot be created for the exact same reason. Further, the inherited properties of these classes are similarly affected. In Cidoc Model a definition like `class1 subClassOf class2` specifies that `class1` can also define all properties with domain `class2` (similarly for inverse properties). If class `class1` is implemented as rdfs:Literal then it cannot also define the properties of `class2`.

Another aspect that should be considered during the implementation of properties in RDFs is whether a distinct inverse property definition should be implemented.

In this version we followed the following policies regarding CIDOC Properties implementation in RDFS:

> B1. CIDOC Property backwards/inverse direction is **not** defined if range is interpreted as rdfs:Literal (A.1). Similarly, CIDOC Property forward/direct direction is **not** defined when domain is interpreted as rdfs:Literal (A.1).

&nbsp;

As a result, the following 2 direct/forward CIDOC Property directions were not defined:

- `E95 Spacetime Primitive. P169 defines spacetime volume: E92 Spacetime Volume`
- `E61 Time Primitive. P170 defines time: E52 Time-Span`

Additionally, the following 11 inverse/backwards CIDOC Property directions were not defined:

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

Additionally, the following **inherited** properties cannot be expressed for all classes included in Policy A. (i.e. in this version `E59 Primitive`, `E60 Number`, `E61 Time Primitive`, `E62 String`, `E94 Space Primitive`, `E95 Spacetime Primitive`)

- Properties inherited from `E1 CRM Entity`
  - `P1 is identified by (identifies): E41 Appellation`
  - `P2 has type (is type of): E55 Type`
  - `P3 has note: E62 String`
  - `P48 has preferred identifier (is preferred identifier of): E42 Identifier`
  - `P137 exemplifies (is exemplified by): E55 Type`

Additionally, the following **inherited** properties cannot be expressed for classes included in Policy A that are also subClassOf another Cidoc class.  (i.e. in this version `E61 Time Primitive`, `E94 Space Primitive`, `E95 Spacetime Primitive` that are all subClassOf `E41 Appellation`)

- Properties inherited from `E70 Thing`
  - `P43 has dimension (is dimension of): E54 Dimension`
  - `P101 had as general use (was use of): E55 Type`
  - `P130 shows features of (features are also found on): E70 Thing`
- Properties inherited from `E71 Human-Made Thing`
  - `P102 has title (is title of): E35 Title`
  - `P103 was intended for (was intention of): E55 Type`
- Properties inherited from `E72 Legal Object`
  - `P104 is subject to (applies to): E30 Right`
  - `P105 right held by (has right on): E39 Actor`
- Properties inherited from `E90 Symbolic Object`
  - `P106 is composed of (forms part of): E90 Symbolic Object`
  - `P190 has symbolic content: E62 String`
- Properties inherited from `E41 Appellation`
  - `P139 has alternative form: E41 Appellation`
  - `P139 has alternative form: E41 Appellation`

&nbsp;

> B2. Whenever no backwards/inverse property name is specified and property domain matches with property range, then the forward/direct property should be used for **both** directions and **no inverse/backwards property** needs to be defined.

As a result, did not implement a separate backwards/inverse direction for the following 5 CIDOC Properties:

- `E53 Place. P121 overlaps with: E53 Place`
- `E53 Place. P122 borders with: E53 Place`
- `E92 Spacetime Volume. P132 spatiotemporally overlaps with: E92 Spacetime Volume`
- `E92 Spacetime Volume. P133 is spatiotemporally separated from: E92 Spacetime Volume`
- `E41 Appellation. P139 has alternative form: E41 Appellation`

&nbsp;

Additionally, all aforementioned redundant backwards/inverse CIDOC Property direction references, were replaced by the respective direct/forward CIDOC Property direction.

- The hierarchical link of  `P10i` subPropertyOf `P132i` was replaced with `P10i` subPropertyOf `P132`.

&nbsp;

#### **C. Labels and appellations**

Since, in current practice, the property `rdfs:label` is used to denote appellations, the following `subPropertyOf` relation was defined:

- `rdfs:Resource. rdfs:label: rdfs:Literal` subPropertyOf `E1 CRM Entity. P1 is identified by: E41 Appellation`

Note that according to the RDF Schema 1.1 specification, the domain (or range) of a property is not necessarily 'subClassOf' or 'equal' to the domain (or range) of its superproperties. With this addition, one can either directly use `rdfs:label` for providing an identifier/appellation for an instance of `E1 CRM Entity`, or use `P1 is identified by` and an intermediate node (i.e. an instance of `E41_Appellation`) for providing additional information about the appellation.  

&nbsp;

#### **D. Implementing Value Intervals**

The following are property pairs needed to simulate interval-valued primitive values foreseen by the CRM:

> D1. addition of 2 additional subPropertiesOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

As a result, created the following property definitions:

- `E52 Time-Span. P81a end of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
- `E52 Time-Span. P81b begin of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)

&nbsp;

> D2. addition of 2 additional subPropertiesOf `E52 Time-Span. P82 at some time within: E61 Time Primitive` for the exact time specification of `E61 Time Primitive`.

As a result, created the following property definitions:

- `E52 Time-Span. P82a begin of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
- `E52 Time-Span. P82b end of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)

&nbsp;

> D3. addition of 2 additional subPropertiesOf `E54 Dimension. P90 has value: E60 Number` for the upper and lower boundaries specification of `E60 Number`.

As a result, created the following property definitions:

- `E54 Dimension. P90a has lower value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)
- `E54 Dimension. P90b has upper value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)

&nbsp;

#### **E. Custom Multiple ISA declarations**

The following are declarations of frequently used multiple instantiations as classes with respective multiple IsA:

> E1. addition of 1 additional subClassOf `E41 Appellation` and `E33 Linguistic Object` for all Appellations being regarded
specific to or characteristic for a language group and being directly described by a literal and not indirectly via a URI.

&nbsp;

As a result, created the following class definition

- `E41_E33_Linguistic_Appellation` subClass of `E41_Appellation` and `E33_Linguistic_Object`

&nbsp;

#### **F. Translations and Scope notes**

> F1. **Translations** Each RDFS Class or Property definition is accompanied by a set of translations expressed as rdfs:labels separated by xml:lang tags. The translation label used for each language is **the most recent** still **valid** translation. Still valid translation may be interpreted in the following ways:

- The English label of the `CurrentVersion` that current rdfs file is referring to is *almost Equal* (accepting only differences regarding multiple spaces) to the English label of the `TranslationVersion` that the translated label was created for.
E.g. In order to accept a translation for `E22 Human-Made Object` in CurrentVersion (7.1.1) we should have a translation for E22 that was created based on a CIDOC version that used the same name for E22. E22 class name changed from `E22 Man-Made Object` to `E22 Human-Made Object` in version 6.2.7 so the only **valid** translations would be these that were created since 6.2.7. Currently no such translation has been created so `E22 Human-Made Object` in CurrentVersion (7.1.1) does not define any translation in current RDFS File.

- Another option is to qualify translations as **valid** based on the scope note comparison of `CurrentVersion` and `TranslationVersion`. Since scope note includes formatting info the comparison should ignore formatting changes.

Currently in this version we follow the option to qualify translations as valid based on the comparison of their name in the relevant CIDOC versions.

&nbsp;

> F2. **Scope notes** are defined as rdfs:comment including just the text of the official CIDOC-CRM version in English language. Since scope note is not just simple text but includes formatting, simple removal of formatting does not always produce a readable format. Thus we followed the following simple conventions for the transformation of CIDOC-CRM scope note text in a readable rdfs:comment:

- Each Paragraph defines a new line
- Each element in an unordered list defines a new line
- Each element in an ordered list defines a new line prefixed with `'{order}. '`
- Each superscript text is separated from its base with `'-'`
- Each subscript text is separated from its base with `'_'` (none found)

&nbsp;

> F3. **Scope notes of Inverse Properties**. Scope notes for the inverse/backwards direction of Properties is not defined as it is covered by the definition of the direct/forward direction definition. Whenever the direct/forward property direction of `Pxyz` is not defined due to **B1**, then its scope note, is used as the scope note of the inverse/backwards property `Pxyzi` and prefixed with `Scope note for 'Pxyz':`.

As a result, the direct/forward property definition was used for the rdfs:comment definition of

- `E92 Spacetime Volume. P169i spacetime volume is defined by: rdfs:Literal`
- `E52 Time-Span. P170i time is defined by: rdfs:Literal`
