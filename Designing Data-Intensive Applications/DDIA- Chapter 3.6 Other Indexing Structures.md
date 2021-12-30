---
alias: 
title: Chapter 3.6 Other Indexing Structures 
creation date: 2021-12-29 18:02
type: note
category: []
status:
priority:
tags:
relates: 
area: 
project:
---
link:: 
tags:: 
relations:: [Designing Data-Intensive Applications](Designing%20Data-Intensive%20Applications.md) 

[<- BACK TO BOOK ](Designing%20Data-Intensive%20Applications.md)
[<- Back to Chapter 3](DDIA-%20Chapter%203.%20Storage%20and%20Retrieval.md)

# Chapter 3.6 Other Indexing Structures


- So far we have only discussed key-value indexes, which are like a _primary key_ index in the relational model. 
- A primary key uniquely identifies one row in a relational table, or one document in a document database, or one vertex in a graph database
- It is also very common to have **secondary indexes**. (often crucial for performing joins efficiently)
- A secondary index can easily be constructed from a key-value index. The main difference is that in a secondary index, the indexed values are not necessarily unique.
- Two approaches - 
    - a key match to list of matching rows identifiers.
    - making each entry unique by appending a row identifier to it.


### Storing values within the index
the value can be one of two things: 
1. actual row (document, vertex) in question, or 
2. a reference to the row stored elsewhere.

- In the latter case, the place where rows are stored is known as a heap file, and it stores data in no particular order.
- The heap file approach is common because it avoids duplicating data when multiple secondary indexes are present: each index just references a location in the heap file, and the actual data is kept in one place.
- When updating a value, either change it inplace if it is smaller the old value or move it some larger place and leave **forwarding pointer** at old location.
- In some situations, the extra hop from the index to the heap file is too much of a performance penalty for reads, so it can be desirable to store the indexed row directly within an index. This is known as a **clustered index**.
- A compromise between a **clustered index** (storing all row data within the index) and a **nonclustered index** (storing only references to the data within the index) is known as a **_covering index_ or _index with included columns_**, which stores _some_ of a table’s columns within the index.
- As with any kind of duplication of data, clustered and covering indexes can speed up reads, but they require additional storage and can add overhead on writes
- NOTE: there could be only one clustered index, also data is stored with sorted with clustered index.

### Multi-column indexes

- The most common type of multi-column index is called a **_concatenated index_**, which simply combines several fields into one key by appending one column to another (the index definition specifies in which order the fields are concatenated).
- **Multi-dimensional indexes** are a more general way of querying several columns at once, which is particularly important for geospatial data.
- More commonly, specialized spatial indexes such as **R-trees** are used.

### Full-text search and fuzzy indexes
- search for _similar_ keys, such as misspelled words.
- Lucene uses a SSTable-like structure for its term dictionary. This structure requires a small in-memory index that tells queries at which offset in the sorted file they need to look for a key. 
- In LevelDB, this in-memory index is a sparse collection of some of the keys, but in Lucene, the in-memory index is a finite state automaton over the characters in the keys, similar to a _trie_. This automaton can be transformed into a **_Levenshtein automaton_**, which supports efficient search for words within a given edit distance

### Keeping everything in memory
- Many datasets are simply not that big, so it’s quite feasible to keep them entirely in memory, potentially distributed across several machines. This has led to the development of **_in-memory databases_**.
- Some in-memory key-value stores, such as **Memcached**, are intended for caching use only, where it’s acceptable for data to be lost if a machine is restarted.
- But other in-memory databases aim for durability, 
    - which can be achieved with special hardware (such as battery-powered RAM), 
    - by writing a log of changes to disk, 
    - by writing periodic snapshots to disk, or 
    - by replicating the in-memory state to other machines.
- Writing to disk also has operational advantages: 
    - files on disk can easily be backed up, 
    - inspected, and analyzed by external utilities.
- Examples : VoltDB, MemSQL, and Oracle TimesTen are in-memory databases with a relational model.
- **Redis** and **Couchbase** provide weak durability by writing to disk asynchronously.


#### Comparison with disk-based DB
- Even a disk-based storage engine may never need to read from disk if you have enough memory, because the operating system caches recently used disk blocks in memory anyway. 
- Rather, they can be faster because they can **avoid the overheads of encoding in-memory data structures** in a form that can be written to disk.
- Besides performance, another interesting area for in-memory databases is **providing data models that are difficult to implement with disk-based indexes**. 
- For example, Redis offers a database-like interface to various data structures such as priority queues and sets. Because it keeps all data in memory, its implementation is comparatively simple.
- Supporting in-memory database larger then available physical RAM : The so-called **_anti-caching_** approach works by evicting the least recently used data from memory to disk when there is not enough memory, and loading it back into memory when it is accessed again in the future.


# Refs
- [Clustered and nonclustered indexes described](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-ver15)


























