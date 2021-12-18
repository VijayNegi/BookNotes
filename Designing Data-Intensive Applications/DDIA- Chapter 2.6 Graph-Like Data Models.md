---
alias: 
title: Chapter 2.6 Graph-Like Data Models 
creation date: 2021-12-18 19:30
type: note
category: []
status:
priority:
tags:
author:
rating: 
relates: 
area: 
project:
---
link:: 
tags:: 
relations:: [Designing Data-Intensive Applications](Designing%20Data-Intensive%20Applications.md)

[<- BACK TO BOOK ](Designing%20Data-Intensive%20Applications.md)
[<- Back to Chapter 2](DDIA-%20Chapter%202.%20Data%20Models%20and%20Query%20Languages.md)

# Chapter 2.6 Graph-Like Data Models

We saw earlier that many-to-many relationships are an important distinguishing feature between different data models.

A graph consists of two kinds of objects: _vertices_ (also known as _nodes_ or _entities_) and _edges_ (also known as _relationships_ or _arcs_).
Well-known algorithms can operate on these graphs.
graphs are not limited to such _homogeneous_ data: an equally powerful use of graphs is to provide a consistent way of storing completely different types of objects in a single datastore.

There are several different, but related, ways of structuring and querying data in graphs.
1. _property graph_ model (implemented by Neo4j, Titan, and InfiniteGraph)
2. _triple-store_ model (implemented by Datomic, AllegroGraph, and others)

We will look at three declarative query languages for graphs: 
1. Cypher, 
2. SPARQL, and 
3. Datalog.

## Property Graphs

In the property graph model, each vertex consists of:
- A unique identifier
- A set of outgoing edges
- A set of incoming edges
- A collection of properties (key-value pairs)

Each edge consists of:
- A unique identifier
- The vertex at which the edge starts (the _tail vertex_)
- The vertex at which the edge ends (the _head vertex_)
- A label to describe the kind of relationship between the two vertices
- A collection of properties (key-value pairs)

You can think of a graph store as consisting of two relational tables, one for vertices and one for edges.

## The Cypher Query Language

_Cypher_ is a declarative query language for property graphs, created for the Neo4j graph database.
e.g.: 
```json
CREATE
  (NAmerica:Location {name:'North America', type:'continent'}),
  (USA:Location      {name:'United States', type:'country'  }),
  (Idaho:Location    {name:'Idaho',         type:'state'    }),
  (Lucy:Person       {name:'Lucy' }),
  (Idaho) -[:WITHIN]->  (USA)  -[:WITHIN]-> (NAmerica),
  (Lucy)  -[:BORN_IN]-> (Idaho)
```

`(Idaho) -[:WITHIN]-> (USA)` creates an edge labeled `WITHIN`, with `Idaho` as the tail node and `USA` as the head node.

query example, find the names of all the people who emigrated from the United States to Europe
```json
MATCH
  (person) -[:BORN_IN]->  () -[:WITHIN*0..]-> (us:Location {name:'United States'}),
  (person) -[:LIVES_IN]-> () -[:WITHIN*0..]-> (eu:Location {name:'Europe'})
RETURN person.name
```

## Triple-Stores and SPARQL

The triple-store model is mostly equivalent to the property graph model, using different words to describe the same ideas.

In a triple-store, all information is stored in the form of very simple three-part statements: 
(_subject_, _predicate_, _object_). 

For example, in the triple (_Jim_, _likes_, _bananas_), _Jim_ is the subject, _likes_ is the predicate (verb), and _bananas_ is the object.

The subject of a triple is equivalent to a vertex in a graph. The object is one of two things:

1.  A value in a primitive datatype, such as a string or a number. In that case, the predicate and object of the triple are equivalent to the key and value of a property on the subject vertex. For example, (_lucy_, _age_, _33_) is like a vertex `lucy` with properties `{"age":33}`.
2.  Another vertex in the graph. In that case, the predicate is an edge in the graph, the subject is the tail vertex, and the object is the head vertex. For example, in (_lucy_, _marriedTo_, _alain_) the subject and object _lucy_ and _alain_ are both vertices, and the predicate _marriedTo_ is the label of the edge that connects them.

### Example
example written as triples in a format called _Turtle_, a subset of _Notation3_ (_N3_)

```
@prefix : <urn:example:>.
_:lucy     a       :Person.
_:lucy     :name   "Lucy".
_:lucy     :bornIn _:idaho.
_:idaho    a       :Location.
_:idaho    :name   "Idaho".
_:idaho    :type   "state".
_:idaho    :within _:usa.
_:usa      a       :Location.
_:usa      :name   "United States".
_:usa      :type   "country".
_:usa      :within _:namerica.
_:namerica a       :Location.
_:namerica :name   "North America".
_:namerica :type   "continent".
```
vertices of the graph are written as `_:_someName_`.
when predicate is a property, the object is a string literal, as in `_:usa :name "United States"`


### The semantic web

The semantic web is fundamentally a simple and reasonable idea: websites already publish information as text and pictures for humans to read, so why don’t they also publish information as machine-readable data for computers to read? The _Resource Description Framework_ (RDF)  was intended as a mechanism for different websites to publish data in a consistent format, allowing data from different websites to be automatically combined into a _web of data_—a kind of internet-wide “database of everything.”

### The RDF data model
The Turtle language is a human-readable format for RDF data. Sometimes RDF is also written in an XML format.
RDF has a few quirks due to the fact that it is designed for internet-wide data exchange. The subject, predicate, and object of a triple are often URIs.
The reasoning behind this design is that you should be able to combine your data with someone else’s data, and if they attach a different meaning to the words , you won’t get a conflict


### The SPARQL query language

SPARQL is a query language for triple-stores using the RDF data model. (It is an acronym for SPARQL Protocol and RDF Query Language, pronounced “sparkle.”)

The same query as before—finding people who have moved from the US to Europe—is even more concise in SPARQL than it is in Cypher
```
PREFIX : <urn:example:>

SELECT ?personName WHERE {
  ?person :name ?personName.
  ?person :bornIn  / :within* / :name "United States".
  ?person :livesIn / :within* / :name "Europe".
}
```

The structure is very similar. The following two expressions are equivalent (variables start with a question mark in SPARQL)
```
(person) -[:BORN_IN]-> () -[:WITHIN*0..]-> (location)   # Cypher

?person :bornIn / :within* ?locatio
```

### GRAPH DATABASES COMPARED TO THE NETWORK MODEL

They differ in several important ways:
- In CODASYL, a database had a schema that specified which record type could be nested within which other record type. In a graph database, there is no such restriction: any vertex can have an edge to any other vertex. This gives much greater flexibility for applications to adapt to changing requirements.
- In CODASYL, the only way to reach a particular record was to traverse one of the access paths to it. In a graph database, you can refer directly to any vertex by its unique ID, or you can use an index to find vertices with a particular value.
- In CODASYL, the children of a record were an ordered set, so the database had to maintain that ordering (which had consequences for the storage layout) and applications that inserted new records into the database had to worry about the positions of the new records in these sets. In a graph database, vertices and edges are not ordered (you can only sort the results when making a query).
- In CODASYL, all queries were imperative, difficult to write and easily broken by changes in the schema. In a graph database, you can write your traversal in imperative code if you want to, but most graph databases also support high-level, declarative query languages such as Cypher or SPARQL.


## The Foundation: Datalog

_Datalog_ is a much older language than SPARQL or Cypher, having been studied extensively by academics in the 1980s
Datalog’s data model is similar to the triple-store model, generalized a bit. Instead of writing a triple as (_subject_, _predicate_, _object_), we write it as _predicate_(_subject_, _object_).




