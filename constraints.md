---
layout: page
---

We use [cypher](http://neo4j.com/docs/2.2.2/cypher-refcard/) notation here and there.

# Schema or "Category".

We said we "maintain user specified constraints". Which constraints?

Lets take the following viewpoint. The nodes in our graph model *things*

> A **thing X** is a node ![document](images/node.svg) `(:X)`.

The edges describe relations between things.

> A **relation R** is an edge ![document](images/edge.svg) `() <-[:R]- ()` between *things*.

Now, specify a **category** of *things* and *relations* between them. What do we constrain?

> Ensure the graph only consists of things and relations as specified in the category.

We might further restrict the document to some JSON-schema.

## Example

Remember that a dataset provides documents and each document has an **id** and **type**, lets denote this `id:TYPE`.

![document](images/dataset.svg)
![document](images/document.svg)`1:A`
![document](images/document.svg)`2:A`
![document](images/document.svg)`3:P`
![document](images/document.svg)`4:P`

Now to each type we associate a *thing* or *relation*, let's say

>`A` ~ thing of type A
>
>`P` ~ relation of type P : A × A

So documents of type `A` are considered to be things of type `A` and become nodes of type `(:A)`.

Documents of type `P` are considered to be relations of type `P` and become edges with as source a node of type `A` and as target a node of type `A`.

Thus we turn our dataset into a graph like this:

> ![document](images/node.svg) `(:A {id: 1})`

> ![document](images/node.svg) `(:A {id: 2})`

> ![document](images/edge.svg) `(:A)-[:P {id: 3}]->(:A)`

> ![document](images/edge.svg) `(:A)-[:P {id: 4}]->(:A)`

But where to point the edges? It is clear that when the type is a relation
(so document 3 and 4) we must also specify **source** and **target** identifiers.

Also note, edges must also have an *id*, but if you don't specify one we derive it
from the `type`, `source` and `target` identifiers.

This implies then that there can exist only one edge of each relation
type between two nodes. (since the second edge would conclude the same *id*).


According to our *category*, the source and target of a relation of type `P` must be nodes of type `A`.

This is the invariant that we maintain.

### Another example.

Our category:

	A : thing
	B : thing
	C : thing
	in: [`A → B`,`B → C`] : [thing × thing]
	eq: [`C → C`,`B → B`,`A → A`] : [thing × thing]

Possible graph

	(:A {id: 1}) -[:in]-> (:B {id: 2})
	(:B {id: 2}) -[:eq]-> (:B {id: 3})
	(:B {id: 3}) -[:in]-> (:C {id: 4})

### This only works when nodes are added first!

Good observation.

We could add a relation between identifier `1` and `2` which might not yet exist as things.

How do we know the types corresponding to things `1` and `2`?
In general, we don't.

Possible solutions:

- Adding a relation between non-existing things is always allowed.
	When the identifiers become occupied and it turns out the domain or co-domain
	of the relation doesn't match the types, we remove the edge (or mark invalid).

- When specifying a `thing`, we might constrain some identifier subspace. That way,
	when we encounter an identifier, we might know it's type and check validity
	of the relation before adding the nodes.
	
- Instead of removing invalid edges, when creating *anchor*-nodes record their type.
	Then, when adding, restrict the type and disallow wrongly typed nodes to be added.
	But: order-dependent?

Neither is implemented, nor is this typing.