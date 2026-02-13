---
title: "Adding arrows to networkx"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2018-05-29
description: "Contributing proper arrow rendering for directed networks to the networkx library."
---

## The problem

Surprisingly, `networkx`—the most popular Python package for network analysis—didn’t render proper arrows when drawing directed graphs. Instead, it used rectangular markers to indicate edge direction.

As a regular user, I wanted to fix this and contribute back to the open-source community.

## The contribution

The workflow was straightforward:

1. I opened an [issue](https://github.com/networkx/networkx/issues/2757) on the networkx GitHub repo
2. After the maintainers confirmed it was worth pursuing, I submitted a [pull request](https://github.com/networkx/networkx/pull/2760)
3. After some review and discussion, the code was merged

**Before:**

{{< figure src="/images/old_networkx.png" caption="Figure 1: Directed network rendering before the fix." >}}

**After:**

{{< figure src="/images/new_networkx.png" caption="Figure 2: Directed network rendering with proper arrows." >}}

Working on this contribution taught me a lot about the internals of `networkx`’s drawing code and `matplotlib` more broadly. Contributing to an open-source project is one of the best ways to improve as a programmer.
