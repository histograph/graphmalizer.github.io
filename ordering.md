---
layout: page
---

##  Building a graph and don't care.

We say a graph consists of **nodes** and **edges**,

node ![document](images/node.svg), edge ![document](images/edge.svg)

Edges have a **source** and **target** node, which needs to be there before you can add the edge.

#### Ordering matters

Typically, when constructing a graph you need first need to specify the nodes before you can specify the edges.

![add x, y then f](images/build-x-y-f.svg)

Changing the order, for instance, adding `y` node and then the `f` edge from
`x` to `y` will fail to produce the same graph.

![add y, f then x](images/build-y-f-x.svg)

#### Order independent

Lets introducing *anchors*, nodes that are considered "vacant", not really there.
Now we make graph construction ordering independent:

![join x with y and f](images/join-x-y-f.svg)

Changing the order has no effect anymore, as in, we still get the same graph in the end.

![add y, f then x](images/join-y-f-x.svg)

## Joining datasets

What does this give us? We can now think of a union of datasets.

> ![dataset](images/dataset.svg) `foo`
> ![dataset](images/dataset.svg) `bar`

> ![dataset](images/graph.svg)![dataset](images/graph2.svg)

Each dataset gives a graph and we join those into a bigger graph.

Suppose there is some overlapping identifier "*x*".

> ![split](images/two-sets.svg)

Then

> ![join](images/overlay-sets.svg) gives ![whee](images/joined-sets.svg)
