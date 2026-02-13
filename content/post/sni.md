---
title: "A quick look at Mexico’s National System of Researchers"
author: ["Rodrigo Dorantes-Gilardi"]
date: 2019-09-28
description: "Parsing Mexico’s SNI 2019 results with Python to explore gender gaps across researcher levels."
toc: true
---

## SNI

The *Sistema Nacional de Investigadores* (SNI) is a Mexican public program that awards grants to researchers based on academic productivity. Founded in 1984, it remains a cornerstone of Mexico’s scientific ecosystem.

In 2019, the SNI published its annual results as a PDF—available [here](https://www.conacyt.gob.mx/images/SNI/2019/RESULTADOS%5FSNI%5FCONVOCATORIA%5F2019%5FINGRESO%5FO%5FPERMANENCIA.pdf). I decided to parse the data and take a closer look.

## Basic Overview

The [historical records](https://www.conacyt.gob.mx/index.php/el-conacyt/sistema-nacional-de-investigadores/archivo-historico) list all active SNI members as of 2018. Reading the Excel file into pandas:

```python
import pandas as pd

df_all = pd.read_excel("/Users/rdora/Downloads/BENEFICIARIOS_2018.xlsx")
df_all = df_all.iloc[:, 0:14]
print(df_all.shape)
```

This gives us **28,633** active SNI researchers.

## 2019 Results

The 2019 results PDF spans 203 pages. Using `tabula-py` to extract the tables:

```python
import tabula

path = "/Users/rdora/Downloads/RESULTADOS_SNI_CONVOCATORIA_2019_INGRESO_O_PERMANENCIA.pdf"
dfs = [tabula.read_pdf(path, pages=str(p)) for p in range(1, 204)]
df = pd.concat(dfs, axis=0).dropna()
```

After cleaning, we get **9,942 candidates** with their SNI level and name. To find new researchers:

```python
df_all["NAME"] = df_all["PATERNO"] + " " + df_all["MATERNO"] + "," + df_all["NOMBRE"]
df_new = df[~df.NOMBRE.isin(df_all.NAME)]
```

Of the **5,338 new researchers**, 42% are female—compared to 37% among existing members.

{{< figure src="/images/gender_sni.png" caption="Figure 1: Gender distribution of new members" >}}

{{< figure src="/images/gender_sni_all.png" caption="Figure 2: Gender distribution of existing members" >}}

Researchers are assigned one of six levels: Candidate (C), one- or two-year extensions (PC1, PC2), and SNI Levels 1–3. New members are mostly Candidates (54%) and Level 1 (38%), with only 1.5% reaching Level 3.

{{< figure src="/images/dist_sni.png" caption="Figure 3: New members by SNI level" >}}

The gender gap widens at higher levels: female researchers are more likely to be Candidates (57% vs. 51% of males), while males are 1.5× more likely to reach Level 2 and 3× more likely to reach Level 3.

{{< figure src="/images/dist_sni_M.png" caption="Figure 4: New members by SNI level — Males" >}}

{{< figure src="/images/dist_sni_F.png" caption="Figure 5: New members by SNI level — Females" >}}
