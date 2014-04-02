# Graph API Tutorial

This Tutorial shows you how to work with the API and how to 
put data into OGIT based on a set of examples.

## Introduction Service Example
The _service example_ demonstrates the whole OGIT process. A list of almost one hundred services will be posted to OGIT in this tutorial. The services file is found here [service_list.xlsx](https://github.com/arago/OGIT/blob/master/examples/service_list.xlsx).

## Creating an NTO
First please get familiar with the [basic concepts] (https://github.com/arago/OGIT/wiki/Basic-Concepts) to understand what an NTO is. After this the NTO of the service has to be created. A service is an entity of global knowledge. The Service NTO is shown here [Service NTO] (https://github.com/arago/OGIT/tree/master/NTO/Service).

## Authenticate
To register send an email to __ogit@arago.de__. Registration is required to be able authenticate against OGIT. With your username and password you will receive an access token. This token is needed for further operations with the Graph API.

TODO provide info for authentication flow

## Create data in OGIT

The following request creates a `service` with the `/Level=IaaS` and a `/Name=Compute x86`.
$TOKEN is your personal access token you have get in the authenticate response.
If the format of your services is in cvs or similar you can use the [Mr. Data Converter] (http://shancarter.github.io/mr-data-converter) to convert the payload into JSON.
To store all the services in OGIT the converter supports the generation of the following curl shell-script: [curl_post_services.sh] (https://github.com/arago/OGIT/blob/master/examples/curl_post_services.sh)

> REST (request)

    curl -X POST 'https://graphit-test.arago.de/new/arago%2fService?_TOKEN='$TOKEN -d '{"/Level":"IaaS", "/Name": "Compute x86"}'

> REST (response)

    {"ogit/_type":"arago/Service","/Level":"IaaS","ogit/_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","ogit/_creator":"graphit-tutorial@arago.de","ogit/_owner":"graphit-tutorial@arago.de","ogit/_graphtype":"vertex","/Name":"Compute x86","ogit/_is-deleted":false,"ogit/_modified-on":1384027732695,"ogit/_created-on":1384027732688}

The response is described in the [API-Reference] (https://github.com/arago/OGIT/wiki/API-Reference) under `create`.

## Update data in OGIT

This request updates the `Name` of the service to `Compute SPARC`:

> REST (request)

    curl -X POST 'https://graphit-test.arago.de/99c5f2e4-0113-40f7-961a-8f49b633ca2e?_TOKEN='$TOKEN -d '{"/Name":""Compute SPARC"}'

> REST (response)

    {"ogit/_type":"arago/Service","/Level":"IaaS","ogit/_creator":"graphit-tutorial@arago.de","ogit/_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","ogit/_owner":"graphit-tutorial@arago.de","ogit/_graphtype":"vertex","/Name":"Compute SPARC","ogit/_is-deleted":false,"ogit/_modified-on":1384030207505,"ogit/_created-on":1384027732688}

## Delete data in OGIT

This request deletes the service: 

> REST (request)

    curl -X DELETE 'https://graphit-test.arago.de/99c5f2e4-0113-40f7-961a-8f49b633ca2e?_TOKEN='$TOKEN

> REST (response)

    {"ogit/_type":"arago/Service","/Level":"IaaS","ogit/_creator":"graphit-tutorial@arago.de","ogit/_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","ogit/_owner":"graphit-tutorial@arago.de","ogit/_graphtype":"vertex","/Name":"Compute SPARC","ogit/_is-deleted-on":1384030663012,"ogit/_is-deleted":true,"ogit/_modified-on":1384030207505,"ogit/_created-on":1384027732688}

## Query data in OGIT

You can run a [gremlin query](http://gremlindocs.com/) to list items in OGIT. The following query searches all the entities that `/Name` is `Memory SPARC` and `owned` by the user with the `id=username`.

> REST (request)

    curl -X GET 'https://graphit-test.arago.de/query/gremlin?query=outE.has("label",label).inV.has("/Name",name)&root=username&_TOKEN=$TOKEN&name=Memory%20SPARC&label=_owns'

> REST (response)

    {"items":[{"/Name":"Memory SPARC","ogit/_type":"arago/Service","ogit/_id":"ba1252d2-2022-471b-8d66-32174ae599b9","ogit/_creator":"graphit-tutorial@arago.de","ogit/_owner":"graphit-tutorial@arago.de","ogit/_graphtype":"vertex","/Level":"IaaS","ogit/_is-deleted":false,"ogit/_modified-on":1384033807273,"ogit/_created-on":1384033807271}]}

For further and detailed gremlin queries please look at the [API-Reference] (https://github.com/arago/OGIT/wiki/API-Reference) under `Listing items`.
The gremlin methods are listed [here] (https://github.com/tinkerpop/gremlin/wiki/Gremlin-Steps).