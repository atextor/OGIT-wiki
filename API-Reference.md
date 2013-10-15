## Overview

The api reference contains an overview over all available GraphIT apis. Supported methods are:

* `authenticate`: authenticate against GraphIT and obtain an access token
* `get`: get a GraphIT object via id
* `create`: create a GraphIT object via type
* `update`: update an existing GraphIT object
* `delete`: delete an existing GraphIT object
* `connect`: connect two existing GraphIT objects via connection-type
* `query`: run queries against GraphIT

## REST

* All REST requests must contain a GraphIT version header: `GraphIT-Version`.
* All requests except `authenticate` must contain a valid access token `_TOKEN`. 
* All requests will be made against a base `$url` (e.g. `http://graphit-example.tech.arago.de/`). 
* All responses are in JSON.

### authenticate

    POST $url/authenticate
    headers: GraphIT-Version, USER, PASSWORD
    body: [none]

    response: {"_TOKEN": "$TOKEN"}

### get


    GET $url/$id
    headers: GraphIT-Version, _TOKEN
    body: [none]

    response: {"_id": "$id", /* json attributes */}


### create

    POST $url/$type
    headers: GraphIT-Version, _TOKEN
    body: {/* json attributes */}

    response: {"_id": "generated id", "_type": "$type", "_graph-type": "vertex", /* json attributes */}


### update

    PUT $url/$id
    headers: GraphIT-Version, _TOKEN
    body: {/* json attributes to update */}

    response: {"_id": "$id", /* updated json attributes */}

### delete

    DELETE $url/$id
    headers: GraphIT-Version, _TOKEN
    body: [none]

    response: {"_id": "$id", "_deleted": true /* json attributes */}

### connect

    POST $url/connect/$type
    headers: GraphIT-Version, _TOKEN
    body: {"out":"id of the outgoing GraphIT object", "in": "id of the ingoing GraphIT object"}

    response: {"_id": "generated edge id", "_type": "$type", "_graph-type": "edge", /* ... */}

### query


    POST $url/query/$type?query=$queryString&parameter1=val1&parameter2=
    headers: GraphIT-Version, _TOKEN
    body: [none]

    response: {"items": [{"_id": "...", /* json attributes */}, {"_id": "...", /* json attributes */}, ...]}

### help

    GET $url/help
    headers: [none]
    body: [none]

    response: {/* json help file */}

### graphit.js

    GET $url/graphit.js
    headers: [none]
    body: [none]

    response: /* GraphIT js api*/

## Javascript

The GraphIT javascript can be loaded from GraphIT directly:

    <!doctype html>
      <head>
        <script src="$url/graphit.js"></script>
      </head>
      <!-- -->

## GraphIT-CLI