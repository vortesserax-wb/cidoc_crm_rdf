# CIDOC-CRM implementation in RDF

This project branch will keep track of draft CIDOC-CRM v7.1.1 RDF implementation

## Statistics

Draft release date: 10/7/2021

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

#### **E. Translations and Scope notes**

> E1. **Translations** Each RDF Class or Property definition is accompanied by a set of translations expressed as rdfs:labels separated by xml:lang tags. The translation label used for each language is **the most recent** still **valid** translation. Still valid translation may be interpreted in the following ways:

- The English label of the `CurrentVersion` that this rdf is refferring to is *almost Equal* (accepting only differences regarding multiple spaces) to the English label of the `TranslationVersion` that the translated label was created for.
E.g. In order to accept a translation for `E22 Human-Made Object` in CurrentVersion (7.1.1) we should have a tranlation for E22 that was created based on a CIDOC version that used the same name for E22. E22 class name changed from `E22 Man-Made Object` to `E22 Human-Made Object` in version 6.2.7 so the only **valid** tranlsations would be these that were created since 6.2.7. Currently no such translation has been created so `E22 Human-Made Object` in CurrentVersion (7.1.1) does not define any translation in current RDF File.

- Another option is to qualify translations as **valid** based on the scope note comparison of `CurrentVersion` and `TranslationVersion`. Since scope note includes formatting info the comparison should ingnore formatting changes.

Currently in this version we follow the option to qualify translations as valid based on the comparison of their name in the relevant cidoc versions.

&nbsp;

> E2. **Scope notes** are defined as rdfs:comment including just the text of the official CIDOC-CRM version in English language. Since scope note is not just simple text but includes formatting, simple removal of formatting does not always produce a readable format. Thus we followed the following simple conventions for the transformation of CIDOC-CRM scope note text in a readable rdfs:comment:

- Each Paragrah defines a new line
- Each element in an unordered list defines a new line
- Each element in an ordered list defines a new line prefixed with `'{order}. '`
- Each superscript text is separated from its base with `'-'`
- Each subscript text is separated from its base with `'_'` (none found)

&nbsp;

> E3. **Scope notes of Inverse Properties**. Scope notes for the inverse/backwards direction of Properties is not defined as it is covered by the definition of the direct/forward direction definition. Whenever the direct/forward property direction of `Pxyz` is not defined due to **B1**, then its scope note, is used as the scope note of the inverse/backwards property `Pxyzi` and prefixed with `Scope note for 'Pxyz':`.

&nbsp;

### Policies outcome

Following the aforementioned policies resulted in the following RDF generation decisions:

&nbsp;

- Did not define 6 Classes due to **A1**:

  - `E59 Primitive`
  - `E60 Number`
  - `E61 Time Primitive`
  - `E62 String`
  - `E94 Space Primitive`
  - `E95 Spacetime Primitive`

     Did not also define the isA relationships that these classes participate in while their references in rdfs:range of Properties were replaced by rdfs:Literal

&nbsp;

- Did not define direct/forward Properties for 2 Cidoc Properties due to **B1** (literal domain/range)

  - `E95 Spacetime Primitive. P169 defines spacetime volume: E92 Spacetime Volume`
  - `E61 Time Primitive. P170 defines time: E52 Time-Span`

     **Note:** scope notes of P169 and P170 that cannot be defined are used as scope notes of `P169i` and `P170i` with the prefix defined in **B1**.  

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

  - `E52 Time-Span. P81a end of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  - `E52 Time-Span. P81b begin of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
  - `E52 Time-Span. P82a begin of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  - `E52 Time-Span. P82b end of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
  - `E54 Dimension. P90a has lower value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)
  - `E54 Dimension. P90b has upper value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)  

&nbsp;

- Added the following 1 RDF class definition due to **D1**

  - `E41_E33_Linguistic_Appellation` subClass of `E41_Appellation` and `E33_Linguistic_Object`
