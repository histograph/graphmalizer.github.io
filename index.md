---
layout: page
---

# Graphmalizer

##### Turns datasets into a graph, deal with it.

Given some datasets.

![document](images/dataset.svg)![document](images/dataset.svg)![document](images/dataset.svg)

*Graphmalizer is thinking.*
Then:

![document](images/joined-sets.svg)

A graph, same datapoints → same graph.

- [Order independent graph assembly](ordering.html)
- [Maintain graph according to some category](constraints.html)

## Parts.

We allow you to manage **datasets**. They are newline-delimited JSON files or CSV files.
Each *row* or *datapoint* is considered to be a **document**.
Every document has an **identifier** and a certain **type**.

![document](images/dataset.svg)
![document](images/document.svg)`1:A`
![document](images/document.svg)`2:A`
![document](images/document.svg)`3:P`
![document](images/document.svg)`4:P`
![document](images/document.svg)`5:A`

You specifiy which types exist. Types can refer to **things** or **relations** between things.
To each thing we associate a **node** and to each relation a **edge**.

For instance, we can say

> type `A` ~ things of some distinguishing sort (called A) ~ ![thing](images/node.svg)
>
> type `P` ~ relation of sort P, which runs between A × A ~ ![relation](images/edge.svg)

Documents of a type corresponding to a relation (here `3:P`,`4:P`) must specify 
a **source** and **target** identifier (referring to nodes of type `A`).

**Note**: If you leave out the *id*, we derive it from the type, source and target (details [here](encoding.html)).

For example

![document](images/dataset.svg)
![document](images/document.svg)`1:A`
![document](images/document.svg)`2:A`
![document](images/document.svg)`P:2→5`
![document](images/document.svg)`P:1→2`
![document](images/document.svg)`5:A`

Will become

	(1)---→(2)---→(5)

Or more precisely, using the cypher-pattern language.

	(:A {id: 1}) -[:P]-> (:A {id: 2}) -[:P]-> (:A {id: 5})
