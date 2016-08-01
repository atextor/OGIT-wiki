# Using OGIT with Apache Tinkerpop

[Apache Tinkerpop](http://tinkerpop.apache.org/) is the De-facto standard abstraction for graph
databases in Java. It comes with the [Gremlin](http://tinkerpop.incubator.apache.org/gremlin.html)
query language. The most important parts of Tinkerpop are the API and the Gremlin language, but the
Apache project also provides a server implementation and a console (REPL). The following steps help
you to get started with OGIT in Tinkerpop using a simple in-memory graph, but for a production 
system, you might want to have a look at the Tinkerpop-compatible
[providers](http://tinkerpop.apache.org/providers.html#data-system-providers). Here we will start
the interactive, Groovy-based Gremlin shell, load OGIT into it and run a query. Note that for 
Tinkerpop 2.x, there exists an 
[adapter](https://github.com/tinkerpop/blueprints/wiki/Sail-Ouplementation) to OpenRDF’s SAIL 
interface, which allows the graph database to be accessed like a triple store. In this tutorial we 
will use Tinkerpop 3.x, which has an API that is incompatible with Tinkerpop 2.x. It’s still easy to 
import the RDF though, as you will see below.

1. We will use the [rapper](http://librdf.org/raptor/rapper.html) command-line utility to merge all
of OGIT’s turtle files into one file in N-Triples-Format. In a Debian-based system, you can install
it by running `apt-get install raptor2-utils`. Clone the OGIT repository, then in the OGIT directory
run:

	`rapper -i turtle -o ntriples <(find . -name '*.ttl' -exec cat "{}" \;) > ogit.nt`

2. Download and extract the [Apache Gremlin
   Console](https://www.apache.org/dyn/closer.lua/incubator/tinkerpop/3.2.0-incubating/apache-gremlin-console-3.2.0-incubating-bin.zip).

3. From the extracted directory, run:

	`bin/gremlin.sh`

	This opens the the Gremlin REPL.

4. Move or link the `ogit.nt` to the directory of the Gremlin console. Save the following script in
the same directory as `ogit.gr`. This Groovy script can be loaded by Gremlin and will insert OGIT's
RDF statements into an in-memory graph store. This is fine for testing purposes, for large use cases
you should consult [Gremlin’s 
documentation](http://tinkerpop.apache.org/docs/3.2.0-incubating/reference/).

	```groovy
	graph = TinkerGraph.open()
	g = graph.traversal()

	node = { id ->
		g.V().has(T.label, id).tryNext().orElseGet{ g.addV(T.label, id).next() }
	}

	new File('ogit.nt').eachLine {
		(s, p, o) = it.replaceAll(" \\.\$", "").split(" ", 3).collect { i -> i.substring(1, i.length() - 1) }
		node(p)
		if (!o.isEmpty()) {
			node(s).addEdge(p, node(o))
		}
	}
	```

5. In the Gremlin console, type:

	`:load ogit.gr`

6. Now we can query the graph. Enter the following query. This will list the names of the Entities
defined in OGIT.

	`g.V().has(T.label, "http://www.purl.org/ogit/Entity").in("http://www.w3.org/2000/01/rdf-schema#subClassOf").label()`

