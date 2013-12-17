# Overview

The first part of the tutorial covers how one can access data stored in GraphIT. The test service resides at `https://graphit-test.arago.de/`.
REST explains how the service is used via curl (via http), CLI explains how the service is used with the graphit-cli (via zmq).

## Authenticating

To register send an email to ogit@arago.de. Registration is required to be able authenticate against OGIT. With your username and password you will receive an access token. This token is needed for further operations with the Graph API.

> REST (request)

    curl -X POST  -H 'username:user' -H 'password:pw' 'https://graphit-test.arago.de/authenticate'

> REST (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}

> REST (request, plain)

    curl -X POST  -H 'username:user' -H 'password:pw' 'https://graphit-test.arago.de/authenticate/plain'


> REST (response, plain, can be used to obtain token as: TOKEN=`curl .../plain`; echo $TOKEN)

    DLtVymWXhnem7pgyWsAd0lgnqPvbL0


> CLI (request)

    graphit-cli authenticate -u tcp://graphit-test.tech.arago.de:7290 -p 'username:user' -p 'password:pw'

> CLI (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}    

This token can now be used in all subsequent requests. For brevity it will be abbreviated as `$TOKEN`.    

## Getting Items

Any item can be retrieved by its id, for example the identity of the tutorial user graphit-tutorial@arago.de:

> REST (request)

    curl -X GET  'https://graphit-test.arago.de/graphit-tutorial@arago.de?_TOKEN=$TOKEN'

> REST (response)

    {"_type":"GraphITIdentity","_creator":"graphit-tutorial@arago.de","_id":"graphit-tutorial@arago.de","_graphtype":"vertex","_deleted":false}


> CLI (request)

    graphit-cli get tcp://graphit-test.tech.arago.de:7290 -token $TOKEN -id graphit-tutorial@arago.de --format yaml

> CLI (response, yaml)

    {_type: GraphITIdentity, _creator: graphit-tutorial@arago.de, _id: graphit-tutorial@arago.de, _graphtype: vertex, _deleted: false}    


## Listing Items

Listing items can be done by running a [gremlin query](https://gremlindocs.com/), in this example we get the identity vertex and list all outgoing edges/vertices of graphit-tutorial@arago.de: `outE`.

> REST (request, NOTE `root` is passed as a parameter with a placeholder, the root is the id of the root vertex where the query starts, and it must be present)

    curl -X GET  'https://graphit-test.arago.de/query/gremlin?query=outE&root=graphit-tutorial@arago.de&_TOKEN=$TOKEN'

> REST (response)

    {"items":[{"_in-id":"81f34412-8431-4581-97ad-09beeff047e9","_type":"created","_edge-id":"graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"},{"_in-id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","_type":"created","_edge-id":"graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"}]}


> CLI (request, NOTE `id` is passed as a parameter with a placeholder)

    graphit-cli query -u tcp://graphit-test.tech.arago.de:7290 -type gremlin -query 'outE' -p 'root=graphit-tutorial@arago.de' -token $TOKEN

> CLI (response, json)

    [{"_in-id":"81f34412-8431-4581-97ad-09beeff047e9","_type":"created","_edge-id":"graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"},{"_in-id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","_type":"created","_edge-id":"graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"}]  

> CLI (response, yaml)

    - {_in-id: 81f34412-8431-4581-97ad-09beeff047e9, _type: created, _edge-id: graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9, _graphtype: edge, _out-id: graphit-tutorial@arago.de}
    - {_in-id: b8422c98-e074-4304-8d3f-bcf1e49b686c, _type: created, _edge-id: graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c, _graphtype: edge, _out-id: graphit-tutorial@arago.de}

or listing any vertex, that the tutorial user has created `outE.inV`:

> REST (request)

    curl -X GET  'https://graphit-test.arago.de/query/gremlin?query=outE.inV&root=graphit-tutorial@arago.de&_TOKEN=$TOKEN'

> REST (response)

    {"items":[{"doors":"5","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"81f34412-8431-4581-97ad-09beeff047e9","color":"red","_graphtype":"vertex","_deleted":false,"_modified-on":1381843442494,"_created-on":1381843442476},{"doors":"3","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","color":"yellow","_graphtype":"vertex","_deleted":false,"_modified-on":1381843449109,"_created-on":1381843449107}]}

## Where to go from here

* [Learn how to get data into GraphIT](Getting-Started-Part-2)