---
title: "I forgive your taxes"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-10-02
description: "Analyzing 12 billion pesos in Mexican tax exemptions across three presidencies."
toc: true
---

## Overview

My friend and mentor [Chucho](https://scholar.google.com.mx/citations?user=Aoa-qxcAAAAJ&hl=en) pointed me to a recently published [database](https://privilegiosfiscales.fundar.org.mx/) of tax exemptions granted in Mexico between 2007 and 2019. The data, compiled by the Mexican organization Fundar, includes the year, amount, and recipient of each exemption. Here I focus on tax reductions for individuals (a separate analysis could cover cancellations and corporate exemptions).

The data spans three presidencies:

- **Felipe Calderón Hinojosa (FCH):** 2006–2012
- **Enrique Peña Nieto (EPN):** 2012–2018
- **Andrés Manuel López Obrador (AMLO):** 2018–

| FCH | EPN | AMLO |
|-----|-----|------|
| 5,716 M | 5,792 M | 12 M |

A combined **12 billion pesos** were forgiven, split almost evenly between FCH and EPN. AMLO’s total stood at just 12 million at the time of analysis.

{{< figure src="/images/sum_president.png" caption="Figure 1: Total amount forgiven per president (million pesos)" >}}

A striking pattern: the vast majority of exemptions under FCH occurred in his first two years, and under EPN in his first year alone. This raises the question of whether some exemptions were effectively debt settlements.

{{< figure src="/images/sum_year.png" caption="Figure 2: Amount forgiven per year" >}}

## Felipe Calderón Hinojosa

FCH granted exemptions to 1,290 individuals, with 96.6% of the total concentrated in his first two years. The largest single exemption went to Carlos Efraín De Jesús Cabal Peniche (500 M pesos), followed by Miguel Fernando Valladares García (221 M pesos).

{{< figure src="/images/favs_fch.png" caption="Figure 3: Top recipients under FCH" >}}

## Enrique Peña Nieto

EPN’s list was longer—4,352 individuals—with 94% of the total forgiven in his first year.

{{< figure src="/images/favs_epn.png" caption="Figure 4: Top recipients under EPN" >}}

## Overlap between presidencies

133 individuals received exemptions under both FCH and EPN. Notably, none of these individuals appeared in AMLO’s first-year records.

{{< figure src="/images/mutual.png" caption="Figure 5: Individuals who received exemptions under both FCH and EPN" >}}
