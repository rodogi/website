+++
title = "A quick look at the Mexican national system of researchers with Python"
author = ["Rodrigo Dorantes-Gilardi"]
date = 2019-09-28T00:00:00-05:00
draft = false
toc = true
+++

## SNI {#sni}

The national system of researchers (Sistema Nacional de Investigadores) or SNI is a mexican public
organization that aims at boosting national research by giving grants to outstanding researchers
working in the country.

It was founded in 1984 and since then, it has been a key component of the Mexican
science. Yesterday, the SNI published the list of accepted, renewed, or upgraded candidates
in 2019. It did so in a PDF format that you can find [here](https://www.conacyt.gob.mx/images/SNI/2019/RESULTADOS%5FSNI%5FCONVOCATORIA%5F2019%5FINGRESO%5FO%5FPERMANENCIA.pdf). I decided to take a quick look at the
list to see what is new under the sun.


## Basic Overview {#basic-overview}

If we go to [this link](https://www.conacyt.gob.mx/index.php/el-conacyt/sistema-nacional-de-investigadores/archivo-historico), we can see the records of the actual researchers that are members in the
SNI as of 2018. I've downloaded this excel file in my `Downloads` folder and read it using pandas.

```python
import pandas as pd

df_all = pd.read_excel('/Users/rdora/Downloads/BENEFICIARIOS_2018.xlsx')
# Just take the first 14 columns
df_all = df.iloc[:, 0:14]
print(df.shape)
```

According to this, there are 28,633 SNI researchers. We'll keep this table for later.


## 2019 Stats {#2019-stats}

On September 27, SNI published the list of accepted and kept candidates for 2019. This list is a bit
more tricky to read as it is on a PDF file. Python can read tables on PDF files using a library
called `tabula-py`, that you can install with `pip install tabula-py`. Later, you can read the file
as follows.

```python
import tabula

path = "/Users/rdora/Downloads/RESULTADOS_SNI_CONVOCATORIA_2019_INGRESO_O_PERMANENCIA.pdf"
# The file contains 203 pages
pages = range(1, 204)
dfs = []
# We have to read each page individually
for page in pages:
		df = tabula.read_pdf(path, pages=str(page))
		dfs.append(df)
df = pd.concat(dfs, axis=0)
df = df.dropna()
```

After reading the table, and doing some cleaning (reading from a PDF is not cool), we can see that
there are 9,942 candidates with their SNI ID (defined by level of addition to the SNI), their name,
and their distinction (SNI level, from candidate to SNI level 3).

Let's get the number of new candidates.

```python
df_all['NAME'] = (df_all['PATERNO'] + " " +
								 df_all['MATERNO'] + "," +
								 df_all["NOMBRE"])
df_new = df[~df.NOMBRE.isin(df_all.NAME)]
```

There are 5,338 new researchers in the SNI in 2019. Using this `pd.DataFrame` we can get the number
of women and their distinction or level in the SNI. From these, 0.58 are Male and 0.42 are
Female. The overall ratio of female researchers (without the new members) is 0.37.

<a id="org3df0a84"></a>

{{< figure src="/images/gender_sni.png" caption="Figure 1: Gender distribution new members" >}}

<a id="orgd17f3d2"></a>

{{< figure src="/images/gender_sni_all.png" caption="Figure 2: Gender distribution old members" >}}

Applicants are assigned 1 of 6 categories:

-   C: candidate
-   PC1: candidate-level extension 1 year
-   PC2: candidate-level extension 2 years
-   1: SNI level 1
-   2: SNI level 2
-   3: SNI level 3

New members are mostly candidates (54%) and SNI level 1 (38%) and only 1.5% were assigned the title
of SNI level 3.

<a id="org3256f9d"></a>

{{< figure src="/images/dist_sni.png" caption="Figure 3: New members by SNI level" >}}

Female new members are more likely to be candidates (57% of female population) than males (51%), SNI
male new members are  1.5 times more likely (4.2%) to be SNI-level 2 than their female counterparts
(2.9%) and 3 times more likely to be SNI-level 3 (2% and 0.7%, respectively).

<a id="orgec373f2"></a>

{{< figure src="/images/dist_sni_M.png" caption="Figure 4: New members by SNI level: Males" >}}

<a id="org3146d4c"></a>

{{< figure src="/images/dist_sni_F.png" caption="Figure 5: New members by SNI level: Females" >}}
