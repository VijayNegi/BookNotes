---
alias: 
title: Designing Data-Intensive Applications 
creation date: 2021-11-21 20:47
type: book
category: [system-design]
status: in-progress
priority: 4
tags:
author: Martin Kleppmann
rating: 3
relates: 
area: system-design
project:
---
link:: https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/
tags:: 
relations:: 

# Designing Data-Intensive Applications

The Big Ideas behind reliable, scalable and maintainable systems

## Preface

 - Businesses need to be agile, test hypotheses cheaply, and respond quickly to new market insights by keeping development cycles short and data models flexible.
 - CPU clock speeds are barely increasing, but multi-core processors are standard, and networks are getting faster. This means parallelism is only going to increase.
 - Many services are now expected to be highly available; extended downtime due to outages or maintenance is becoming increasingly unacceptable.
 - We call an application data-intensive if data is its primary challenge—the quantity of data, the complexity of data, or the speed at which it is changing—as opposed to compute-intensive, where CPU cycles are the bottleneck.
 - You need to develop a good intuition for what your systems are doing under the hood so that you can reason about their behavior, make good design decisions, and track down any problems that may arise.

## Book Outline

- Part 1. Fundamental ideas
    - [Chapter 1. Reliability, scalability, and maintainability](DDIA-%20Chapter%201.%20Reliability,%20scalability,%20and%20maintainability.md)
    - Chapter 2. Compare several different data models and query languages
    - Chapter 3. Storage engines: how databases arrange data on disk so that we can find it again efficiently
    - Chapter 4. Formats for data encoding (serialization) and evolution of schemas over time.
- Part 2. Distributed data
    - Chapter 5. Data Replication
    - Chapter 6. Partitioning/Sharding
    - Chapter 7. Transactions
    - Chapter 8. Problems with distributed systems
    - Chapter 9. What it means to achieve consistency and consensus in a distributed system
- Part 3. Derive datasets from other datasets. i.e. databases, caches, indexes.
    - Chapter 10. Derived data from batch processing
    - Chapter 11. Derived data from stream processing
    - Chapter 12. Bringing everything togather to build reliable, scalable, and maintainable applications in the future.

## Others
- Author weblink - https://martin.kleppmann.com/
- book references - https://github.com/ept/ddia-references