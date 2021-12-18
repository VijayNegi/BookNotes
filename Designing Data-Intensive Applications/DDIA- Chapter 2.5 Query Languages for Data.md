---
alias: 
title: Chapter 2.5 Query Languages for Data 
creation date: 2021-12-18 19:01
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

# Chapter 2.5 Query Languages for Data

Relational model, included a new way of querying data: SQL is a _declarative_ query language
IMS and CODASYL queried the database using _imperative_ code.

### imperative query language
Many commonly used programming languages are imperative.
An imperative language tells the computer to perform certain operations in a certain order.
You can imagine stepping through the code line by line, evaluating conditions, updating variables, and deciding whether to go around the loop one more time.


### declarative query language

In a declarative query language, like SQL or relational algebra, you just specify the pattern of the data you want—what conditions the results must meet, and how you want the data to be transformed (e.g., sorted, grouped, and aggregated)—but not how to achieve that goal.
It is up to the database system’s query optimizer to decide which indexes and which join methods to use, and in which order to execute various parts of the query.


### declarative vs imperative

A declarative query language is attractive because it is typically more concise and easier to work with than an imperative API.
But more importantly, it also hides implementation details of the database engine, which makes it possible for the database system to introduce performance improvements without requiring any changes to queries.

The SQL example doesn’t guarantee any particular ordering, and so it doesn’t mind if the order changes. But if the query is written as imperative code, the database can never be sure whether the code is relying on the ordering or not. The fact that SQL is more limited in functionality gives the database much more room for automatic optimizations.

Finally, declarative languages often lend themselves to parallel execution.

##  Declarative Queries on the Web

In a web browser, using declarative CSS styling is much better than manipulating styles imperatively in JavaScript. Similarly, in databases, declarative query languages like SQL turned out to be much better than imperative query APIs

Here the CSS selector `li.selected > p` declares the pattern of elements to which we want to apply the blue style: namely, all `<p>` elements whose direct parent is an `<li>` element with a CSS class of `selected`
```css
li.selected > p {
    background-color: blue;
}
```

In Imperative JavaScript approach, using the core Document Object Model (DOM) API, the result might look something like this:
```js
var liElements = document.getElementsByTagName("li");
for (var i = 0; i < liElements.length; i++) {
    if (liElements[i].className === "selected") {
        var children = liElements[i].childNodes;
        for (var j = 0; j < children.length; j++) {
            var child = children[j];
            if (child.nodeType === Node.ELEMENT_NODE && child.tagName === "P") {
                child.setAttribute("style", "background-color: blue");
            }
        }
    }
}
```


## MapReduce Querying
_MapReduce_ is a programming model for processing large amounts of data in bulk across many machines, popularized by Google.

MapReduce is neither a declarative query language nor a fully imperative query API, but somewhere in between: the **logic of the query is expressed with snippets of code**, which are called repeatedly by the processing framewor
It is based on the `map` (also known as `collect`) and `reduce` (also known as `fold` or `inject`) functions that exist in many functional programming languages.

### Example

Generate a report saying how many sharks you have sighted per month.

The same can be expressed with MongoDB’s MapReduce feature as follows:

```json
db.observations.mapReduce(
    function map() { 
        var year  = this.observationTimestamp.getFullYear();
        var month = this.observationTimestamp.getMonth() + 1;
        emit(year + "-" + month, this.numAnimals); 
    },
    function reduce(key, values) { 
        return Array.sum(values); 
    },
    {
        query: { family: "Sharks" }, 
        out: "monthlySharkReport" 
    }
);
```

- `query: { family: "Sharks" }` The filter to consider only shark species can be specified declaratively (this is a MongoDB-specific extension to MapReduce).
- The JavaScript function map is called once for every document that matches query, with this set to the document object.
- The map function emits a key (a string consisting of year and month, such as "2013-12" or "2014-1") and a value (the number of animals in that observation).
- The key-value pairs emitted by map are grouped by key. For all key-value pairs with the same key (i.e., the same month and year), the reduce function is called once.
- The reduce function adds up the number of animals from all observations in a particular month.
- The final output is written to the collection monthlySharkReport.

The `map` and `reduce` functions are somewhat restricted in what they are allowed to do. 
- They must be _pure_ functions, which means they only use the data that is passed to them as input, they cannot perform additional database queries
- they must not have any side effects.

These restrictions allow the database to run the functions anywhere, in any order, and rerun them on failure.


MongoDB 2.2 added support for a declarative query language called the _aggregation pipeline_
In this language, the same shark-counting query looks like this:

```json
db.observations.aggregate([
    { $match: { family: "Sharks" } },
    { $group: {
        _id: {
            year:  { $year:  "$observationTimestamp" },
            month: { $month: "$observationTimestamp" }
        },
        totalAnimals: { $sum: "$numAnimals" }
    } }
]);
```