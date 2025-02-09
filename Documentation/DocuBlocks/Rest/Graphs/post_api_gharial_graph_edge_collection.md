@startDocuBlock post_api_gharial_graph_edge_collection

@RESTHEADER{POST /_api/gharial/{graph}/edge/{collection}, Create an edge, createEdge}

@RESTDESCRIPTION
Creates a new edge in the specified collection.
Within the body the edge has to contain a `_from` and `_to` value referencing to valid vertices in the graph.
Furthermore the edge has to be valid in the definition of the used edge collection.

@RESTURLPARAMETERS

@RESTURLPARAM{graph,string,required}
The name of the graph.

@RESTURLPARAM{collection,string,required}
The name of the edge collection the edge belongs to.

@RESTQUERYPARAMETERS

@RESTQUERYPARAM{waitForSync,boolean,optional}
Define if the request should wait until synced to disk.

@RESTQUERYPARAM{returnNew,boolean,optional}
Define if the response should contain the complete
new version of the document.

@RESTBODYPARAM{_from,string,required,}
The source vertex of this edge. Has to be valid within
the used edge definition.

@RESTBODYPARAM{_to,string,required,}
The target vertex of this edge. Has to be valid within
the used edge definition.

@RESTRETURNCODES

@RESTRETURNCODE{201}
Returned if the edge could be created and waitForSync is true.

@RESTREPLYBODY{error,boolean,required,}
Flag if there was an error (true) or not (false).
It is false in this response.

@RESTREPLYBODY{code,integer,required,}
The response code.

@RESTREPLYBODY{edge,object,required,edge_representation}
The internal attributes for the edge.

@RESTREPLYBODY{new,object,optional,edge_representation}
The complete newly written edge document.
Includes all written attributes in the request body
and all internal attributes generated by ArangoDB.
Will only be present if returnNew is true.

@RESTRETURNCODE{202}
Returned if the request was successful but waitForSync is false.

@RESTREPLYBODY{error,boolean,required,}
Flag if there was an error (true) or not (false).
It is false in this response.

@RESTREPLYBODY{code,integer,required,}
The response code.

@RESTREPLYBODY{edge,object,required,edge_representation}
The internal attributes for the edge.

@RESTREPLYBODY{new,object,optional,edge_representation}
The complete newly written edge document.
Includes all written attributes in the request body
and all internal attributes generated by ArangoDB.
Will only be present if returnNew is true.

@RESTRETURNCODE{400}
Returned if the input document is invalid.
This can for instance be the case if the `_from` or `_to` attribute is missing
or malformed.

@RESTREPLYBODY{error,boolean,required,}
Flag if there was an error (true) or not (false).
It is true in this response.

@RESTREPLYBODY{code,integer,required,}
The response code.

@RESTREPLYBODY{errorNum,integer,required,}
ArangoDB error number for the error that occurred.

@RESTREPLYBODY{errorMessage,string,required,}
A message created for this error.

@RESTRETURNCODE{403}
Returned if your user has insufficient rights.
In order to insert edges into the graph  you at least need to have the following privileges:

1. `Read Only` access on the Database.
2. `Write` access on the given collection.

@RESTREPLYBODY{error,boolean,required,}
Flag if there was an error (true) or not (false).
It is true in this response.

@RESTREPLYBODY{code,integer,required,}
The response code.

@RESTREPLYBODY{errorNum,integer,required,}
ArangoDB error number for the error that occurred.

@RESTREPLYBODY{errorMessage,string,required,}
A message created for this error.

@RESTRETURNCODE{404}
Returned in any of the following cases:
* no graph with this name could be found.
* the edge collection is not part of the graph.
* the vertex collection referenced in the `_from` or `_to` attribute is not part of the graph.
* the vertex collection is part of the graph, but does not exist.
* `_from` or `_to` vertex does not exist.

@RESTREPLYBODY{error,boolean,required,}
Flag if there was an error (true) or not (false).
It is true in this response.

@RESTREPLYBODY{code,integer,required,}
The response code.

@RESTREPLYBODY{errorNum,integer,required,}
ArangoDB error number for the error that occurred.

@RESTREPLYBODY{errorMessage,string,required,}
A message created for this error.

@EXAMPLES

@EXAMPLE_ARANGOSH_RUN{HttpGharialAddEdge}
  var examples = require("@arangodb/graph-examples/example-graph.js");
~ examples.dropGraph("social");
~ require("internal").db._drop("relation");
~ require("internal").db._drop("female");
~ require("internal").db._drop("male");
  examples.loadGraph("social");
  var url = "/_api/gharial/social/edge/relation";
  body = {
    type: "friend",
    _from: "female/alice",
    _to: "female/diana"
  };
  var response = logCurlRequest('POST', url, body);

  assert(response.code === 202);

  logJsonResponse(response);
  examples.dropGraph("social");
@END_EXAMPLE_ARANGOSH_RUN
@endDocuBlock
