---
alias: Summary of 35 years of data model proposals
title: What Goes Around Comes Around 
creation date: 2021-12-17 09:40
type: note
category: [database, Article]
status:
priority:
tags:
author:
rating: 
relates: 
area: 
project:
---
link:: [What Goes Around Comes Around](http://mitpress2.mit.edu/books/chapters/0262693143chapm1.pdf)
tags:: 
relations:: [DDIA- Chapter 2. Data Models and Query Languages](../DDIA-%20Chapter%202.%20Data%20Models%20and%20Query%20Languages.md)

# What Goes Around Comes Around (2005)
summary of 35 years of data model proposals, grouped into 9 different eras.

## Introduction

We present data model proposals in nine historical epochs:
- Hierarchical (IMS): late 1960’s and 1970’s
- Network (CODASYL): 1970’s
- Relational: 1970’s and early 1980’s
- Entity-Relationship: 1970’s
- Extended Relational: 1980’s
- Semantic: late 1970’s and 1980’s
- Object-oriented: late 1980’s and early 1990’s
- Object-relational: late 1980’s and early 1990’s
- Semi-structured (XML): late 1990’s to the present

In each case, we discuss the data model and associated query language


## IMS Era

**requires  tree-structured data**

- IMS was released around 1968, and initially had a **hierarchical data model**. 
- **record type** - a collection of named fields with their associated data types. 
- **instance of a record type** follow definition of the record type. 
- **Key** : some subset of the named fields uniquely specify a record instance.
- the record types arranged in a tree (single parent other then root)
- An IMS data base is a collection(in form of tree) of instances of record types.

Two common undesirable properties:
- Information is repeated.
- Existence depends on parents.

IMS has chosen to limit the amount of [Physical data independence](Data%20Independence%20in%20DBMS.md#Physical%20data%20independence) that is possible.

IMS supports a certain level of [Logical data independence](Data%20Independence%20in%20DBMS.md#Logical%20data%20independence),
because DL/1 is actually defined on a logical data base, not on the actual physical data
base that is stored. Hence, a DL/1 program can be written initially by defining the logical
data base to be exactly same as the physical data base. Later, record types can be added
to the physical data base, and the logical data base redefined to exclude them.

> It is an excellent idea to have the programmer interact with a logical abstraction of the
> data, because this allows the physical organization to change, without compromising the
> runability of database queries.


**Lesson 1:** Physical and logical data independence are highly desirable
**Lesson 2:** Tree structured data models are very restrictive
**Lesson 3:** It is a challenge to provide sophisticated logical reorganizations of tree structured data
**Lesson 4:** A **record-at-a-time** user interface forces the programmer to do manual query optimization, and this is often hard.


## CODASYL Era

CODASYL (Committee on Data Systems Languages)
CODASYL was an ad-hoc committee that championed a network data model along with a record-at-a-time data manipulation language.

This model organized a collection of record types, each with keys, into a network, rather than a tree. 
Hence, a given record instance could have multiple parents, rather than a single one, as in IMS.

A CODASYL network is a collection of named record types and named set types that form a connected graph. 
Moreover, there must be at least one entry point (a record type that is not a child in any set). 
A CODASYL data base is a collection of record instances and set instances that obey this network description.
The CODASYL data manipulation language is a record-at-a-time language whereby one enters the data base at an entry point and then navigates to desired data by following sets.

The CODASYL proposal provided essentially no physical data independence.

**Lesson 5:** Networks are more flexible than hierarchies but more complex
**Lesson 6:** Loading and recovering networks is more complex than hierarchies

## Relational Era

Ted Codd proposed his relational model in 1970 focused on providing better data independence.

His proposal was threefold:
Store the data in a simple data structure (tables)
Access it through a high level set-at-a-time DML
No need for a physical storage proposal


Set-at-a-time languages offer substantial programmer productivity improvements, relative to record-at-a-time languages.

**Lesson 7:** Set-a-time languages are good, regardless of the data model, since they offer much improved physical data independence.
**Lesson 8:** Logical data independence is easier with a simple data model than with a complex one.
**Lesson 9:** Technical debates are usually settled by the elephants of the marketplace, and often for reasons that have little to do with the technology.
**Lesson 10:** Query optimizers can beat all but the best record-at-a-time DBMS application programmers.

## The Entity-Relationship Era


data base be thought of a collection of instances of entities.
Loosely speaking these are objects that have an existence, independent of any other entities in the data base.
entities have attributes, which are the data elements that characterize the entity.
there could be relationships between entities.
Relationships could be 1-to-1, 1-to-n, n-to-1 or m-to-n, depending on how the entities participate in the relationship

There is one area where the E-R model has been wildly successful, namely in data base (schema) design.

**Lesson 11:** Functional dependencies are too difficult for mere mortals to understand. Another reason for KISS (Keep it simple stupid).

## R++ Era

new constructs to the relational model were proposed
- set-valued attributes.
- aggregation (tuple-reference as a data type).
- generalization

**Lesson 12:** Unless there is a big performance or functionality advantage, new constructs will go nowhere.

## The Semantic Data Model Era

They suggested that the relational data model is “semantically impoverished”, i.e. it is incapable of easily expressing a class of data of interest. 
Hence, there is a need for a “post relational” data model.
Post relational data models were typically called semantic data models.

SDM focuses on the notion of classes, which are a collection of records obeying the same schema. 
SDM exploited the concepts of aggregation and generalization and included a notion of sets.

## OO Era

Object-oriented DBMSs (OODB). 
Basically, this community pointed to an “impedance mismatch” between relational data bases and languages like C++.


In practice, relational data bases had their own naming systems, their own data type systems, and their own conventions for returning data as a result of a query. 
Whatever programming language was used alongside a relational data base also had its own version of all of these facilities. 
Hence, to bind an application to the data base required a conversion from “programming language speak” to “data base speak” and back. 
This was like “gluing an apple onto a pancake”, and was the reason for the so-called **impedance mismatch**.

one would like a **persistent programming language**, i.e. one where the variables in the language could represent disk-based data as
well as main memory data and where data base search criteria were also language constructs.


In our opinion, there are a number of reasons for this market failure.
1) absence of leverage. The OODB vendors presented the customer with the opportunity to avoid writing a load program and an unload program. This is not a major service, and customers were not willing to pay big money for this feature.
2) No standards. All of the OODB vendor offerings were incompatible.
3) Relink the world. In anything changed, for example a C++ method that operated on persistent data, then all programs which used this method had to be relinked.This was a noticeable management problem.
4) No programming language Esperanto. If your enterprise had a single application not written in C++ that needed to access persistent data, then you could not use one of the OODB products.

Of course, the OODB products were not designed to work on business data processing applications. 
Not only did they lack strong transaction and query systems but also they ran in the same address space as the application.
This meant that the application could freely manipulate all disk-based data, and no data protection was possible. 
Protection and authorization is important in the business data processing market.


**Lesson 13:** Packages will not sell to users unless they are in “major pain”
**Lesson 14:** Persistent languages will go nowhere without the support of the programming language community


## The Object-Relational Era


The Object-Relational (OR) era was motivated by a very simple problem: **two dimensional search** . eg search bounded geographic positions

the B-trees in INGRES are a one dimensional access method. One-dimensional access methods do not do two dimensional searches efficiently, 
so there is no way in a relational system for this query to run fast.

In summary, simple GIS queries are difficult to express in SQL, and they execute on standard B-trees with unreasonably bad performance.



The following observation motivates the OR proposal. 
- Early relational systems supported integers, floats, and character strings, along with the obvious operators, primarily because these were the data types of IMS, which was the early competition.
- IMS chose these data types because that was what the business data processing market wanted, and that was their market focus. 
- Relational systems also chose B-trees because these facilitate the searches that are common in business data processing. 
- Later relational systems expanded the collection of business data processing data types to include date, time and money. More recently, packed decimal and blobs have been added.

==Hence, to address any given market, one needs data types and access methods appropriate to the market.==
Since there may be many other markets one would want to address, it is inappropriate to “hard wire” a specific collection of data types and indexing strategies. 
Rather a sophisticated user should be able to add his own; i.e. to customize a DBMS to his particular needs. 
Such customization is also helpful in business data processing, since one or more new data types appears to be needed every decade.

**As a result, the OR proposal added**

- user-defined data types,
- user-defined operators,
- user-defined functions, and
- user-defined access methods

to a SQL engine. The major OR research prototype was Postgres .

 
To address the GIS market one needs a **multi-dimensional indexing system, such as Quad trees or R-trees** .


The Postgres UDTs and UDFs generalized this notion to allow code to be written in a conventional programming language and to be called in the middle of processing conventional SQL queries.
Postgres implemented a sophisticated mechanism for UDTs, UDFs and user-defined access methods. 
In addition, Postgres also implemented less sophisticated notions of inheritance, and type constructors for pointers (references), sets, and arrays. This latter set of features allowed Postgres to become “object-oriented” at the height of the OO craze.

**Lesson 14:** The major benefits of OR is two-fold: putting code in the data base (and thereby blurring the distinction between code and data) and user-defined access methods.
**Lesson 15:** Widespread adoption of new technology requires either standards and/or an elephant pushing hard.



## Semi Structured Data


There are two basic points that this class of work exemplifies.
1) schema last
2) complex network-oriented data model

### Schema Last

The first point is that a schema is not required in advance. 
In a “schema first” system the schema is specified, and instances of data records that conform to this schema can be subsequently loaded. 
Hence, the data base is always consistent with the pre-existing schema, because the DBMS rejects any records that are not consistent with the schema.
All previous data models required a DBA to specify the schema in advance.



In this class of proposals the schema does not need to be specified in advance. 
It can be specified last, or even not at all. In a “schema last” system, data instances must be **self describing**, because there is not necessarily a schema to give meaning to incoming records. Without a self-describing format, a record is merely “a bucket of bits”.
To make a record self-describing, one must tag each attribute with metadata that defines the meaning of the attribute. 

      

This is an example of semantic heterogeneity, where information on a common object (in this case a person) does not conform to a common representation. 
Semantic heterogeneity makes query processing a big challenge, because there is no structure on which to base indexing decisions and query execution strategies.


the following scheme that classifies applications into four buckets.

| app                                                     | schema       |
| ------------------------------------------------------- | ------------ |
| Ones with rigidly structured data                       | schema first |
| Ones with rigidly structured data with some text fields | schema first |
| Ones with semi-structured data                          | schema last  |
| Ones with text                                          | schema last  |


Typical warehouse projects are over budget, because schema homogenization is so hard. 
Any schema-last application will have to confront semantic heterogeneity on a record-by record basis, where it will be even more costly to solve. 
This is a good reason to avoid “schema last” if at all possible.


## XML Data Model

the data model presented in XMLSchema has the following characteristics:

1) XML records can be hierarchical, as in IMS
2) XML records can have “links” (references to) other records, as in CODASYL, Gem and SDM
3) XML records can have set-based attributes, as in SDM
4) XML records can inherit from other records in several ways, as in SDM

Obviously, XMLSchema is far and away the most complex data model ever proposed.
It is clearly at the other extreme from the relational model on the “keep it simple stupid” (KISS) scale. 
It is hard to imaging something this complex being used as a model for structured data.

**Lesson 16:** Schema-last is a probably a niche market
**Lesson 17:** XQuery is pretty much OR SQL with a different syntax
**Lesson 18:** XML will not solve the semantic heterogeneity either inside or outside the enterprise.

















