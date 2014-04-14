![graphit-logo](https://github.com/arago/OGIT/raw/master/docs/images/graphit-logo.png)

The api reference contains an overview over all available GraphIT apis. 
All attributes not starting with ogit/_ must be of type string.

Supported methods are:

* `get`: get a GraphIT object via id
* `create`: create a GraphIT object via type
* `update`: update an existing GraphIT object (MODIFY the node, add, overwrite and delete properties)
* `replace`: replace an existing GraphIT object (REPLACE the node, write the object as in the request)
* `delete`: delete an existing GraphIT object
* `connect`: connect two existing GraphIT objects via connection-type
* `stream`: stream GraphIT events
* `query`: run queries against GraphIT

## Namespace

All type ids (attribute ids, entity ids, verb ids, ...) in GraphIT are namespaced. To ensure consistency and avoid ambiguity, these have a mandatory prefix `http://www.purl.org/`. Everything after this prefix is the shortened id, that will be used by graphit, e.g. `ogit/_id` derives from http://www.purl.org/ogit/_id. (see http://www.purl.org/docs/index.html for further info).
Attributes, that are not defined in OGIT have an empty namespace, e.g.: `/IssueXML`.
(sidenote: the properties `id` and `label` can never be used due to storage limitations).

## REST

* All requests must contain a valid access token `_TOKEN`. 
* All requests will be made against a base `$url` (e.g. `https://graphit-test.arago.de/`). 
* All responses are in JSON
    * __JSON__ response format: set header `Accept` to `application/json`
    * __JSON__ request format (for requests with body): set header `Content-Type` to `application/json`

### get

    // get an object by id
    GET $url/$id
    headers: _TOKEN
    body: [none]

    response: {"ogit/_id": "$id", /* json attributes */}

    // get outgoing vertices of edge/verb $type
    // optional parameter direction=in|out
    GET $url/$id/$verb
    headers: _TOKEN
    body: [none]

    response: {"items": {/* json attributes */}}

    // get timeseries values of types that support it
    // from=timestamp in ms
    // to=timestamp in ms 
    GET $url/$id/values?from=&to=
    headers: _TOKEN
    body: [none]

    response: {"items": {/* timeseries values */}}

Appending `?metadata=true` to a `GET` for a vertex will return metadata about the vertex.

### create

    POST $url/new/$type
    headers: _TOKEN, content-type
    
    // content-type header must be application/json
    // you can also pass an own ogit/_id
    body: {/* json attributes */}

    response: {"ogit/_id": "generated id", "ogit/_type": "$type", "ogit/_graph-type": "vertex", /* json attributes */}


### replace

    PUT $url/$id
    headers: _TOKEN
    // content-type header must be application/json
    body: {/* json attributes to update */}

    response: {"ogit/_id": "$id", /* json attributes as in the request body */}

### update

    POST $url/$id
    headers: _TOKEN
    // content-type header must be application/json
    // all properties in the body will be added or overwritten, to delete properties, set them to null, e.g.
    // {"a": null, "b": null, "c": "5"} will delete properties a and b, and add or overwrite property c to 5
    body: {/* json attributes to update */}

    response: {"ogit/_id": "$id", /* updated json attributes */}

    // write timeseries value for types that support it
    POST $url/$id/values
    headers: _TOKEN
    body: {"value": "timeseries value", "timestamp": timestampInMs}

    response: {}


### delete

    DELETE $url/$id
    headers: _TOKEN
    body: [none]

    response: {"ogit/_id": "$id", "ogit/_is-deleted": true /* json attributes */}

### connect

    POST $url/connect/$type
    headers: _TOKEN
    // content-type header must be application/json
    body: {"out":"id of the outgoing GraphIT object", "in": "id of the ingoing GraphIT object"}

    response: {"ogit/_id": "generated edge id", "ogit/_type": "$type", "ogit/_graph-type": "edge", /* ... */}


### stream


    // stream events from graphit
    GET $url/stream
    headers: _TOKEN
    body: [none]

    // the response is a newline (\n) separated list of events
    response: {"identity":"id of the identity","action":"type, e.g. CREATE","element":{/* properties like ogit/_id*/}}\n/* more \n-separated events */


### query


    POST $url/query/$type?query=$queryString&parameter1=val1&parameter2=
    headers: _TOKEN
    body: [none]

    response: {"items": [{"ogit/_id": "...", /* json attributes */}, {"ogit/_id": "...", /* json attributes */}, ...]}

#### query types:

__gremlin__: `$url/query/gremlin?query=&root=` (see http://gremlindocs.com/)

    // MANDATORY root is the vertex id of the root vertex
    GET $url/query/gremlin?query=outE.inV&root=123
    headers: _TOKEN
    body: [none]

    response: {"items": [{"ogit/_id": "123", /* json attributes */}, {"ogit/_id": "...", /* json attributes */}, ...]}

gremlin can use placeholders: `$url/query/gremlin?query=has('attribute',var1)&root=&var1=value` whereas var1 is the placeholder vor value. 

__lucene__: `$url/query/vertices?query=` and  `$url/query/edges?query=` (see [query-parser syntax](http://lucene.apache.org/core/4_6_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#package_description), [es-query-string](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html))

    GET $url/query/vertices?query=+attribute:a +something:b&limit=10&offset=0&order=a asc, b desc
    /*
       optional parameters:
       limit  = limit of results to return
       offset = offset of results
       order  = how to order search results: prop asc|desc[, prop1 asc|desc]
       fields = comma-separated list of attributes to fetch
    */

    headers: _TOKEN
    body: [none]

    response: {"items": [{"ogit/_id": "...", /* json attributes */, "_graphtype": "vertex|edge"}, {"ogit/_id": "...", /* json attributes */}, ...]}

lucene can use placeholders: `$url/query/vertices?query=+field:$var1 +anotherfield:"$var2"&var1=value&var2=value2` whereas var1 is the placeholder for value1 and var2 the placeholder value2. placeholders are very useful, because escaping is done automatically.


__multi id query__: `$url/query/ids?query=id1,id2,id3,...` fetches multiple ids at once

    GET $url/query/ids?query=id1,id2,id3
    headers: _TOKEN
    body: [none]

    response: {"items": [{"ogit/_id": "id1", /* json attributes */}, {"ogit/_id": "...", /* json attributes */}, ...]}


### graphit.js

    GET $url/graphit.js
    headers: [none]
    body: [none]

    response: /* GraphIT js api*/


### Batch Requests

Batch requests are request executed in one transaction. Batch requests are `0`-indexed, so the first request has the index `0`.

    POST $url/batch
    headers: _TOKEN
    // content-type header must be application/json
    body: {"requests": [{/* first batch request */}, {/* second batch request *//*, ... */}]}

    response: {"items": [{/* result of first batch request */}, {/* result of second batch request */}/*, ...*/]}


Available request types are: `GET`, `CONNECT`, `CREATE`, `UPDATE`, `REPLACE`, `DELETE`. 
Batch requests support backreferencing, values may contain a relative or absolute backreference:

* absolute: `${15}` is the 15th request in the sent request list
* relative: `${-2}` is the -2 request in the sent request list from the current request

e.g.: 

    CREATE x
    GET    Y
    CONNECT ${0} ${-1}
    
    // will connect x->y
    // these are the same only with different references
    // CONNECT ${0} ${1} - both absolutely referenced
    // CONNECT ${-2} ${-1} - both relatively referenced
    // CONNECT ${-2} ${1} - like ${0} ${-1}
    

### Batch GET

      {
          "type": "GET",
          "parameters": {
              "ogit/_id": "vertex-id"
          }
      }

### Batch CONNECT

      {
          "type": "CONNECT",
          "body": {
              "ogit/_out-id": "out-vertex-id",
              "ogit/_in-id": "in-vertex-id",
              "ogit/_type": "connect-type-id"
          }
      }

### Batch CREATE

      {
          "type": "CREATE",
          "parameters": {
              "ogit/_type": "entity-type-id"
          },
          "body": {
              /* "ogit/_id": "user-defined-id", */
              "/some-property": "value"
          }
      }

### Batch UPDATE

      {
          "type": "UPDATE",
          "parameters": {
              "ogit/_id": "vertex-id"
          },
          "body": {
              /* "ogit/_id": "user-defined-id", */
              "/some-property": "value"
          }
      }

### Batch REPLACE

      {
          "type": "REPLACE",
          "parameters": {
              "ogit/_id": "vertex-id"
          },
          "body": {
              /* "ogit/_id": "user-defined-id", */
              "/some-property": "value"
          }
      }

### Batch DELETE

      {
          "type": "DELETE",
          "parameters": {
              "ogit/_id": "$id"
          }
      }



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

### get


    graphit.get($id, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });


### create

    
    graphit.create($type, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });


### update

    graphit.update($id, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });

### replace

    graphit.replace($id, {/* attributes */}, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });

### delete

    graphit.del($id, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });

### connect

    graphit.connect($idOut, $idIn, $connectionType, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret);
    });

### query


    graphit.query($queryType, $queryString, {/* $queryParameters */}, function(ret)
    {
      if (ret.error) return handleError(ret.error);
      
      console.log(ret.items);
    });