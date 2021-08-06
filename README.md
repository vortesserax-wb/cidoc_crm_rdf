# CIDOC-CRM implementation in RDF Schema 1.1 (RDFS)

This project branch will keep track of the draft RDFS implementation of the CIDOC-CRM v7.1.1 

## Statistics

Draft release date: 14/7/2021

Version: 7.1.1 CIDOC Classes: 81 CIDOC Properties: 160

- CIDOC Classes definitions in RDFS: 75 (another 6 interpreted as rdfs:Literal)
- CIDOC Properties forward definitions in RDFS: 158
- CIDOC Properties backwards definitions in RDFS: 144
- RDFS Implementation of Value Interval Properties: 6
- RDFS Multiple ISA custom declaration Classes: 1

## RDFS Generation Policies

### Sources

Policies followed for the RDFS implementation of CIDOC v.7.1.1 were created w.r.t.:

- [Implementing the CIDOC Conceptual Reference Model in RDF](http://www.cidoc-crm.org/Resources/implementing-the-cidoc-conceptual-reference-model-in-rdf)
- [How to implement CRM Time in RDF](http://old.cidoc-crm.org/docs/How_to%20implement%20CRM_Time_in%20RDF.pdf)
- CIDOC-CRM SIG feedback and the general guidelines:
  - Each property is declared twice, forward and backwards, unless no inverse name is defined in parentheses, or domain or range is interpreted as literal
  - All Primitive Values become rdfs:Literal
  - All IsA declarations from/to and between Primitive values are ignored

### Policies

&nbsp;

#### **A. CIDOC Classes implementation in RDFS**

The CIDOC-CRM model defines classes, that in Triple Stores are typically implemented as primitive values. These classes are all subclasses of `E59 Primitive` and in RDFS they are interpreted as Literal. There are some classes in the CIDOC-CRM model that are both subclasses of `E59 Primitive` and E41 Appellation. So far we have followed the decision by CRM-SIG in "Issue 394: Solution for Dualism of E41 Appellation and rdfs:label" in 28-11-2018 wrt the question whether these classes should be implemented as rdfs:Class or still be interpreted as rdfs:Literal.

In this version we have followed the following policy regarding CIDOC Classes implementation in RDFS:

&nbsp;

> A1. CIDOC Classes that are subClasses of `E59 Primitive` are interpreted as rdfs:Literal regardless of whether they are also subClasses of another Class.

As a result, the following CIDOC classes were not defined in RDFS:

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

The decisions in policy A affect the implementation of CIDOC Properties in RDFS. A property cannot define an rdfs:Literal as rdfs:domain, and the backwards direction of a property with an rdfs:Literal as rdfs:range cannot be created for the exact same reason.

Further, the property ranges defined as rdfs:Literal in this implementation do not inherit the properties of the superclasses defined in the CIDOC Model. A separate guideline will describe how to instantiate such properties if at all needed.

Another aspect considered for the implementation of properties in RDFS is whether a distinct inverse property definition should be provided.

In this version we followed the following policies regarding CIDOC Properties implementation in RDFS:

> B1. CIDOC Property backwards/inverse direction is **not** defined, whenever no inverse name for the property has been specified in parentheses in the official CIDOC documentation and property domain is not equal with property range.

&nbsp;

As a result, the following properties do not define an inverse direction.

- `E1 CRM Entity. P3 has note: E62 String`
- `E19 Physical Object. P57 has number of parts: E60 Number`
- `E52 Time-Span. P79 beginning is qualified by: E62 String`
- `E52 Time-Span. P80 end is qualified by: E62 String`
- `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`
- `E52 Time-Span. P82 at some time within: E61 Time Primitive`
- `E54 Dimension. P90 has value: E60 Number`
- `E53 Place. P171 at some place within: E94 Space Primitive`
- `E53 Place. P172 contains: E94 Space Primitive`
- `E90 Symbolic Object. P190 has symbolic content: E62 String`

> B2. CIDOC Property forward direction is **not** defined whenever property domain is interpreted as rdfs:Literal (A.1). Similarly, CIDOC Property backwards/inverse direction is **not** defined, whenever property range is interpreted as rdfs:Literal (A.1). Further, the instances of classes interpreted as rdfs:Literal, cannot express in RDFs their **inherited** properties that could be expressed according to CIDOC Model.

&nbsp;

As a result,

a) The following **direct forward direction** CIDOC Properties **cannot** be expressed and are **not defined** in RDFS:

- `E95 Spacetime Primitive. P169 defines spacetime volume (spacetime volume is defined by): E92 Spacetime Volume`
- `E61 Time Primitive. P170 defines time (time is defined by): E52 Time-Span`

&nbsp;

b) The following **direct backwards direction** CIDOC Properties **cannot** be expressed and are **not defined** in RDFS:

- `E94 Space Primitive. P168i defines place (place is defined by): E53 Place`

&nbsp;

> B3. Whenever no backwards/inverse property name is specified and property domain matches with property range, then the forward/direct property direction should be used for **both** directions and **no backwards/inverse property** needs to be defined.

As a result, we did not implement a separate backwards/inverse direction for the following CIDOC Properties:

- `E53 Place. P121 overlaps with: E53 Place`
- `E53 Place. P122 borders with: E53 Place`
- `E92 Spacetime Volume. P132 spatiotemporally overlaps with: E92 Spacetime Volume`
- `E92 Spacetime Volume. P133 is spatiotemporally separated from: E92 Spacetime Volume`
- `E41 Appellation. P139 has alternative form: E41 Appellation`

&nbsp;

Additionally, all aforementioned redundant backwards/inverse CIDOC Property direction references, are replaced by the respective direct/forward CIDOC Property direction.

- The hierarchical link of  `P10i` subPropertyOf `P132i` was replaced with `P10i` subPropertyOf `P132`.

&nbsp;

#### **C. Labels and appellations**

Since, in the current practice, the property `rdfs:label` is widely used to denote appellations, the following `subPropertyOf` relation was defined according to [CRM-SIG Issue 394](http://cidoc-crm.org/Issue/ID-394-solution-for-dualism-of-e41-appellation-and-rdfslabel):

- `rdfs:Resource. rdfs:label: rdfs:Literal` subPropertyOf `E1 CRM Entity. P1 is identified by: E41 Appellation`

Note that according to the RDF Schema 1.1 specification, the domain (or range) of a property needs not necessarily be 'subClassOf' or 'equal' to the domain (or range) of its superproperties. With this definition, one can either directly use `rdfs:label` for providing a non-URI identifier/appellation for an instance of `E1 CRM Entity`, or use `P1 is identified by` for associating an additional URI-identifier with an instance of `E1 CRM Entity`, or use `P1 is identified by` and create an intermediate node (i.e. a URI-identifier for instance of `E41_Appellation`) for providing more detailed information about this appellation. A separate guideline will describe how to instantiate these cases if at all needed.


&nbsp;

#### **D. Implementing Value Intervals**

The following are property pairs needed to simulate the interval-valued primitive values foreseen by the CRM:

> D1. addition of two new properties, defined as subproperties of `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`, for the 'maximum known temporal extent' boundaries specification of `E61 Time Primitive`.

As a result, we have created the following property definitions:

- `E52 Time-Span. P81a end of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)
- `E52 Time-Span. P81b begin of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P81 ongoing throughout: E61 Time Primitive`)

&nbsp;

> D2. addition of two new properties, defined as subproperties of `E52 Time-Span. P82 at some time within: E61 Time Primitive`, for the 'minimum outer bounds' specification of `E61 Time Primitive`.

Consequently, we have created the following property definitions:

- `E52 Time-Span. P82a begin of the begin: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)
- `E52 Time-Span. P82b end of the end: rdfs:Literal` (subPropertyOf `E52 Time-Span. P82 at some time within: E61 Time Primitive`)

&nbsp;

> D3. addition of two new properties, defined as subproperties of `E54 Dimension. P90 has value: E60 Number`, for the upper and lower boundaries specification of `E60 Number`.

Consequently, we have created the following property definitions:

- `E54 Dimension. P90a has lower value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)
- `E54 Dimension. P90b has upper value limit: rdfs:Literal` (subPropertyOf `E54 Dimension. P90 has value: E60 Number`)

&nbsp;

#### **E. Multiple ISA custom declaration Classes**

The following are declarations of frequently used multiple instantiations as classes with respective multiple IsA (so far only one):

> E1. addition of a new class, defined as subClassOf `E41 Appellation` and `E33 Linguistic Object`, for all Appellations being regarded
specific to or characteristic for a language group and being described indirectly via a URI.

&nbsp;

As a result, we have created the following class definition:

- `E41_E33_Linguistic_Appellation` subClass of `E41_Appellation` and `E33_Linguistic_Object`

&nbsp;

#### **F. Translations and Scope notes**

> F1. **Translations** Each RDFS Class or Property definition is accompanied by a set of translations expressed as rdfs:labels separated by xml:lang tags. The translation label used for each language is **the most recent** still **valid** translation. Still valid translation may be interpreted in the following ways:

- The English label of the `CurrentVersion` that the current RDFS file is referring to is *almost equal* (accepting only differences regarding multiple spaces) to the English label of the `TranslationVersion` that the translated label was created for. For example, in order to accept a translation for `E22 Human-Made Object` in the CurrentVersion (7.1.1) we should have a translation for E22 that was created based on a CIDOC version that used the same name for E22. The class name of E22 was changed from `E22 Man-Made Object` to `E22 Human-Made Object` in version 6.2.7. So the only **valid** translations would be these that were created based on version 6.2.7 or later. Currently no such translation has been created, so `E22 Human-Made Object` in CurrentVersion (7.1.1) does not define any translation in the RDFS file.

- Another option is to qualify translations as **valid** based on the scope note comparison of `CurrentVersion` and `TranslationVersion`. Since scope notes include formatting information, the comparison should ignore formatting changes.

Currently in this version we follow the option to qualify translations as valid based on the comparison of the respective labels in the corresponding CIDOC versions.

&nbsp;

> F2. **Scope notes** are defined as rdfs:comment and include only the text of the official CIDOC-CRM version in English language. Since scope notes are not just simple texts but include formatting data, a simple removal of the formatting data does not always produce a readable format. Therefore we followed the below simple conventions for the transformation of CIDOC-CRM scope note text into a readable rdfs:comment:

- Each Paragraph defines a new line
- Each element in an unordered list defines a new line
- Each element in an ordered list defines a new line prefixed with `'{order}. '`
- Each superscript text is separated from its base with `'-'`
- Each subscript text is separated from its base with `'_'` (none found)

&nbsp;

> F3. **Scope notes of Inverse Properties**. Scope notes for the inverse/backwards direction of properties are not defined as they are covered by the definition of the direct/forward direction definition. Whenever the direct/forward property direction of a property `Pxxx` is not defined due to policy **B1**, then its scope note is used as scope note for the inverse/backwards property `Pxxxi`, and labeled as `Scope note for 'Pxxx':`.

As a result, the direct/forward property definition was used for the rdfs:comment definition of

- `E92 Spacetime Volume. P169i spacetime volume is defined by: rdfs:Literal`
- `E52 Time-Span. P170i time is defined by: rdfs:Literal`
