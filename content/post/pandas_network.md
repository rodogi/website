---
title: "Using pandas for network analysis"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-05-15
description: "How to pair pandas DataFrames with networkx for cleaner network analysis."
---

## The context

[Networkx](https://networkx.github.io/) is the most popular Python library for network analysis. I used it extensively during my PhD for building and comparing protein networks, then set it aside during a stint in industry where pandas dominated my workflow.

## The problem

Coming back to academia to work on gene regulatory networks, I found that networkx is excellent for structural (topological) analysis, but handling node and edge attributes can get verbose.

For example, finding the node with the maximum degree in a 20,000-node network:

```python
degree_sequence = sorted([d for n, d in G.degree()], reverse=True)
dmax = max(degree_sequence)
```

Or to also retrieve the node name:

```python
nmax, dmax = sorted(G.degree, key=lambda x: x[1], reverse=True)[0]
```

Both approaches feel heavier than they should be for such a basic query.

## Pandas to the rescue

Networkx exposes network attributes through dict-like view objects: `DegreeView`, `NodeView`, `EdgeView`, `NodeDataView`, and `EdgeDataView`. These convert directly into pandas DataFrames:

```python
import pandas as pd

# Edge list
df_edges = pd.DataFrame(G.edges(), columns=["node_1", "node_2"])

# Edge list with attributes
df_edges_data = pd.DataFrame(
    G.edges(data="weight"), columns=["node_1", "node_2", "weight"]
)

# Degree
df_degree = pd.DataFrame(G.degree(), columns=["node", "degree"])

# Weighted degree
df_weighted_degree = pd.DataFrame(
    G.degree(weight="weight"), columns=["node", "weighted_degree"]
)
```

From here, finding the max-degree node is just `df_degree.sort_values("degree").tail(1)`—clean, readable, and idiomatic.
