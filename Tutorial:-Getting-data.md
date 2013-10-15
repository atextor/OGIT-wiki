# Overview

This tutorial covers how one can access data stored in GraphIT. The test service resides at `https://graphit-test.tech.arago.de/`.
REST explains how the service is used via curl (via http), CLI explains how the service is used with the graphit-cli (via zmq).

## Authenticating

In order to use GraphIT an access token has to be obtained. The `authenticate` endpoint is called with the user `graphit-tutorial@arago.de` and the password `0xDEADBEEF`.

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' -H 'USER:graphit-tutorial@arago.de' -H 'PASSWORD:0xDEADBEEF' 'https://graphit-test.tech.arago.de/authenticate'

> REST (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}


> CLI (request)

    graphit-cli authenticate -u tcp://graphit-test.tech.arago.de:7290 -p 'USER=graphit-tutorial@arago.de' -p 'PASSWORD=0xDEADBEEF'

> CLI (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}    

This token can now be used in all subsequent requests. For brevity it will be abbreviated as `$TOKEN`.    

## Getting Items

Any item can be retrieved by its id, for example the identity of the tutorial user:

> REST (request)

    curl -X GET -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.tech.arago.de/graphit-tutorial@arago.de?_TOKEN=$TOKEN'

> REST (response)

    {"_type":"GraphITIdentity","_creator":"graphit-tutorial@arago.de","_id":"graphit-tutorial@arago.de","_graphtype":"vertex","_deleted":false}


> CLI (request)

    graphit-cli get tcp://graphit-test.tech.arago.de:7290 -token $TOKEN -id graphit-tutorial@arago.de --format yaml

> CLI (response, yaml)

    {_type: GraphITIdentity, _creator: graphit-tutorial@arago.de, _id: graphit-tutorial@arago.de, _graphtype: vertex, _deleted: false}    


## Listing Items

Listing items can be done by running a [gremlin query](https://gremlindocs.com/), in this example we get the identity vertex and list all outgoing edges/vertices `g.V("_id", "graphit-tutorial@arago.de").outE`.

> REST (request, NOTE `id` is passed as a parameter with a placeholder)

    curl -X GET -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.tech.arago.de/query/gremlin?query=g.V("_id",id).outE&id=graphit-tutorial@arago.de&_TOKEN=$TOKEN'

> REST (response)

    {"items":[{"_in-id":"81f34412-8431-4581-97ad-09beeff047e9","_type":"created","_edge-id":"graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"},{"_in-id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","_type":"created","_edge-id":"graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"}]}


> CLI (request, NOTE `id` is passed as a parameter with a placeholder)

    graphit-cli query -u tcp://graphit-test.tech.arago.de:7290 -type gremlin -query 'g.V("_id", id).outE' -p 'id=graphit-tutorial@arago.de' -token $TOKEN

> CLI (response, json)

    [{"_in-id":"81f34412-8431-4581-97ad-09beeff047e9","_type":"created","_edge-id":"graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"},{"_in-id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","_type":"created","_edge-id":"graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c","_graphtype":"edge","_out-id":"graphit-tutorial@arago.de"}]  

> CLI (response, yaml)

    - {_in-id: 81f34412-8431-4581-97ad-09beeff047e9, _type: created, _edge-id: graphit-tutorial@arago.de_4e0da517-db73-4b98-8069-29cc61100c6f_81f34412-8431-4581-97ad-09beeff047e9, _graphtype: edge, _out-id: graphit-tutorial@arago.de}
    - {_in-id: b8422c98-e074-4304-8d3f-bcf1e49b686c, _type: created, _edge-id: graphit-tutorial@arago.de_4a098821-51d8-448a-97b5-c6491abe051a_b8422c98-e074-4304-8d3f-bcf1e49b686c, _graphtype: edge, _out-id: graphit-tutorial@arago.de}

or listing any vertex, that the tutorial user has created `g.V("_id", "graphit-tutorial@arago.de").outE.inV`:

> REST (request, NOTE `id` is passed as a parameter with a placeholder)

    curl -X GET -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.tech.arago.de/query/gremlin?query=g.V("_id",id).outE.inV&id=graphit-tutorial@arago.de&_TOKEN=$TOKEN'

> REST (response)

    {"items":[{"doors":"5","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"81f34412-8431-4581-97ad-09beeff047e9","color":"red","_graphtype":"vertex","_deleted":false,"_modified-on":1381843442494,"_created-on":1381843442476},{"doors":"3","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"b8422c98-e074-4304-8d3f-bcf1e49b686c","color":"yellow","_graphtype":"vertex","_deleted":false,"_modified-on":1381843449109,"_created-on":1381843449107}]}

## Where to go from here

* review basic aspects -> basics
* learn how to put data into GraphIT