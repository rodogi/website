---
title: "Public procurement networks"
description: "Analysis of public procurement networks and their implications for policy and governance."
date: 2024-01-15
image: "/images/new_networkx.png"  # Add your image here
---

## Overview

Public procurement networks represent the complex relationships between government entities and private contractors. Our research focuses on understanding these relationships and their impact on public spending efficiency.

### Key Findings

1. Network structure reveals preferential attachments between certain contractors and government entities
2. Community detection algorithms identify potential procurement clusters
3. Temporal analysis shows evolution of procurement patterns

![Procurement Network Visualization](/images/favs.png)

### Methodology

We applied several network analysis techniques:
- Community detection
- Centrality measures
- Temporal evolution analysis

```python
# Example code snippet
import networkx as nx

def analyze_procurement_network(edges):
    G = nx.Graph()
    G.add_edges_from(edges)
    communities = nx.community.louvain_communities(G)
    centrality = nx.eigenvector_centrality(G)
    return communities, centrality
```

### Publications

1. "Structure and dynamics of public procurement networks" (2023)
2. "Temporal evolution of contractor relationships" (2024)

### Collaborators

- Dr. Jane Smith (Harvard University)
- Prof. John Doe (MIT)