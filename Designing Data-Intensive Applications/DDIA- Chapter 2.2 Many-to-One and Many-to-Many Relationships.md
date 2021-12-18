---
alias: 
title: Chapter 2.2 Many-to-One and Many-to-Many Relationships 
creation date: 2021-12-18 17:55
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


# Chapter 2.2 Many-to-One and Many-to-Many Relationships


If the user interface has free-text fields for entering the region and the industry, it makes sense to store them as plain-text strings. But there are advantages to having standardized lists of geographic regions and industries, and letting users choose from a drop-down list or autocompleter:

- Consistent style and spelling across profiles
- Avoiding ambiguity (e.g., if there are several cities with the same name)
- Ease of updating—the name is stored in only one place, so it is easy to update across the board if it ever needs to be changed (e.g., change of a city name due to political events)
- Localization support—when the site is translated into other languages, the standardized lists can be localized, so the region and industry can be displayed in the viewer’s language
- Better search—e.g., a search for philanthropists in the state of Washington can match this profile, because the list of regions can encode the fact that Seattle is in Washington (which is not apparent from the string `"Greater Seattle Area"`)



> The advantage of using an ID is that because it has no meaning to humans, it never needs to change: the ID can remain the same, even if the information it identifies changes. 
> Anything that is meaningful to humans may need to change sometime in the future—and if that information is duplicated, all the redundant copies need to be updated. 
> That incurs write overheads, and risks inconsistencies (where some copies of the information are updated but others aren’t). 
> Removing such duplication is the key idea behind _normalization_ in databases.

==normalizing this data requires _many-to-one_ relationships (many people live in one particular region, many people work in one particular industry), which don’t fit nicely into the document model.==

If the database itself does not support joins, you have to emulate a join in application code by making multiple queries to the database.

Moreover, even if the initial version of an application fits well in a join-free document model, data has a tendency of becoming more interconnected as features are added to applications.

![ddia_0204_many-to-many](Resources/ddia_0204_many-to-many.png)
Extending résumés with many-to-many relationships.

