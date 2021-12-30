---
alias: 
title: Chapter 3.1 World’s simplest database 
creation date: 2021-12-20 23:17
type: note
category: []
status:
priority:
tags:
author:
area: 
project:
---
link:: 
tags:: 
relations:: [Designing Data-Intensive Applications](Designing%20Data-Intensive%20Applications.md)

[<- BACK TO BOOK ](Designing%20Data-Intensive%20Applications.md)
[<- Back to Chapter 3](DDIA-%20Chapter%203.%20Storage%20and%20Retrieval.md)


# Chapter 3.1 World’s simplest database

Consider the world’s simplest database, implemented as two Bash functions:

```bash
#!/bin/bash

db_set () {
    echo "$1,$2" >> database
}

db_get () {
    grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}

```

These two functions implement a key-value store.

You can call `db_set key value`, which will store `key` and `value` in the database. The key and value can be (almost) anything you like—for example, the value could be a JSON document. 
You can then call `db_get key`, which looks up the most recent value associated with that particular key and returns it.

And it works:

```bash
$ db_set 123456 '{"name":"London","attractions":["Big Ben","London Eye"]}'

$ db_set 42 '{"name":"San Francisco","attractions":["Golden Gate Bridge"]}'

$ db_get 42
{"name":"San Francisco","attractions":["Golden Gate Bridge"]}
```

The underlying storage format is very simple: a text file where each line contains a key-value pair, separated by a comma (roughly like a CSV file, ignoring escaping issues).
Every call to `db_set` appends to the end of the file, so if you update a key several times, the old versions of the value are not overwritten—you need to look at the last occurrence of a key in a file to find the latest value (hence the `tail -n 1` in `db_get`):

 Real databases have more issues to deal with 
 - concurrency control
 - reclaiming disk space so that the log doesn’t grow forever
 - handling errors 
 - partially written records

> NOTE
> The word _log_ is often used to refer to application logs, where an application outputs text that describes what’s happening. In this book, _log_ is used in the more general sense: an append-only sequence of records. It doesn’t have to be human-readable; it might be binary and intended only for other programs to read.

### Write
Our `db_set` function actually has pretty good performance for something that is so simple, because appending to a file is generally very efficient.

### Read

On the other hand, our `db_get` function has terrible performance if you have a large number of records in your database as `db_get` has to scan the entire database file from beginning to end.
In algorithmic terms, the cost of a lookup is _O_(_n_): if you double the number of records _n_ in your database, a lookup takes twice as long. That’s not good.

### Index

**In order to efficiently find the value for a particular key in the database, we need a different data structure: an _index_.**
The general idea behind them is to keep some additional metadata on the side, which acts as a signpost and helps you to locate the data you want.

An index is an _additional_ structure that is derived from the primary data. 

Many databases allow you to add and remove indexes, and this doesn’t affect the contents of the database; it only affects the performance of queries.
Maintaining additional structures incurs overhead, especially on writes.

### Choose indexes manually
This is an important trade-off in storage systems: well-chosen indexes speed up read queries, but every index slows down writes.



