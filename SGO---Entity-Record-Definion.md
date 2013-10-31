An entity record consists of the following fields:

Element | Definition
-------- | ---------
`id` | a unique identifier
`name` | A human readable display name
`description` | A description of this entity
`valid-from` | The earliest date and time this entity is valid
`valid-until` | The date and time until this entity is valid
`creator` | The ID of the user-entity who created this entity
`deleter` | The ID of the user-entity who made this entity obsolete
`created` | The date and time until this entity was created
`modified` | The date and time until this entity was updated last
`admin-contact` | The ID of the user who maintains this entity on the SGO level
`tech-contact` | The ID of the user who maintains this entity on the NTO level
`history` | A change log for this entity showing when, what, and by whome changes where made.
`attributes` | Lists all required (mandatory) and best-practices (optional) attributes specific to this entity
