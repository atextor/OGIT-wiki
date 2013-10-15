## Overview

In this tutorial we assume that we have a type `Car` which has two properties: `color` of the car and number of `doors`, as well as a type `Driver` which has a `name` and can `drive` cars:

## Create 

The following request creates a `green` `Car` with `4` doors:

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' 'http://graphit-test.tech.arago.de/Car?_TOKEN=$TOKEN' -d '{"color":"green", "doors": "4"}'

> REST (response)

    {"doors":"4","_type":"Car","_id":"b87bad04-f23d-47c9-8b7d-12b3b4994f20","_creator":"graphit-tutorial@arago.de","color":"green","_graphtype":"vertex","_deleted":false,"_modified-on":1381845426211,"_created-on":1381845426210}


## Update

This request updates the `color` of the car to `red`:

> REST (request)

    curl -X PUT -H 'GraphIT-Version:4.2-SNAPSHOT' 'http://graphit-test.tech.arago.de/b87bad04-f23d-47c9-8b7d-12b3b4994f20?_TOKEN=$TOKEN' -d '{"color":"red"}'

> REST (response)

    {"doors":"4","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"b87bad04-f23d-47c9-8b7d-12b3b4994f20","color":"red","_graphtype":"vertex","_deleted":false,"_modified-on":1381845614874,"_created-on":1381845426210}

## Delete

This request deletes the car: 

> REST (request)

    curl -X DELETE -H 'GraphIT-Version:4.2-SNAPSHOT' 'http://graphit-test.tech.arago.de/b87bad04-f23d-47c9-8b7d-12b3b4994f20?_TOKEN=$TOKEN'

> REST (response)

    {"doors":"4","_type":"Car","_creator":"graphit-tutorial@arago.de","_id":"b87bad04-f23d-47c9-8b7d-12b3b4994f20","color":"red","_graphtype":"vertex","_deleted-on":1381845706465,"_deleted":true,"_modified-on":1381845614874,"_created-on":1381845426210}

## Connect

This request creates a connection of type `drives` between the `Driver` (id: $driver) and a `Car` (id: $car):

> REST (request)

    curl -X POST -H 'GraphIT-Version:4.2-SNAPSHOT' 'http://graphit-test.tech.arago.de/connect/drives?_TOKEN=$TOKEN' -d '{"out": $driver, "in": $car}'

> REST (response)

    {"_in-id":"$car","_type":"drives","_edge-id":"3c7469b0-0b8c-424c-a73c-870ab164f385_0e59c3f8-86ba-4064-bd54-c47f71edb4da_b87bad04-f23d-47c9-8b7d-12b3b4994f20","_creator":"graphit-tutorial@arago.de","_graphtype":"edge","_out-id":"$driver","_deleted":false,"_created-on":1381846115968}

The connection can now also be retrieved via `http://graphit-test.tech.arago.de/3c7469b0-0b8c-424c-a73c-870ab164f385_0e59c3f8-86ba-4064-bd54-c47f71edb4da_b87bad04-f23d-47c9-8b7d-12b3b4994f20`.
