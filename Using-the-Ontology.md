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

First we start with same basic examples in more depth:

### Simple Use case A: basic attribute search

#### Problem description

Find all registered users with an email address ending in "@x.com".

#### Mapping to OGIT data

| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| user | ogit/Person | entity | |
| email address | ogit/email | attribute | plays the role of a *primary key* for ogit/Person entities |

#### Sample query

Attribute based search is best done with an index query of type *vertices*. 
```
curl -X GET '<graphit base url>/query/vertices?query=ogit%5C%2email%3A*%40x.com%20AND%20ogit%5C%2F_type%3Aogit%5C%2FPerson'
```

special characters are escaped using standard URL escaping rules.

The query syntax is based on [lucene index queries](http://lucene.apache.org/core/4_6_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#package_description). 

The unescaped query string will be:
```
ogit\/email:*@x.com AND ogit\/_type:ogit\/Person
```

### Simple Use case B: on step graph search

#### Problem description

Which groups, departments, companies does user with *sample@x.com* belong is member of?

#### Mapping to OGIT data

| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| user | ogit/Person | entity | |
| email address | ogit/email | attribute | plays the role of a *primary key* for ogit/Person entities |
| group | ogit/Organization | entity | |
| department | ogit/Organization | entity | |
| company | ogit/Organization | entity | |
| is member of | ogit/isMemberOf | verb | |
| *UUID* | ogit/_id | attribute | internal attribute |
| group/department/company ID | ogit/_id | attribute | by convention ogit/Organization will have some domain name like *ogit/_id* used as primary identifier |
| group/department/company name | ogit/name | attribute | uniquness not guaranteed |

#### Sample query

First we need an index query to figure out the UUID of the ogit/Person node with email *sample@x.com*:

Attribute based search is best done with an index query of type *vertices*. 
```
curl -X GET '<graphit base url>/query/vertices?query=ogit%5C%2email%3Asample%40x.com%20AND%20ogit%5C%2F_type%3Aogit%5C%2FPerson'
```

query part in unescaped form will be:
```
ogit\/email:sample@x.com AND ogit\/_type:ogit\/Person
```

The result will be a JSON structure liek this:
```
[
  {
    "ogit/_created-on" : "Wed, 01 Jan 2014 12:00:00 GMT"
    "ogit/_creator" : "autopilotadmin@arago.de"
    "ogit/_graphtype" : "vertex"
    "ogit/_id" : "sample@x.com"
    "ogit/_is-deleted" : "false"
    "ogit/_modified-on" : "Wec, 01 Jan 2014 12:00:00 GMT"
    "ogit/_owner" : "sample@x.com"
    "ogit/_type" : "ogit/Person"
    "ogit/email" : "sample@x.com"
  }
]
```

From the result we pick *ogit/_id*. (in case of *ogit/Person* objects this will be identidal to *ogit/email* but that's not true for other entity types).

Then we use that ID as starting point for the secondary query:

```
curl -X GET '<graphit base url>/sample%40x.com/ogit%2FisMemberOf'
```

the result will be a JSON array containing all entities connected via *ogit/isMemberOf* to the starting point of the search. Each entry will look like:

```
  {
    "ogit/_created-on" : "Thu, 04 Sep 2014 13:02:01 GMT"
    "ogit/_creator" : "autopilotadmin@arago.de"
    "ogit/_graphtype" : "vertex"
    "ogit/_id" : "x.com"
    "ogit/_is-deleted" : "false"
    "ogit/_modified-on" : "Thu, 04 Sep 2014 13:02:01 GMT"
    "ogit/_owner" : "autopilotadmin@arago.de"
    "ogit/_type" : "ogit/Organization"
    "ogit/name" : "X Ltd."
  }
```

(to be sure) you should filter the result set by condition: *ogit/_type* equals *ogit/Organization*. Then the list of *ogit/_id* attributes will be the final result.


### Simple Use case C: basic graph search

#### Problem description

For a giving *Organization* find all other organizations which directly connected to the given one by any type of relationship.

#### Sample query

As in the previous example we need to figure out the *ogit/_id* of *ogit/Organization* being the starting point of our search. Let's assume this resulted in "ca9c2d7e-7dc1-4ba2-9995-01e2d66c47d4".

Then the answer to our graph query will be found by:
```
curl -X GET '<graphit base url>/query/gremlin?query=bothE.bothV.has%28%27ogit%2F_type%27%2C%20%27ogit%2FOrganization%27%29&root=ca9c2d7e-7dc1-4ba2-9995-01e2d66c47d4"
```

Without URL escaping the query reads
```
bothE.bothV.has('ogit/_type', 'ogit/Organization')
```

Graph queries are based on the [Gremlin query language](http://gremlindocs.com/)


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

