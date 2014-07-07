# OGIT ontology details

Here we take a more technical view of OGIT. 

Reading this will help you give a deeper understanding of the ontology and make it much easier to read the [ontology documentation](https://autopilot.co/dev/doxygen-graphit).

Reading this is a must if you want to contribute to the ontology.

## Some terminology

The ontology defines
* attributes 
* entities
* verbs

_Entities_ define the objects we are dealing with in OGIT. Any instance of an _entitiy_ type can have attributes, which are predefined by _attribute_ definitions from the ontology.
_Verb_ definitions declare which relationships are possible between instances of _entity_ types.

### Relation between and OGIT ontology and OGIT data

The actual data will be stored in GraphIT. GraphIT is based on a graph database and used the OGIT ontology
to ensure that only valid data gets stored. We have the following relation between graph data and ontology definitions:
* Each **vertex** in the graph must be a valid instance of an _entity_ type defined by the ontology
* All vertex **properties** must be valid w.r.t. the the _attribute_ definitions from the ontology (what attribute validity means will be explained below)
* All **edges** in the graph must have labels defined by some _verb_ type in the ontology. 
* All triples defined by graph data must be valid according to the ontology (see below for more details)

Note: to prevent some confusion from now on we will use the terms _attribute_, _entity_, and _verb_ whenever referring to elements from the ontology and _property_, _vertex_, and _edge_ when talking about instance data.

### OGIT ontology format description

The ontology is represented in YAML format. By convention we are maintaining an individual YAML file for each attribute, verb, and entity definition. 

#### Attribute definitions

Defining an _attribute_ in OGIT ontology will require a YAML stanza like this (the exact format can be found in [Attribute.yaml.tpl](../blob/master/SGO/format/Attribute.yaml.tpl)):

```yaml
- Attribute:
    id: http://www.purl.org/ogit/color
    name: color
    description: "can be used to store the color of objects"
    validation-type: ""
    validation-parameter: ""
```

The example contains the following details

| Parameter | Description |
| --- | --- |
| id | a unique id for that ontology element (see "IDs of ontology elements" below) | 
| name | by convention this repeats the last part of ID. It is only used as display name. | 
| description | should describe the semantics of that attribute as clear as possible. This will be the main source of information if somebody needs to decide if that attribute can be re-used |
| validation-type, validation-parameter | if an entity definition refers to that attribute definition and a vertex of that entity is to be created, then actual value of that attribute will be the validated according to the requirements defined by validation-type and validation-parameter. Empty values, as in our example, will skip any further validation. (see "Attribute Validation") |

#### Entity definitions

Defining an _entity_ in OGIT ontology will require a YAML stanza like this (the exact format can be found in [Entity.yaml.tpl](../blob/master/SGO/format/Entity.yaml.tpl)):

```yaml
- Entity:
    id: http://www.purl.org/ogit/Tree
    name: Tree
    description: "This entity type models a tree"
    scope: SGO
    parent: http://www.purl.org/ogit/Factual

    attributes:
      any: false
      mandatory:
        - http://www.purl.org/ogit/type
      optional:
        - http://www.purl.org/ogit/color
```

The example contains the following details

| Parameter | Description |
| --- | --- |
| id | a unique id for that ontology element (see "IDs of ontology elements" below) | 
| name | by convention this repeats the last part of ID. It is only used as display name. |
| description | description of the purpose of that entity. This should be detailed enough to let others decide whether this entity can be re-used |
| scope | either "SGO" (stating that this entity definition is considered as part of the core ontology) or "NTO" (meaning that this entity definition is part of some domain specific extension) |
| parent | contains the id of another entity definition (see section about "Inheritance" below) |
| attributes | used for the property validation of all instances (vertices) of that entity type |  

#### Verb definitions

Defining a _verb_ in OGIT ontology will require a YAML stanza like this (the exact format can be found in [Verb.yaml.tpl](../blob/master/SGO/format/Verb.yaml.tpl)):

```yaml
- Verb:
    id: http://www.purl.org/ogit/likes
    name: likes
    description: |
      expressed that the 'from' entity 'likes' the 'to' entity
    cardinality: 

    allowed:
     - from: http://www.purl.org/ogit/Person
       to: http://www.purl.org/ogit/Tree
```
The example contains the following details

| Parameter | Description |
| --- | --- |
| id | a unique id for that ontology element (see "IDs of ontology elements" below) | 
| name | by convention this repeats the last part of ID. It is only used as display name. |
| description | description of the semantics of this verb. This should be detailed enough to let others decide whether this verb can be re-used |
| cardinality | possible values are: one2one, one2many, many2one, many2many. An empty value defaults to many2many. This is used by the GraphIT validator during edge creation. See https://github.com/thinkaurelius/titan/wiki/Type-Definition-Overview#cardinality-constraints for the specification of cardinality constraints |
| allowed | This is used by GraphIT validator during edge creation. It is a list of \['from', 'to'\] pairs. An edge of that verb type can be created if the pair \['entity type of source vertex', 'entity type of destination vertex'\] is compatible with one of the \['from', 'to'\] pairs. This comparison takes "inheritance" into account. |

#### Attribute Validation

property checks of vertices.

property value checks

#### IDs of ontology elements

IDs of ontology elements, e.g. _http://www.purl.org/ogit/color_ must be unique across the whole ontology (i.e. across the union of all attributes, verbs, and entities).

To ensure uniqueness we register a suitable Persistent URL for each ontology element at www.purl.org. 

**Note:** When using GraphIT REST API those IDs are always used with the prefix _http://www.purl.org/_. In our examples you would use _ogit/color_ to address that attribute type within an application.

#### Naming conventions

### to inherit or not to inherit?

