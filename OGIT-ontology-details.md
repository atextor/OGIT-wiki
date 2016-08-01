# OGIT ontology details

Here we take a more technical view of OGIT.

You should read this if you want to contribute to the ontology.

### Some terminology

OGIT defines an information schema, while an implementation that makes use of OGIT manages instances of the schema. OGIT differentiates between three types of elements:

* _Entities_ represent concepts (nodes in the graph),
* _Verbs_ are binary relations (edges) between two Entities and describe something an Entity does to or with another,
* _Attributes_ are binary relations (edges) between an Entity and a scalar value, such as a string or an integer.

OGIT does not specify how an implementation to manage instance data must look like, but on the [Start page](wiki/Home) 
you can find some pointers on how to get started with OGIT and different technologies. Although it is no technical 
requirement, using a graph database or triple store to store instance data is a good idea due to the nature of RDF 
format. You can then use whatever APIs the technology of your choice provides to insert and query data, e.g., REST APIs 
or SPARQL endpoints.

### OGIT ontology format description

The ontology is represented in the [Resource Description 
Format](https://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/) (RDF), specifically the 
[Turtle](https://www.w3.org/TR/2014/REC-turtle-20140225/)-Syntax. By convention, each definition of an entity, a verb or 
an attribute is described in a separate file.

#### Attribute definitions

Defining an _attribute_ in the OGIT ontology will require a Turtle fragment like this (the exact format can be found in [attribute-sample.ttl](wiki/attribute-sample.ttl)):

```turtle
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl:     <http://www.w3.org/2002/07/owl#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix ogit:    <http://www.purl.org/ogit/> .

<http://www.purl.org/ogit/color>
    a owl:DatatypeProperty;
    rdfs:subPropertyOf ogit:Attribute;
    rdfs:label "color";
    dcterms:description "can be used to store the color of objects";
    ogit:validation-type "regex";
    ogit:validation-parameter "^(blue|red|yellow)";
.
```

In this example for an attribute called _color_, its unique id is denoted by the URI in the first line. Each following
line specifies more information about the element:

| Parameter | Description |
| --- | --- |
|a|Defines this element to be a _data property_, i.e., something that relates an entity with a scalar value.|
|rdfs:subPropertyOf|Defines this element to explicitly be an OGIT attribute.|
|rdfs:label|Assigns a human-readable name to the element. By convention, this is the same as the last part of the URI.|
|dcterms:description|Assigns a human-readable description to the element. It should describe the attribute succinctly so that a user can make a decision to re-use this attribute in another context or not. This uses the [Dublin Core](http://dublincore.org/) metadata vocabulary.|
|ogit:validation-type, ogit:validation-parameter|Allow to define an optional attribute value validation. Possible validation-types are 'regex', 'xml' and 'generator'. Empty values will skip any further validation.|

#### Entity definitions

Defining an _entity_ in the OGIT ontology will require a Turtle fragment like this (the exact format can be found in [entity-sample.ttl](wiki/entity-sample.ttl)):

```turtle
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl:     <http://www.w3.org/2002/07/owl#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix ogit:    <http://www.purl.org/ogit/> .

<http://www.purl.org/ogit/biology/Tree>
    a rdfs:Class;
    rdfs:subClassOf ogit:Entity;
    rdfs:label "Tree";
    dcterms:description "This entity type models a tree";
    ogit:mandatory-attributes (
             ogit:id
    );
    ogit:optional-attributes (
             ogit:color
    );
.
```

The example contains the following details:

| Parameter | Description |
| --- | --- |
|a|Defines this element to be a _Class_, i.e., a set of things (instances).|
|rdfs:subClassOf|Defines this element to explicitly be an OGIT entity.|
|rdfs:label|Assigns a human-readable name to the element. By convention, this is the same as the last part of the URI.|
|dcterms:description|Assigns a human-readable description to the element. It should describe the entity succinctly. This uses the [Dublin Core](http://dublincore.org/) metadata vocabulary.|
|ogit:mandatory-attributes|A list of attributes that must be present for instances of this entity.|
|ogit:optional-attributes|A list of attributes that may be present for instances of this entity.|

#### Verb definitions

Defining an _verb_ in the OGIT ontology will require a Turtle fragment like this (the exact format can be found in [verb-sample.ttl](wiki/verb-sample.ttl)):

```turtle
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl:     <http://www.w3.org/2002/07/owl#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix ogit:    <http://www.purl.org/ogit/> .

<http://www.purl.org/ogit/likes>
    a owl:ObjectProperty;
    rdfs:subPropertyOf ogit:Verb;
    rdfs:label "likes";
    dcterms:description "expresses the 'from' entity 'likes' the 'to' entity";
    ogit:cardinality "many2many";
    ogit:allowed (
        [
            ogit:from ogit:Person;
            ogit:to ogit:Tree;
        ]
        [
            ogit:from ogit:Animal;
            ogit:to ogit:Person;
        ]
    );

```

The example contains the following details:

| Parameter | Description |
| --- | --- |
|a|Defines this element to be an _object property_, i.e., something that relates an entity with another entity.|
|rdfs:subPropertyOf|Defines this element to explicitly be an OGIT verb.|
|rdfs:label|Assigns a human-readable name to the element. By convention, this is the same as the last part of the URI.|
|dcterms:description|Assigns a human-readable description to the element. It should describe the verb succinctly. This uses the [Dublin Core](http://dublincore.org/) metadata vocabulary.|
|ogit:cardinality|Possible values are: one2one, one2many, many2one, many2many. If this line is left out, the value defaults to many2many.|
|ogit:allowed|A list of pairs of `ogit:from` and `ogit:to` that are valid for instances using this verb.|

#### Attribute Validation

An implementation that makes use of OGIT should honor the validation rules defined for attributes, entities and verbs.
* When an instance for an entity is created, attributes defined as "mandatory" must be present (they can have empty values, though).
* For all values of attributes marked as "mandatory" or "optional", value validation is performed, i.e., `ogit:validation-type`, `ogit:validation-parameter` and `ogit:allowed` are evaluated.

#### Naming conventions

Each ontology element has a unique URI. Each URI consists of three parts:

```
http://www.purl.org/<namespace>/<short name of ontology element>
```

Guidelines for all OGIT elements including NTOs can be found [here] (../tree/master/SGO/format/README.md).

### Free attributes

Every instance of an entity can have attributes additional to those defined in the ontology. They are called *free* attributes by convention. An implementation that makes use of OGIT should support this.

