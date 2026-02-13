---
title: "Create a network with graph-tool and pandas"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-06-26
description: "A tutorial on building graph-tool networks from pandas DataFrames."
---

## graph-tool

[graph-tool](https://graph-tool.skewed.de/) is a Python package for network analysis backed by C++ and the Boost Graph Library. Compared to the more popular `networkx`, it offers significantly better performance for large networks.

Some of its standout features:

- **Filters and views** for extracting subgraphs without copying data
- **Interactive drawing** and beautiful graph layouts
- **Fast topological algorithms**

The main downside: documentation could use more worked examples—which is partly why I’m writing this.

## Pandas + graph-tool

Network analysis often involves inspecting structural parameters (degree statistics, centrality) or working with node/edge attributes in tabular form. Pandas makes this natural.

Suppose we have an edge list as a CSV:

| node 1 | node 2 | color | weight |
|--------|--------|-------|--------|
| a | b | red | 2 |
| a | c | black | 5 |
| b | c | red | 1 |

Here’s how to read it into a graph-tool `Graph` with edge properties:

```python
import pandas as pd
import graph_tool.all as gt

df = pd.read_csv("table.csv")

g = gt.Graph()
weight = g.new_edge_property("int")
color = g.new_edge_property("string")

edgelist = df.values
node_id = g.add_edge_list(edgelist, hashed=True, eprops=[color, weight])

for node in range(g.num_vertices()):
    print(f"Node {node} has id: {node_id[node]}")
```

Before saving, register the properties as internal property maps so they persist:

```python
g.vertex_properties["node_id"] = node_id
g.edge_properties["color"] = color
g.edge_properties["weight"] = weight

g.save("my_network.graphml")
```
