# 2022-Retreat-Hackathon


 This is the repo for the Mele Lab 2022 Retreat Mini Hackathon
 
## Introduction to the problem
In **[Zimmerman KD et al, 2021]([url](https://www.nature.com/articles/s41467-021-21038-1))** they hypothesize that measures from cells from the same individual should be more (positively) correlated with each other than cells from unrelated individuals. Empirically, this appears true across a range of cell types (Fig. 1). They demonstrate this effect by estimating the pairwise correlation of cells within an individual and across any two different individuals. For a given cell type, the correlation of cells within an individual (intra-individual correlation) is always higher than the correlation of cells across individuals (inter-individual correlation). Thus, single-cell data have a hierarchical structure in which the single cells may not be mutually independent and have a study-specific correlation (e.g., exchangeable correlation within an individual).

However, we think the code they provide to estimate the **intra and inter-individual correlation** could be optimized. Hence, we will provide the instructions you need to calculate these correlations.

## Required Software
**R version** 
>=4.1.2 version (already installed in MareNostrum4)

**R packages** (optional)
library(...)

**Benchmark your code**
* Time:
microbenchmark()

* Memory:
profvis()

## HPC resources
- You can NOT use parallelisation in the HPC machine (job arrays/Greasy).
- You can run your final script inside an interactive session:
```
salloc -n 24 -N 1
```

## Detailed Instructions
