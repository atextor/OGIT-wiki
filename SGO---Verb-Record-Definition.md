A Verb record consists of the following fields:  

Element | Definition
-------- | ---------
AID | ID automatically generated verb 
Display Name | A human readable descriptive name for this verb
Description | A description of this verb
Validity start Time | The date and time this verb was created
Validity start User | The ID of the user-entity who created this verb
Validity end Time | The date and time this verb was mane obsolete
Validity end User | The ID of the user-entity who made the verb obsolete
Modify Time | The date and time this verb was last modified
Modify User | The ID of the user-entity who last modified this verb record
Admin Contact | The ID of the user-entity who maintains this verb on the SGO level
Tech Contact | The ID of the user-entity who maintains this verb on the NTO level
History | A list of DATE, ID, DESCRIPTION showing changes made when, by who and a description of the change made. This is a list of  graph links to inactive versions
Entity List | A list of SOURCE_ENTITY DESTINATION_ENTITY DIRECTED describing which entities this verb can connect and if the connection is uni directional or bi directional. 
Verb Definition | A URL where the definition for this verb can be obtained. This definition must include a type (XML, RDF, …)
