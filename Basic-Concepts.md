## Ontology layers 

The best way to describe the meta model is to see it as a 5 layer onion to describe the data spaces within OGIT:

![Onion like Meta model](https://github.com/arago/graphIT-ontology/raw/master/Wiki/imgs/Onion.png)

### 1. SGO - Semantic Graph Ontology

The SGO represents the top level of semantic behind OGIT and is maintained by the _Ontology Board_.

The boundary where SGO ends and the NTOs starts also determines the responsibility: While the SGO ist maintained by the _Ontology board_, the NTOs are maintained by appointed SMEs.

There are two kinds of data stored on the SGO level:

**Entities**

Describes the kind of data that is stored under this category.

_[Entity record definition](OGIT-ontology-details#entity-definitions)_

**Verbs**

Describes the type of _connection*_ and which other entities can be connected by it. 

_[Verb record definition](OGIT-ontology-details#verb-definitions)_

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

### 2. NTO - Node Type Ontology

The differentiation between SGO and NTO is made to allow subject matter experts to deal with NTO definitions while strategic experts deal with the definition of the big picture. 

Examples for NTOs:

* Automation
* Business Process
* Cost
* Service Management

### 3. SNRA, SNBA, SNFA - Specific Node Attributes 

Each of these attribute sections describes attributes available or used with a well defined note type ( _SGO:NTO_ ).  

#### 3.1 SNRA - Specific Node Required Attribute

Each node well defined by SGO and NTO ontologies will have a set of attributes that is specific to this unique type of node. The SNRA defines the minimal required attribute set. 

The SNRA are always included and are mandator, so the syntax will be checked and the tools attached will only work properly if the SNRA definition is followed. 

### 3.2 SNBA - Specific Node Best Practice Attributes

Each well defined node type can have a number of attributes that have proven useful. If these attribute suggestions are followed, then the reuse and effectiveness of platform resources (e. g. knowledge in automation, architectural benchmarks) are maximized. SNBA could be read as "optional" attributes in GraphIT documentation [entity pages] (https://graphit.co/docs/group__entities.html).

### 3.3 SNFA - Specific Node Free Attributes 

The Free attribute space in every node is used by applications, users and organizations to add data structured according to their own needs, concepts and ideas. Data stored here uses the mechanisms of the _Open Graph of IT_ for storage and the clients of the _Open Graph of IT_ as tools, but does not count on any cross user reuse or platform wide distribution of effect, e.g. marketability. This section is also the source for best practices which might move up the chain into the SNBA section. As users use the same or similar attributes in the SNFA sections they will move into the SNBA section to become published and discussed in a platform wide distribution.