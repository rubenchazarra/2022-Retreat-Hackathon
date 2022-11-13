# 2022-Retreat-Hackathon

This is the repo for the Mele Lab 2022 Retreat Mini Hackathon

## Introduction to the problem
In **[Zimmerman KD et al, 2021]([url](https://www.nature.com/articles/s41467-021-21038-1))** they hypothesize that measures from cells from the same individual should be more (positively) correlated with each other than cells from unrelated individuals. Empirically, this appears true across a range of cell types (Fig. 1). They demonstrate this effect by estimating the pairwise correlation of cells within an individual and across any two different individuals. For a given cell type, the correlation of cells within an individual (intra-individual correlation) is always higher than the correlation of cells across individuals (inter-individual correlation). Thus, single-cell data have a hierarchical structure in which the single cells may not be mutually independent and have a study-specific correlation (e.g., exchangeable correlation within an individual).

However, we think the code they provide to estimate the **intra and inter-individual correlation** could be optimized. Hence, we will provide the instructions you need to calculate these correlations.

## Required Software

All of the required software is already installed in MareNostrum4.

### R software
*>=4.1.2 version*

```
module load R/4.1.2
```

### R packages (optional)

```
library(Matrix)
library(data.table)
library(purrr)
library(gdata)
library(ggplot2)
library(plyr)
library(dplyr)
library(reshape2)
library(stringi)
library(stringr)
library(textTinyR)
```


### Benchmark your code

We encourage you benchmark (time/memory) of the different pieces of your code. You can use:

**Time**: `microbenchmark` R package
* [CRAN package]([url](https://cran.r-project.org/web/packages/microbenchmark/index.html)) 
* Other resources: 
- https://www.r-bloggers.com/2015/01/using-the-microbenchmark-package-to-compare-the-execution-time-of-r-expressions/
- http://adv-r.had.co.nz/Performance.html#microbenchmarking
3. Other packages/functions: https://www.alexejgossmann.com/benchmarking_r/

**(!) Of note:** You can also use the regular `base::system.time()`. 

**Memory**: `profvis` R package
* [CRAN package]([url]([https://cran.r-project.org/web/packages/microbenchmark/index.html](https://cran.r-project.org/web/packages/profvis/index.html))) 
* Other resources: 
- https://www.r-bloggers.com/2015/01/using-the-microbenchmark-package-to-compare-the-execution-time-of-r-expressions/
(!) Note that it requires `pandoc` for compilation, so currently, it can only be used through Rstudio (not in the HPC machines).

## HPC resources
- You can NOT use parallelisation in the HPC machine (job arrays/Greasy).
- You can run your final script inside an interactive session:
```
salloc -n 24 -N 1
```

## Detailed Instructions

As a **testing case** you will use **single-cell data** from **CD4 T cells** (32,738 genes x 12,777 cells). You will have to calculate the pairwise correlation of cells within an individual **(intra-correlation)** and across any two different individuals **(inter-correlation)**. To simplify the task, this analysis will be performed within the different age groups (Y=Young, M=Middle and O=Old) and using only a subset of cells and genes. If you have time, you can also try to perform the analysis within each of the randomly created N groups, where N is the number of levels of the phenotype (in this case 3). See the **Constraints** section for further details.

### Inputs

As a **testing case** you will use **single-cell data** from **CD4 T cells** (32,738 genes x 12,777 cells)

* Count matrix (dgCMatrix):

```
/gpfs/projects/bsc83/Projects/2022-MeleLab-Retreat/Hackathon/01_R_minihackathon/inputs/CD4_T.dgCMatrix.rds
```

**(!) Of note:** It is better if you load the `Matrix` package (`library(Matrix)`) before reading this file.

* Metadata (data frame):

```
/gpfs/projects/bsc83/Projects/2022-MeleLab-Retreat/Hackathon/01_R_minihackathon/inputs/CD4_T.metadata.rds
```

### Constraints

**1. Data-related**

**Splitting the raw input data**

- The **aging variable** in the metadata file is *Age_cat*, with the levels: Y=Young, M=Middle and O=Old.
- If you have time, you can create N random groups, where N is the number of levels of the phenotype (in this case 3).
- The analyses should be performed within each of the levels of the grouping variable. 

**Subsetting the cells of the splitted data**

- You only need to use 50% of the cells (within each group).

**Filtering the genes of the splitted data**

- Filtering the lowly variable and lowly expressed genes: You only need to consider the genes with a mean expression of 0.5 (within each group).
- Filtering the highly correlated genes: To control for the correlation structure between genes, genes were sampled one at a time, and any genes with a Spearman’s correlation coefficient >0.25 relative to the gene that was drawn were subsequently trimmed from the dataset. This step was repeated until either no more uncorrelated genes remained or a total of 500 uncorrelated genes were obtained, whichever happened first.

**2. Correlation-related**

For both the inter- and intra- pairwise correlations, you should use the Spearman’s correlation.

*Inter-correlation*
- Pull one random cell per individual
- Compute the inter-individiual pairwise correlation (donor-donor, each one represented by 1 random cell)
- Repeat the proces 10 times

*Intra-correlation*
- Among all the possible intra-pairwise correlations (cell-cell), you only have to select 20 random pairs

**3. Output-related**

- Data: You have to provide a unique data frame with the inter- and intra- pairwise correlation results with the following columns:

*"Group*: age levels (Y, M, O)
*"Type"*: type of correlation (Inter or Intra)
*"CellType*: cell type of interest (CD4_T)
*"Correlation*: Spearman's correlation value

- Plots: If possible, you should provide a boxplot for each age level (or for all levels together) splitted by Intra and Inter categories.

**(!) Of note:** You can create your own directory to perform the analysis here `/gpfs/projects/bsc83/Projects/2022-MeleLab-Retreat/Hackathon/01_R_minihackathon` 
