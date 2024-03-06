---
layout: post
title:  "Graphs are hard"
date:   2024-03-06
---

Why are graphs hard? In [The hunt for the missing datatype](https://www.hillelwayne.com/post/graph-types/) Hillel Wayne describes four reasons:

1. There are too many types of graphs.
2. There are too many graph algorithms.
3. Even if you focus on one particular type of graph, there are many possible different graph representations.
4. The graph representation you choose has a large effect on performance.

This all sounds too familiar too me.

Performance is hard.
My team went through three iterations of the graph storage model until we arrived at a data model that performed well for both read and write.
The model is specific to the business problem we work on. Yet, one data model is not enough.
To run community detection on our graph we create another representation of the graph by folding (remove intermediate nodes) the graph.
For this there are two reasons:
1. create communities with just the vertex types we're interested in
2. greatly reduce the size of the graph to be able to run the [Leiden community detection algorithm](https://en.wikipedia.org/wiki/Leiden_algorithm)

The type of storage influences the data model too.
Initially the graph was stored in Neo4J as an undirected graph (each edge can be traversed in both directions).
Later we changed the storage to Postgres.
In this case it turned out to be easier to consider the graph to be directed, and mimic the undirectional behaviour by storing the edge twice.