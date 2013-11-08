# Graph API Tutorial

This Tutorial shows you how to work with the API an how you can
put data into OGIT on the base of an example.

## The Service Example


## Creating NTO
First please get familiar with the basic concepts (https://github.com/arago/OGIT/wiki/Basic-Concepts) to understand what an NTO is. After this you have to create the NTO of the Service. A Service is an entity of global knowledge. The Service NTO is shown here. https://github.com/arago/OGIT/tree/master/NTO/Service

## Authenticate
To register send us an email. The registration is needed to authenticate against OGIT. With you username and password you will get an access token. This token is needed for further operations with the Graph API.

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' -H 'username:user' -H 'password:pw' 'https://graphit-test.arago.de/authenticate'

> REST (response)

    {"_TOKEN":"DLtVymWXhnem7pgyWsAd0lgnqPvbL0"}

## Create data into OGIT

## Update data in OGIT

## Delete data in OGIT

## Query data in OGIT