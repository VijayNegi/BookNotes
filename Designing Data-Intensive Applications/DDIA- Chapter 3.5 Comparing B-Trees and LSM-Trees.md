---
alias: 
title: Chapter 3.5 Comparing B-Trees and LSM-Trees 
creation date: 2021-12-28 10:37
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


# Chapter 3.5 Comparing B-Trees and LSM-Trees

- As a rule of thumb, LSM-trees are typically faster for writes, whereas B-trees are thought to be faster for reads.
- Reads are typically slower on LSM-trees because they have to check several different data structures and SSTables at different stages of compaction.


### Advantages of LSM-trees
 **Writes**
- A B-tree index must write every piece of data at least twice: once to the write-ahead log, and once to the tree page itself (and perhaps again as pages are split).
- There is also overhead from having to write an entire page at a time, even if only a few bytes in that page changed. 
- Some storage engines even overwrite the same page twice in order to avoid ending up with a partially updated page in the event of a power failure.

- Log-structured indexes also rewrite data multiple times due to repeated compaction and merging of SSTables. 

> This effect—one write to the database resulting in multiple writes to the disk over the course of the database’s lifetime—is known as **write amplification**.

- LSM-trees are typically able to sustain higher write throughput than B-trees, partly because they sometimes have lower write amplification (although this depends on the storage engine configuration and workload), and 
- partly because they sequentially write compact SSTable files rather than having to overwrite several pages in the tree.

**Space Utilization**
- LSM-trees can be compressed better, and thus often produce smaller files on disk than B-trees.
- B-tree storage engines leave some disk space unused due to fragmentation.


**So Preferred properties are** 
- lower write amplification.
- reduced fragmentation. 
- representing data more compactly allows more read and write requests within the available I/O bandwidth.

NOTE : On many SSDs, the firmware internally uses a log-structured algorithm to turn random writes into sequential writes on the underlying storage chips, so the impact of the storage engine’s write pattern is less pronounced.


### Downsides of LSM-trees

- A downside of log-structured storage is that the compaction process can sometimes interfere with the performance of ongoing reads and writes.
- Another issue with compaction arises at high write throughput: the disk’s finite write bandwidth needs to be shared between the initial write (logging and flushing a memtable to disk) and the compaction threads running in the background.
- it can happen that compaction cannot keep up with the rate of incoming writes. In this case, the number of unmerged segments on disk keeps growing until you run out of disk space, and reads also slow down because they need to check more segment files.
- An advantage of B-trees is that each key exists in exactly one place in the index, whereas a log-structured storage engine may have multiple copies of the same key in different segments.This aspect makes B-trees attractive in databases that want to offer strong transactional semantics: in many relational databases.




