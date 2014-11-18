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

**Note**: all example assume:
* suitable data is already there (i.e. the queries can return a non-empty set)
* the access policies allow the user to read/query the data

### Simple Use case A: basic attribute search

#### Problem description

Find all registered users with an email address ending in "@x.com".

#### Mapping to OGIT data

| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- |
| user | ogit/Person | entity | |
| email address | ogit/email | attribute | play the role of a *primary key* for ogit/Person entities |

#### Sample query

Attribute based search is best done with a query of type *vertices*. 
```
curl -X GET '<graphit base url>/query/vertices?query=ogit%5C%2email%3A*%40x.com%20AND%20ogit%5C%2F_type%3Aogit%5C%2FPerson'
```

special characters are escaped using standard URL escaping rules.

The query syntax is based on [lucene index queries](http://lucene.apache.org/core/4_6_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#package_description). 

The unescaped query string will be:
```
ogit\/email:*@x.com AND ogit\/_type:ogit\/Person
```

### Simple Use case B: basic graph search

#### Problem description

#### Mapping to OGIT data

#### Sample query


### Use case 1: ticket statistics

#### Problem description

A User wants to compile various statistics on Tickets: Find out who was the responsible agent, when and what he did, which authorizations were required, how long it took solve the problem. Also have a look at the request side - find out who was the requester, which applications/components were affected. 

#### Mapping to OGIT data

#### Sample query


### Use case 1: ticket statistics

#### Problem description

A User wants to compile various statistics on Tickets: Find out who was the responsible agent, when and what he did, which authorizations were required, how long it took solve the problem. Also have a look at the request side - find out who was the requester, which applications/components were affected. 

#### Mapping to OGIT data

#### Sample query

