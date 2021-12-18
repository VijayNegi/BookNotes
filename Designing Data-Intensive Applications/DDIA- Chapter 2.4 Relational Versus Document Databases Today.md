---
alias: 
title: Chapter 2.4 Relational Versus Document Databases Today 
creation date: 2021-12-18 18:36
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

# Chapter 2.4 Relational Versus Document Databases Today


The main arguments in favor of the document data model are 
- schema flexibility, 
- better performance due to locality, and 
- that for some applications it is closer to the data structures used by the application. 

The relational model counters by providing 
- better support for joins, and 
- many-to-one and 
- many-to-many relationships.

### Which data model leads to simpler application code?

The relational technique of _shredding_—splitting a document-like structure into multiple tables can lead to cumbersome schemas and unnecessarily complicated application code.

The document model has limitations: for example, you cannot refer directly to a nested item within a document, but instead you need to say something like “the second item in the list of positions for user 251” (much like an access path in the hierarchical model).


However, if your application does use many-to-many relationships, the document model becomes less appealing. 
- It’s possible to reduce the need for joins by denormalizing, but then the application code needs to do additional work to keep the denormalized data consistent.
- Joins can be emulated in application code by making multiple requests to the database, but that also moves complexity into the application and is usually slower than a join performed by specialized code inside the database.

It’s not possible to say in general which data model leads to simpler application code; it depends on the kinds of relationships that exist between data items.

### Schema flexibility in the document model

Document databases are sometimes called _schemaless_, but that’s misleading, as the code that reads the data usually assumes some kind of structure—i.e., there is an implicit schema, but it is not enforced by the database

A more accurate term is _schema-on-read_ (the structure of the data is implicit, and only interpreted when the data is read), in contrast with _schema-on-write_ (the traditional approach of relational databases, where the schema is explicit and the database ensures all written data conforms to it)
Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-time) type checking.

The schema-on-read approach is advantageous if the items in the collection don’t all have the same structure for some reason (i.e., the data is heterogeneous)—for example, because:
- There are many different types of objects, and it is not practicable to put each type of object in its own table.
- The structure of the data is determined by external systems over which you have no control and which may change at any time.

### Data locality for queries

A document is usually stored as a single continuous string, encoded as JSON, XML, or a binary variant thereof (such as MongoDB’s BSON). There is a performance advantage to this _storage locality_

The locality advantage only applies if you need large parts of the document at the same time. 
The database typically needs to load the entire document, even if you access only a small portion of it, which can be wasteful on large documents.
On updates to a document, the entire document usually needs to be rewritten—only modifications that don’t change the encoded size of a document can easily be performed in place.

Data locality in other data models
- Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table.
- Oracle allows the same, using a feature called _multi-table index cluster tables_
- The _column-family_ concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality

### Convergence of document and relational databases

Most relational database systems have supported XML/JSON. This includes functions to make local modifications and the ability to index and query inside XML documents, which allows applications to use data models very similar to what they would do when using a document database.

On the document database side, RethinkDB supports relational-like joins in its query language, and some MongoDB drivers automatically resolve document references (effectively performing a client-side join, although this is likely to be slower than a join performed in the database since it requires additional network round-trips and is less optimized).




















