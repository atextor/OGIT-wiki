## Introduction

The goal of the OGIT (Open Graph of IT) Ontology Framework is to build an open semantic representation of all IT and it's interactions with business process and people for providing a foundation for computational evaluation. The resulting knowledge graph allows to interconnect various IT Systems in the IT Operations and IT Management space. While previous approaches required too much details and did not allow any ambiguity and incorrectness, OGIT allows varying levels of details, accepts incorrectness and supports different classes of data to allow proper handling from a computational perspective. The ontology structure is designed for providing a common, best practice approach, thought giving great flexibility and extensibility for every implementor.  

## The meta model

The best way to describe the meta model is to see it as a 5 layer onion to describe the data spaces within OGIT:

![Onion like Meta model](https://github.com/arago/graphIT-ontology/raw/master/Wiki/imgs/Onion.png)

### 1. SGO - Semantic Graph Ontology

This upper level defines entity type and actions possibly connecting two entity in the graph data model. At this level the highest level of semantic is described governing the whole ontology framework.

### 2. NTO - Node Type Ontology

This level describes the sub ontology for a specific node type, which is represented by a entity on the SGO level. These sub ontology represents the special needs of the corresponding node type. Here the subject matter expertise is put into the semantic model.

### 3. SNRA - Specific Node Required Attribute

Each node well defined by SGO and NTO ontologies will have a set of attributes that is specific to this unique type of node. The SNRA defines the minimal required attribute set. The Open Graph of IT can handle any data following the SGO and NT typology, but the tools attached will only work properly if the SNRA definition is followed.

### 4. SNBA - Specific Node Best Practice Attributes

Each well defined node type can have a number of attributes that have proven useful. If these attribute suggestions are followed reuse and effectiveness of platform resources (e. g. knowledge in automation, architectural benchmarks) are maximized.

### 5. SNFA - Specific Node Free Attributes 

The Free attribute space in every node is used by applications, users and organizations to add data structured according to their own needs, concepts and ideas. Data stored here uses the mechanisms of the Open Graph of IT for storage and the clients of the Open Graph of IT as tools, but does not count on any cross user reuse or platform wide distribution of effect, e.g. marketability. This section is also the source for best practices which might move up the chain into the SNBA section. As users use the same or similar attributes in the SNFA sections they will move into the SNBA section to become published and discussed in a platform wide distribution.

![OGIT Structure](https://github.com/arago/graphIT-ontology/raw/master/Wiki/imgs/OGIT_Structure.png)

***

## The Ontology layers in details

### SGO - Semantic Graph Ontology

The SGO is a unique structure with represents the top level of semantic behind the Open Graph of IT. In this highest level of data structure the entities and actions connecting these entities are described. The SGO is maintained by the _SGO Ontology Board_. Because the entities are representing knowledge they will be categorized as knowledge classes. There are 4 categories of entities in the SGO, which differ in terms of validity, volatility and level of correctnes.

______ | Global Knowledge | Factual Knowledge | Actionable Knowledge|Situational Knowledge
------ | --------------- | --------------- | --------------- | --------------- | 
_Description_ | long lasting meta definitions | actual state of the world | possible activities and decisions | current state of the model
_Examples_ | the ontology itself, governing policies | actual IT provisioned, contracts | commands, reactions to regul. demands | monitoring data, events, KPIs
_Volatility_ | extremely low | low | high | very high
_Validity_ | virtually infinite | long | virtually infinite | verly low
_Correctness_ | high | low | high | very high

The ontology definition is stored in the global knowledge part itself, following a "build yourself" approach in terms of data, structure and platform. While the top layers _'BigBang'_ and _'Global, Factual, Actionable, Situational knowlege'_ are fixed, the further hierarchy constructed below these must follow these simple principles:

a. Any child type must posses any categorizing 'attribute/action' the parent type has.  

b. An instance of a child type must be a valid instance of any parent type.  

c. The "leafs" of the SGO type tree are described by NTOs

This classification for each entity on the top level of the ontology of the open graph of IT is done to have a classification in terms of use case, but also to determine the class of technology and algorithmic approaches valid when working with data from the respective sets (i.e. can caching be applied, is in memory technology a good choice, do algorithms need to deal with incorrectness, is ambiguity part of analytical approaches etc.). 

The boundary where SGO ends and the NTOs starts also determines the responsobility: While the SGO ist maintained by the SGO ontology board, the NTOs are maintained by appointed SMEs.

There are two kinds of data stored on the SGO level:

*Entities*

Describes the kind of data that is stored under this category.

_[Entity record definition](https://github.com/arago/graphIT-ontology/wiki/SGO---Entity-Record-Definion)_

*Verbs*

Describes the type of _connection*_ and which other entities can be connected by it. 

_[Verb record definition](https://github.com/arago/graphIT-ontology/wiki/SGO---Verb-Record-Definition)_

**Examples**

_Entities:_  
1.	`Fruit`  
2.	`Plant`  
3.	`Ingredient`  

_Verbs of Fruit:_  
1.	`grown on`:     `Fruit` -> `Plant`  
2.	`contains`:   	`Fruit` -> `Ingredient`  
3.	`contains`:	`Plant` -> `Ingredient`  

_*_ _Connections_

_We do not use the term connection on purpose, because we want to ensure that new users coming from a relational approach should not be tempted to simply transfer their table model into semantic networks, but should know that a connection between two entities is something one entity does to or with another. Facebook calls these connections actions. We do not call it actions, because we have actionable knowledge and want to avoid misunderstandings._ 

### NTO - Node Type Ontologies

For each entity there is a sub ontology defining the different kinds of the same entity. The differentiation between SGO and NTO is made to allow subject matter experts to deal with NTO definitions while strategic experts deal with the definition of the big picture. 

_NTO is for the subject matter experts and can go into greater detail._

Subject matter experts can chose to describe the NTO level as typology trees or as sub ontologies themselves. The NTO is a typology behind an entity defined on the SGO level but it includes links to the attributes required, recommended or includable in the specifically defined type. This can follow an OO or single declaration approach. 

Examples for NTOs are

* arago's MARS model (IT model)
* arago's KI (_Knowledge Items_ definition)
* arago's MAID (Monitoring Interface definitions)
* arago's Issues 

### SNRA, SNBA, SNFA - Specific Node Attributes 

Each of these attribute sections describes attributes available or used with a well defined note type (SGO:NTO). The SNRA are always included and will be syntax checked by the open graph of IT access methodologies. The SNBA should be used and syntax checking for these is default but can be turned off and the SNFA are free and their definition can be included freely by the user. 

The NTO Tech Contact has to provide means to include specific free attributes in a local instance of the whole ontology without touching the SGO, NTO, SNRA sections and without having to modify the SNBA section. 
