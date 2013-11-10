# Graph API Tutorial

This Tutorial shows you how to work with the API an how you can
put data into OGIT on the base of an example.

## Introduction Service Example
The service example demonstrates you the whole OGIT process. A list of nearly hundred services will be posted to OGIT in this tutorial. The file of the services is found here [service_list.xlsx](https://github.com/arago/OGIT/blob/master/examples/service_list.xlsx).

## Creating NTO
First please get familiar with the basic concepts (https://github.com/arago/OGIT/wiki/Basic-Concepts) to understand what an NTO is. After this you have to create the NTO of the Service. A Service is an entity of global knowledge. The Service NTO is shown here [Service NTO] (https://github.com/arago/OGIT/tree/master/NTO/Service).

## Authenticate
To register send an email to ogit@arago.de. The registration is needed to authenticate against OGIT. With you username and password you will get an access token. This token is needed for further operations with the Graph API.

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' -H 'username:user' -H 'password:pw' 'https://graphit-test.arago.de/authenticate'

> REST (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}

## Create data into OGIT

The following request creates a `service` with the `level=IaaS` and a `name=Compute x86`.
$TOKEN is your personal access token you have get in the authenticate response.
If the format of your services is in cvs or similar you can use the [Mr. Data Converter] (http://shancarter.github.io/mr-data-converter) to convert the payload into json.
To store all the services into OGIT the converter supported the generation of the following curl shell-script [curl_post_services.sh] (https://github.com/arago/OGIT/blob/master/examples/curl_post_services.sh)

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.arago.de/Service?_TOKEN=$TOKEN' -d '{"level":"IaaS", "name": "Compute x86"}'

> REST (response)

    {"_type":"Service","level":"IaaS","_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","_creator":"graphit-tutorial@arago.de","_owner":"graphit-tutorial@arago.de","_graphtype":"vertex","name":"Compute x86","_deleted":false,"_modified-on":1384027732695,"_created-on":1384027732688}

The response is described in https://github.com/arago/OGIT/wiki/API-Reference under `create`.

## Update data in OGIT

This request updates the `name` of the service to `Compute SPARC`:

> REST (request)

    curl -X PUT -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.arago.de/99c5f2e4-0113-40f7-961a-8f49b633ca2e?_TOKEN=$TOKEN' -d '{"name":""Compute SPARC"}'

> REST (response)

    {"_type":"Service","level":"IaaS","_creator":"graphit-tutorial@arago.de","_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","_owner":"graphit-tutorial@arago.de","_graphtype":"vertex","name":"Compute SPARC","_deleted":false,"_modified-on":1384030207505,"_created-on":1384027732688}

## Delete data in OGIT

This request deletes the service: 

> REST (request)

    curl -X DELETE -H 'GraphIT-Version:4.2-SNAPSHOT' 'https://graphit-test.arago.de/99c5f2e4-0113-40f7-961a-8f49b633ca2e?_TOKEN=$TOKEN'

> REST (response)

    {"_type":"Service","level":"IaaS","_creator":"graphit-tutorial@arago.de","_id":"99c5f2e4-0113-40f7-961a-8f49b633ca2e","_owner":"graphit-tutorial@arago.de","_graphtype":"vertex","name":"Compute SPARC","_deleted-on":1384030663012,"_deleted":true,"_modified-on":1384030207505,"_created-on":1384027732688}

## Query data in OGIT

You can run a gremlin query to list items in OGIT. Please look at the  https://github.com/arago/OGIT/wiki/API-Reference under `Listing items`.
The gremlin methods are listed here https://github.com/tinkerpop/gremlin/wiki/Gremlin-Steps.