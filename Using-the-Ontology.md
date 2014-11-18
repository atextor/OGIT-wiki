## High Level Use case examples

After you got familiar with same concepts and ideas behind OGIT and its ontology you might ask:

* What is it good for?
* What can I do with it?

Remember that there is already a backend application ( *GraphIT* shipped as part of *[AutoPilot](https://autopilot.co)* ) which provides a data store using OGIT ontology as its schema. *GraphIT* provides a RESTful API that allows applications to work with data described by means of OGIT.

There are several benefits in writing applications on top of OGIT and GraphIT:

* the ontology (the *schema*) is decoupled from the application thus allowing different applications to directly work with the same data.
* the ontology is not a fixed standard. It's open for extensions and improvements and it's supposed to evolve along with application development.

But the most interesting aspect of OGIT and GraphIT is the way you can work with data. OGIT ontology is defining [graph semantics](Maintaining the Ontology.md) for the data which allow
* much more flexible ways to define relationships within your data (compared with things like foreign keys in relational data stores)
* several ways to query the data including graph queries based on [Gremlin](https://github.com/tinkerpop/gremlin/wiki)

In the following we present a few use cases showing how questions about your IT environment translate into queries based on OGIT ontology.


