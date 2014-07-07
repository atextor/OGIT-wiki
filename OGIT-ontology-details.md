# OGIT ontology details

Here we take a more technical view of OGIT. 

Reading this will help you give a deeper understanding of the ontology and make it much easier to read the [ontology documentation](https://autopilot.co/dev/doxygen-graphit).

Reading this is a must if you want to contribute to the ontology.

## Some terminology

The ontology defines
* attributes 
* entities
* verbs

//Entities// define the objects we are dealing with in OGIT. Instance of an //entitiy// type can have attributes, which are predefined by //attribute// definitions from the ontology.
//Verb// definitions declare which relationships are possible between instances of //entity// types.

### Relation between and OGIT ontology and OGIT data

The actual data will be stored in GraphIT. GraphIT is based on a graph database and used the OGIT ontology
to ensure that only valid data gets stored. We have the following relation between graph data and ontology definitions:
* Each //vertex// in the graph must be a valid instance of an //entity// type defined by the ontology
* All vertex properties must be valid w.r.t. the the //attribute// definitions from the ontology (what attribute validity means will be explained below)
* All //edges// in the graph must have labels defined by some //verb// type in the ontology. 
* All triples defined by graph data must be valid according to the ontology (see below for more details)

 