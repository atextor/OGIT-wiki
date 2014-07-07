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

The ontology is represented in YAML format. We are 
