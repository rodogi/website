---
title: "NetMedPy"
description: "A Python package for large-scale network medicine screening."
tags: ["Bio", "Network Science", "Network Medicine"]
date: 2024-03-01
---

Network medicine views diseases as localized disruptions in networks of genes, proteins, and other molecular entities, offering a framework to understand how biological systems communicate and fail. NetMedPy is an open-source Python package that streamlines large-scale network medicine analyses, enabling researchers to evaluate network localization, compute proximity and separation between biological entities, and screen thousands of drug–disease pairs efficiently.

The package extends traditional approaches by providing four built-in distance metrics (shortest paths, random walk, biased random walk, and communicability) and four null models (degree match, degree logarithmic binning, strength logarithmic binning, and uniform), while also supporting user-defined custom metrics. By leveraging precomputed distance matrices, NetMedPy optimizes calculations for large interactomes, making network medicine screening practical at scale.

Published in [Bioinformatics, 2025](https://academic.oup.com/bioinformatics/article/41/9/btaf338/8165428). Code available on [GitHub](https://github.com/menicgiulia/NetMedPy).
