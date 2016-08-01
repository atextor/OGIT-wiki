## Ontology extensions - best practices

### General recommendations

* Consider reusing existing attributes and verbs before adding new ones.
* Names for _verbs_: plain verbs should be favored over compound constructs (e.g., those using an auxiliary verb).
* There is usually no need to define _verbs_ for both directions. E.g., if you define 'A smurfs B' you will not need to
  define 'B isSmurfedBy A', too. (You can always query in both directions).
* _Verb_ descriptions are usually very general. Any context-specific semantics should be explained in the entity
  definitions.
* Before you send a pull request, make sure all files are syntactically valid turtle files. You can use the
  [rapper](http://librdf.org/raptor/rapper.html) command-line utility for that. In a Debian-based system, you can install
  it by running `apt-get install raptor2-utils`. In your cloned OGIT repository, run the following command:

  `find . -type f -name '*.ttl' -exec rapper -q -i turtle -o rdfxml "{}" \; >/dev/null`

  If there are any syntax errors, rapper will report the offending file name and line number as follows:

  ```
  rapper: Error - URI file:///home/user/git/OGIT/NTO/ServiceManagement/entities/Incident.ttl:17 - syntax error
  rapper: Failed to parse file ./NTO/ServiceManagement/entities/Incident.ttl turtle content
  ```

  Another possibility is to use an editor or IDE that supports turtle syntax validation, such as [XTurtle](http://aksw.org/Projects/Xturtle.html).


### Recommendations for NTOs
If you think about modelling a complete area/topic with several entities, verbs and attributes, then this will
probably be a separate NTO using a common fixed prefix (namespace).
* All entities will then get that prefix.
* If you need attributes which have general semantics then re-use them from SGO if already present (e.g., `ogit:name`), or
  request an SGO-extension to add them.
* If you need attributes which are quite domain-specific, define them as part of NTO, hence using the same namespace
  prefix as for the entities.
* You may re-use attributes from other NTOs. Sometimes those have exactly the right semantics, e.g.,
  `ogit.gr:validFrom`.
* If you need a verb to define a relationship between an entity of your NTO to one from SGO or another NTO, that verb
  must be defined in namespace `ogit` or at least one of its sub namespaces.
* If you need a verb that has general semantics and is not defined in SGO yet, you should request SGO extension for it
  (even if your current need is restricted to relationships between entities from your NTO).
* If you need a verb that is very specific for your domain, then you should define it within the NTO's namespace (then it
  can define relationships only between entities of that NTO).

### Reusing definitions from other standards/ontologies.

It is quite useful to reuse work from others. The authors of the source ontology might not know about that reuse. All
ontology elements based on something else will have the _source_ information in the *admin-contact* field.
*tech-contact* will refer to the importing organization.

### Maintaining PURLs

See [here](../../blob/master/NTO/PURL_ID_Registration.md)

### Small, Medium, Large?

There are several ways to contribute to the ontology. Depending on the complexity of extension you are going to
suggest/make you might choose a way that gives you more freedom for modelling. However, more flexibility comes with more
responsibility you take for your contributions:

1. You want to use the ontology as is, but you would like to have some extensions (or things in namespace `ogit` or one
of its sub-namespaces). Then use either GitHub's [issue and/or pull mechanism](../../blob/master/CONTRIBUTING.md). Any
such change has to be approved by the _Ontology Board_.

2. You want to model a problem domain where you are confident it will be of general use. Then the best approach is to
create a sub-namespace of `ogit` for things specific to that problem domain. You should plan such a big extension with
the help of the _Ontology Board_. GitHub's pull mechanism will be the appropriate tool to submit your results.

3. You want to model a problem domain with more freedom and you are not sure about the general applicability, yet. Then
the best approach is to define a new namespace `myspace` and define everything in there. <br/> By convention there is one
restriction: Inside such a new namespace you can't define verbs (relationships) pointing to entities from outside that
namespace. Such relationships have to be requested to be included in the `ogit` namespace.<br/> Using this approach you're
expected to maintain the PURL domain for `myspace` by yourself.

