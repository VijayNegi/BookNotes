---
alias: 
title: Data Independence in DBMS 
creation date: 2021-12-17 22:19
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
relations:: 

# Data Independence in DBMS

> Data independence refers characteristic of being able to modify the schema at one level of the database system without altering the schema at the next higher level.

There are 3 levels in the schema architecture of DBMS(lowest to highest level): 
1. physical level
2. logical level
3. view level

![dbms-data-independence](../Resources/dbms-data-independence.png)

## Physical data independence

With Physical independence, you can easily change the physical storage structures or devices with an effect on the conceptual schema. 
Any change done would be absorbed by the mapping between the conceptual and internal levels.

The ability of a data base application to continue to run, regardless of what tuning is
performed at the physical level will be called physical data independence. 

Physical data independence is important because a DBMS application is not typically written all at
once. As new programs are added to an application, the tuning demands may change,
and better DBMS performance could be achieved by changing the storage organization.

### Examples of changes under Physical Data Independence

Due to Physical independence, any of the below change will not affect the conceptual layer.
- Using a new storage device like Hard Drive or Magnetic Tapes
- Modifying the file organization technique in the Database
- Switching to different data structures.
- Changing the access method.
- Modifying indexes.
- Changes to compression techniques or hashing algorithms.


## Logical data independence

Logical Data Independence is the ability to change the conceptual scheme without changing
1.  External views
2.  External API or programs

Any change made will be absorbed by the mapping between external and conceptual levels.
This also means previously written queries should work without any changes.

In addition, the logical requirements of an application may change over time. New
record types may be added, because of new business requirements or because of new
government requirements. It may also be desirable to move certain data elements from
one record type to another.

### Examples of changes under Logical Data Independence

Due to Logical independence, any of the below change will not affect the external layer.

1.  Add/Modify/Delete a new attribute, entity or relationship is possible without a rewrite of existing application programs
2.  Merging two records into one
3.  Breaking an existing record into two or more records