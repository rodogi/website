---
title: "What cities appear most often in El País articles?"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-07-22
description: "Scraping a year of El País front pages to find which cities dominate the Latin American edition."
toc: true
---

## Introduction

El País is one of the most widely read newspapers in the Spanish-speaking world. As a regular reader of its Latin American edition, I noticed that coverage seemed unevenly distributed—most front-page stories appeared to come from Mexico.

To test this impression, I scraped every front-page article from 2018 using Python’s `requests` and `beautifulsoup` libraries.

## Results

The [El País](https://elpais.com/elpais/portada%5Famerica.html) front page is structured in HTML with articles enclosed in `<article>` tags. Each article’s title lives under the class `"articulo-titulo"`, and its dateline under `"articulo-localizacion"`.

Here’s the basic scraping setup:

```python
import requests
from bs4 import BeautifulSoup

r = requests.get("https://elpais.com/elpais/portada_america.html")
soup = BeautifulSoup(r.text, "html.parser")

articles = soup.find_all("article", attrs={"class": "articulo"})
loc = soup.find_all("span", attrs={"class": "articulo-localizacion"})
```

By iterating through every day of 2018, I collected 33,285 articles:

1. 17,368 had **no location** identified
2. 703 distinct cities appeared
3. 52 cities appeared more than 20 times
4. **Madrid** led with 3,327 mentions, followed by **Mexico City** with 2,191

<a id="orgd4e05ef"></a>

{{< figure src="/images/freq_city.png" caption="Figure 1: Frequency of cities with more than 20 appearances on the El País front page (Latin American edition, 2018)." >}}

## Conclusion

Despite being the Latin American edition:

1. **Madrid** still has the most front-page appearances
2. Mexico City is second
3. Two U.S. cities rank third and fourth
4. Buenos Aires and Bogotá follow

Latin American cities are well represented overall, but the newspaper’s Spanish roots mean that Spanish news dominates—even in the edition aimed at Latin America.
