# Real life use case examples

Here we present some real life and sophisticated use cases based on *GraphIT* and *OGIT ontology*.

You should consult the [Simple use case examples](https://github.com/arago/OGIT/wiki/Using-the-Ontology) first.

### Use case 1: ticket statistics

#### Problem description

A User wants to compile various statistics on Tickets: Find out who was the responsible agent, when and what he did, which authorizations were required, how long it took solve the problem. Also have a look at the request side - find out who was the requester, which applications/components were affected. 

#### Mapping to OGIT data


| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| User | ogit/Person | entity | |
| Tickets | ogit/ITSM/Incident, ogit/ITSM/ChangeRequest, ... | entity | |
| to be responsible | ogit/ITSM/assignedTo | verb | |
| responsible agent | ogit/Person, ogit/Organziation | entity | individual or group of individuals |
| actions: when and what? | *n/a* | | |
| authorizations | ogit/ITSM/ApprovalTask - ogit/belongsTo -> ogit/ITSM/ChangeRequest, ... | entity, verb | |
| when was it reported | ogit/ITSM/reportedAt | attribute | specific for ogit/ITSM/Incident, other ticket types might use other fields, e.g. ogit/ITSM/openedAt |
| when was it resolved | ogit/ITSM/resolvedAt | attribute | specific for ogit/ITSM/Incident, other ticket types might use other fields, e.g. ogit/ITSM/closedAt |
| requester | ogit/ITSM/requestedBy -> ogit/Person, ogit/Organization  | verb, entity| specific for ogit/ITSM/ChangeRequest |
| reporter | ogit/ITSM/reportedBy -> ogit/Person | verb, entity | specific for ogit/ITSM/Incident, ogit/ITSM/Problem |
| affected components | ogit/ITSM/reportedOn -> arago/MARSNode | verb, entity | primary CI of some incident or problem |
| affected components | arago/MARSNode <- ogit/affectedBy | verb, entity | secondary CIs of some incident or problem or CIs possibly affected by planned change |

#### Sample query

As an example we show the query needed for one specific question from our problem domain:

Show all unresolved incident records reported by user 'sample@x.com'.

This can be done using graph query 
```
inE.has('label','ogit/ITSM/reportedBy').outV.or(_().hasNot('ogit/ITSM/resolvedAt), _().has('ogit/ITSM/resolvedAt', ''))
```
anchored at `root=sample@x.com`.


### Use case 2: Provider Management

#### Problem description

A user wants to perform Provider Management: The user needs to know which component has which type of contract. Details about the contracts like terms and conditions, SLAs, cost structure, penalties including history, contacts, escalation procedures and vendors are accessed.

#### Mapping to OGIT data


| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| User, Contacts | ogit/Person, ogit/Organization | entity | individual or group of individuals |
| Component | ogit/ITSM/Asset, ogit/Software/Application | entity | |
| which Asset to which Contract | ogit/ITSM/Asset - ogit/coveredBy -> ogit/Contract | entity, verb | |
| Contract | ogit/Contract | entity | |
| which component has which type of contract | ogit/ITSM/Asset - ogit/coveredBy -> ogit/Contract | entity, verb | |
| type of contract | ogit/contractType | attribute | specifies the type of an ogit/Contract entity. E.g. "HardwareContract" |
| SLA | ogit/ITService/SLA | entity | |
| cost structure | CostModel/* | several entities | e.g. CostModel/Budget, CostModel/CostElement |
| penalties | ogit/Penalty | entity | |
| vendors | ogit/Organization | entity | |
| terms and conditions | ogit/ITSM/License | entity | |
| which contract of which vendor | ogit/Contract - ogit/belongsTo -> ogit/Organization | entity, verb | |
| details of contract | ogit/Contract - ogit/contains => ogit/ITSM/License, ogit/ITService/SLA | entity, verb | | 
| which penalties | ogit/Contract - ogit/defines -> ogit/Penalty | entity, verb | |

### Use case 3: Network Planning

#### Problem description

User wants to plan technical networking architecture. Therefor he needs information about the network and application structure and usage (throughput, structure, error rates). Also he needs information from the vendor side about contracts and service.

#### Mapping to OGIT data

| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| User | ogit/Person | entity | |
| networking architecture | ogit/Network/* | entity | several entities |
| vendors | ogit/Organization | entity | |
| Contract | ogit/Contract | entity | |
| SLA | ogit/ITService/SLA | entity | |
| network and application structure I | ogit/Network/Port - ogit/connectsTo -> ogit/Network/NIC, ogit/Network/FCHBA | entity, verb | |
| network and application structure II | ogit/Network/Router - ogit/extends -> ogit/Network/SimpleDevice, ogit/Network/Card | entity, verb | |
| network and application structure III | ogit/Network/Shelf - ogit/has -> ogit/Network/Slot, ogit/Network/SimpleDevice - ogit/has -> ogit/Network/Port, ogit/Network/Card - ogit/has -> ogit/Network/Port | entity, verb | |

### Use case 4: Compare Architectures

#### Problem description

User wants to compare architectures and need to observe which software is used and how the software is used (users, transaction rates, user satisfaction, errors, service disruptions.

#### Mapping to OGIT data

| natural language term | OGIT element | OGIT type | remark |
| --- | --- | --- | --- |
| User | ogit/Person | entity | |
| Software | ogit/Software/Application | |
| transaction rates | ogit/Software/Application - ogit/hasTimeseries -> ogit/Timeseries | entity, verb | |
| observe which software is used | ogit/Software/Application - ogit/appearsAs <- arago/MARSNode | entity, verb | |
| observe which software is used | ogit/Software/Application - ogit/uses <- ogit/Person | entity, verb | |
| how the software is used | ogit/Software/Application - ogit/responsibleFor <- ogit/Person | entity, verb | responsibility of the software |
| how the software is used | ogit/Software/Application - ogit/owns <- ogit/Person | entity, verb | owner of the software |
| users, transaction rates, user satisfaction, errors, service disruptions | see use case 1 | entity, verb | see Ticket stuff of use case 1 |
