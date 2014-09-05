## Ontology extensions - best practices

### General recommendations

* consider reusing existing attributes and verbs before adding new ones
* Names for _Verbs_: plain verbs should be favored compound constructs (e.g. those using an auxiliary verb)
* There is usually no need to define _Verbs_ for both directions. E.g. if you define 'A smurfs B' you won't need not define 'B isSmurfedBy A', too.  (you can always query in both directions)
* _Verb_ descriptions: are usually very general. Any context specific semantics should be explained in the entity definitions

### Recommendations for NTOs

* if you think about modelling a complete area/topic with several entities, verbs, and attributes then this will probably be a separate NTO using a common fixed prefix: ogit/&lt;nto specific prefix&gt;/
* all entities will then get that prefix
* if you need attributes which have general semantics then re-use them from SGO if already there (e.g. ogit/name) or request SGO-extension to add them
* if you need attributes which are quite domain specific define them as part of NTO, hence ogit/&lt;nto specific prefix&gt;/&lt;my specific attribute&gt;
* it is not forbidden to re-use attributes from other NTO's. sometimes those have exactly the right semantics, e.g. ogit/gr/validFrom.
* if you need a verb to define a relationship between an entity of your NTO to one from SGO or another NTO that verb must be defined at SGO level
* if you need a verb that has general semantics and is not defined in SGO, yet.
  you should request SGO extension for it (even if your current need is restricted to relationships between entities from your NTO)
* if you need a verb that is very specific for you domain then you should define it within the NTO's namespace (then it can define relationships only between entities of that NTO)

### Reusing definitions from other standards/ontologies.

It is quite useful to reuse work from others. The authors of the source ontology might not know about that reuse. 
All ontology elements based on something else will have the _source_ information in the *admin-contact* field. *tech-contact* will refer to the importing organziation.

### Modelling for security

When data is actually stored in GraphIT the access control cannot be defined below the entity/node level. I.e. it is possible to allow/disallow
* access to all nodes of a specific type (e.g. 'biology/Tree')
* access to a specific node(s) 
to people or group(s) of people

   
