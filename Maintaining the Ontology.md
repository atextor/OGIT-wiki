## Ontology extensions - best practices

### General recommendations

* consider reusing existing attributes and verbs before adding new ones
* Names for _Verbs_: plain verbs should be favored compound constructs (e.g. those using an auxiliary verb)
* There is usually no need to define _Verbs_ for both directions. E.g. if you define 'A smurfs B' you won't need not define 'B isSmurfedBy A', too.  (you can always query in both directions)
* _Verb_ descriptions: are usually very general. Any context specific semantics should be explained in the entity definitions

### Recommendations for NTOs

* if you think about modelling a complete area/topic with several entities, verbs, and attributes then this will probably be a separate NTO using a common fixed prefix (namespace) 
* all entities will then get that prefix
* if you need attributes which have general semantics then re-use them from SGO if already there (e.g. ogit/name) or request SGO-extension to add them
* if you need attributes which are quite domain specific define them as part of NTO, hence using the same namespace prefix as for the entities
* it is not forbidden to re-use attributes from other NTO's. sometimes those have exactly the right semantics, e.g. ogit/gr/validFrom.
* if you need a verb to define a relationship between an entity of your NTO to one from SGO or another NTO that verb must be defined in namespace ''/ogit'' or at least one of its sub namespaces.
* if you need a verb that has general semantics and is not defined in SGO, yet.
  you should request SGO extension for it (even if your current need is restricted to relationships between entities from your NTO)
* if you need a verb that is very specific for you domain then you should define it within the NTO's namespace (then it can define relationships only between entities of that NTO)

### Reusing definitions from other standards/ontologies.

It is quite useful to reuse work from others. The authors of the source ontology might not know about that reuse. 
All ontology elements based on something else will have the _source_ information in the *admin-contact* field. *tech-contact* will refer to the importing organization.

### Small, Medium, Large?

There are several ways to contribute to the ontology. Depending on the complexity of extension you are going to suggest/make you might choose a way that gives you more freedom for modelling. However, more flexibility comes with more responsibility you take for your contributions:

1. you want to use the ontology as is, but you would like to have some extensions (or things in namespace ''/ogit'' or one of its sub-namespaces). Then use either GitHub's [issue and/or pull mechanism](../../blob/master/CONTRIBUTING.md). Any such change has to be approved by the _SGO Board_.

2. you want to model a problem domain where you are confident it will be of general use. Then the best approach is to create a sub-namespace of /ogit for things specific to that problem domain. You should plan such a big extension with the help of the _SGO Board_. GitHub's pull mechanism will be the appropriate tool to submit your results.

3. you want to model a problem domain with more freedom and you are not sure about the general applicability, yet. The the best approach is to define a new namespace /myspace and define everything in there. <br/> By convention there is one restriction: Inside such a new namespace you can't define verbs (relationships) pointing to entities from outside that namespace. Such relationships have to requested to be included in the /ogit namespace.

### Modelling for security

When data is actually stored in GraphIT the access control cannot be defined below the entity/node level. I.e. it is possible to allow/disallow
* access to all nodes of a specific type (e.g. 'biology/Tree')
* access to a specific node(s) 
to people or group(s) of people.

However, attribute level access control is supported. I.e. it is impossible to allow access to some node 'A' to some people but restricting access to an attribue 'x' in 'A' only to a smaller group of people.

Or in other words: a user can access a node either whole or not at all.

Hence before you add attributes containing sensitive data to some existing entity type you should consider the following alternative: Create a new entity type with some suitable relation to the existing type and define the attributes containing the critical data in the new entity type.
   
If you decide to create new entity types to protect some attributes you should link the new type with 'ogit/extends' relationship to the original one.