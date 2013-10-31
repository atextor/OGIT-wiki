A Verb record consists of the following fields:  

Element | Definition
-------- | ---------
`id` | unique ID of the verb
`name` | A human readable display this verb
`description` | A description of this verb
`valid-from` | The earliest date and time this entity is valid
`valid-until` | The date and time until this entity is valid
`creator` | The ID of the user-entity who created this entity
`deleter` | The ID of the user-entity who made this entity obsolete
`created` | The date and time until this entity was created
`modified` | The date and time until this entity was updated last
`admin-contact` | The ID of the user who maintains this entity on the SGO level
`tech-contact` | The ID of the user who maintains this entity on the NTO level
`url` | A URL where the definition for this verb can be obtained. This definition must include a type (XML, RDF, …)
`history` | A change log for this entity showing when, what, and by whome changes where made.
`allowed` | A list directed FROM_ENTITY TO_ENTITY pairs telling which 'entities' can be connected by this verb

