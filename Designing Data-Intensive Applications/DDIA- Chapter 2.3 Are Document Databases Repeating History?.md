---
alias: 
title: DDIA- Chapter 2.3 Are Document Databases Repeating History? 
creation date: 2021-12-18 18:23
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

# DDIA- Chapter 2.3 Are Document Databases Repeating History?

While many-to-many relationships and joins are routinely used in relational databases, document databases and NoSQL reopened the debate on how best to represent such relationships in a database.

Various solutions were proposed to solve the limitations of the hierarchical model(used by IMS). The two most prominent were the _relational model_ (which became SQL, and took over the world) and the _network model_ (which initially had a large following but eventually faded into obscurity)

### The network model

The network model was standardized by a committee called the Conference on Data Systems Languages (CODASYL) and implemented by several different database vendors; it is also known as the _CODASYL model_.
In the tree structure of the hierarchical model, every record has exactly one parent; in the network model, a record could have multiple parents.
The links between records in the network model were not foreign keys, but more like pointers in a programming language (while still being stored on disk).
The only way of accessing a record was to follow a path from a root record along these chains of links. This was called an _access path_.

But in a world of many-to-many relationships, several different paths can lead to the same record, and a programmer working with the network model had to keep track of these different access paths in their head.
Even CODASYL committee members admitted that this was like navigating around an _n_-dimensional data space

This made the code for querying and updating the database complicated and inflexible.

### The relational model

What the relational model did, by contrast, was to lay out all the data in the open: a relation (table) is simply a collection of tuples (rows), and that’s it.
In a relational database, the query optimizer automatically decides which parts of the query to execute in which order, and which indexes to use.
The relational model thus made it much easier to add new features to applications.

Query optimizers for relational databases are complicated beasts, and they have consumed many years of research and development effort. 
But a key insight of the relational model was this: you only need to build a query optimizer once, and then all applications that use the database can benefit from it.

### Comparison to document databases

Document databases reverted back to the hierarchical model in one aspect: storing nested records (one-to-many relationships, like `positions`, `education`, and `contact_info` in  within their parent record rather than in a separate table.
However, when it comes to representing many-to-one and many-to-many relationships, relational and document databases are not fundamentally different: in both cases, the related item is referenced by a unique identifier, which is called a _foreign key_ in the relational model and a _document reference_ in the document model


