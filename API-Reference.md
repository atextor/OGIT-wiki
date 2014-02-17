## Overview

The api reference contains an overview over all available GraphIT apis. Supported methods are:

* `authenticate`: authenticate against GraphIT and obtain an access token
* `get`: get a GraphIT object via id
* `create`: create a GraphIT object via type
* `update`: update an existing GraphIT object (MODIFY the node, add, overwrite and delete properties)
* `replace`: replace an existing GraphIT object (REPLACE the node, write the object as in the request)
* `delete`: delete an existing GraphIT object
* `connect`: connect two existing GraphIT objects via connection-type
* `query`: run queries against GraphIT

## REST

* All requests except `authenticate` must contain a valid access token `_TOKEN`. 
* All requests will be made against a base `$url` (e.g. `https://graphit-test.arago.de/`). 
* All responses are in JSON unless otherwise desired.
    * __JSON__ response format: set header `Accept` to `application/json`
    * __JSON__ request format (for requests with body): set header `Content-Type` to `application/json`
    * __YAML__ response format: set header `Accept` to `application/yaml`
    * __YAML__ request format (for requests with body): set header `Content-Type` to `application/yaml` 

### authenticate

    POST $url/authenticate
    headers: username, password
    body: [none]

    response: {"_TOKEN": "$TOKEN"}
    // NOTE a cookie _TOKEN will also be set
### get

    // get an object by id
    GET $url/$id
    headers: _TOKEN
    body: [none]

    response: {"_id": "$id", /* json attributes */}

    // get outgoing vertices of edge/verb $type
    GET $url/$id/$verb
    headers: _TOKEN
    body: [none]

    response: {"items": {/* json attributes */}}


### create

    POST $url/$type
    headers: _TOKEN, content-type
    
    // content-type header must be application/json
    body: {/* json attributes */}

    response: {"_id": "generated id", "_type": "$type", "_graph-type": "vertex", /* json attributes */}


### replace

    PUT $url/$id
    headers: _TOKEN
    // content-type header must be application/json
    body: {/* json attributes to update */}

    response: {"_id": "$id", /* json attributes as in the request body */}

### update

    POST $url/$id
    headers: _TOKEN
    // content-type header must be application/json
    // all properties in the body will be added or overwritten, to delete properties, set them to null, e.g.
    // {"a": null, "b": null, "c": "5"} will delete properties a and b, and add or overwrite property c to 5
    body: {/* json attributes to update */}

    response: {"_id": "$id", /* updated json attributes */}


### delete

    DELETE $url/$id
    headers: _TOKEN
    body: [none]

    response: {"_id": "$id", "_deleted": true /* json attributes */}

### connect

    POST $url/connect/$type
    headers: _TOKEN
    // content-type header must be application/json
    body: {"out":"id of the outgoing GraphIT object", "in": "id of the ingoing GraphIT object"}

    response: {"_id": "generated edge id", "_type": "$type", "_graph-type": "edge", /* ... */}

### query


    POST $url/query/$type?query=$queryString&parameter1=val1&parameter2=
    headers: _TOKEN
    body: [none]

    response: {"items": [{"_id": "...", /* json attributes */}, {"_id": "...", /* json attributes */}, ...]}

#### query types:

__gremlin__: `$url/query/gremlin?query=&root=` (see http://gremlindocs.com/)

    // MANDATORY root is the vertex id of the root vertex
    GET $url/query/gremlin?query=outE.inV&root=123
    headers: _TOKEN
    body: [none]

    response: {"items": [{"_id": "123", /* json attributes */}, {"_id": "...", /* json attributes */}, ...]}


__lucene__: `$url/query/vertices?query=` and  `$url/query/edges?query=` (see [query-parser syntax](http://lucene.apache.org/core/4_6_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#package_description), [es-query-string](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html))

    GET $url/query/vertices?query=+attribute:a +something:b
    headers: _TOKEN
    body: [none]

    response: {"items": [{"_id": "...", /* json attributes */, "_graphtype": "vertex|edge"}, {"_id": "...", /* json attributes */}, ...]}

__multi id query__: `$url/query/ids?query=id1,id2,id3,...` fetches multiple ids at once

    GET $url/query/ids?query=id1,id2,id3
    headers: _TOKEN
    body: [none]

    response: {"items": [{"_id": "id1", /* json attributes */}, {"_id": "...", /* json attributes */}, ...]}


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

* The GraphIT javascript can be loaded from GraphIT directly
* It works against the GraphIT REST endpoint: $url
* in order to use, an instance must be created:

<pre>
     &lt;script src="$url/graphit.js"&lt;/script>

     // this is a generic error handling function which does nothing except logging
     var handleError(error)
     {
       console.log(error);
       throw(error);
     };

    var graphit = new GraphIT('$url');

</pre>
After instantiation the javascript api can be used as follows (all calls are in context of `<script />`):

### authenticate

    graphit.authenticate({/* authentication parameters */}, function(ret)
    {
      if (ret.error) return handleError(error);
      
      // graphit now has a token as a cookie, it will automatically set it for all ongoing requests
    });

### get


    graphit.get($id, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });


### create

    
    graphit.create($type, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });


### update

    graphit.update($id, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });

### replace

    graphit.replace($id, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });

### delete

    graphit.del($id, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });

### connect

    graphit.connect($idOut, $idIn, $connectionType, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret);
    });

### query


    graphit.query($queryType, $queryString, {/* $queryParameters */}, function(ret)
    {
      if (ret.error) return handleError(error);
      
      console.log(ret.items);
    });

## GraphIT-CLI
<pre>
Usage: graphit-cli {authenticate|get|create|update|delete|query} [options] 
  Options:
    -t, --timeout
       zmq connection timeout
       Default: 1000
  * -u, --url
       zmq connection url
</pre>
### authenticate

<pre>
Usage: graphit-cli authenticate [options] 
  Options:
    -p
       authentication properties: -p USER=15
</pre>

### get
<pre>
Usage: graphit-cli get [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
  * -id
       the id to get
  * -token
       security token
</pre>

### create
<pre>
Usage: graphit-cli create [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
    -p
       object properties: -p id=15
       Syntax: -pkey=value
       Default: {}
  * -token
       security token
  * -type
       type of the graphit object
</pre>

### update
<pre>
Usage: graphit-cli update [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
  * -id
       id of the graphit object
    -p
       object properties: -p id=15
       Syntax: -pkey=value
       Default: {}
  * -token
       security token
</pre>

### update
<pre>
Usage: graphit-cli replace [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
  * -id
       id of the graphit object
    -p
       object properties: -p id=15
       Syntax: -pkey=value
       Default: {}
  * -token
       security token
</pre>

### delete
<pre>
Usage: graphit-cli delete [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
  * -id
       the id to delete
  * -token
       security token

</pre>

### connect
<pre>
Usage: graphit-cli connect [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
  * -in
       the id of the ingoing vertex
  * -out
       the id of the outgoing vertex
  * -token
       security token
  * -type
       the type of the connection
</pre>

### query
<pre>
Usage: graphit-cli query [options] 
  Options:
        --format
       output format (yaml|json)
       Default: json
    -p
       query parameter: -p id=15
       Syntax: -pkey=value
       Default: {}
  * -query
       content of the query
  * -token
       security token
  * -type
       type of the query

</pre>
