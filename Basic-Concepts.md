## Introduction

The goal of the OGIT Ontology Framework is to build an open semantic representation of all IT and it's interactions with business process and people for providing a foundation for computational evaluation. While previous approaches required too much details and did not allow any ambiguity and incorrectness, OGIT allows varying levels of details, accepts incorrectness and supports different classes of data to allow proper handling from a computational perspective.

## The meta model

The best way to describe the meta model is to see it as a 5 layer onion to describe the data spaces within OGIT:

!['Onion like' Meta model](Wiki/imgs/Onion.png)
.
https://raw.github.com/arago/graphIT-ontology/master/Wiki/imgs/Onion.jpg

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

## The Ontology layers in details
