---
title: "Performing Quantitative RNA-seq"
author: "Cesia Mendez"
format: html
editor: visual
execute:
  keep-md: true
---




## Performing Quantitative RNA-seq

The following project is an introduction to RNA sequencing, which is a powerful tool used by scientists to study gene activity in organisms. RNA sequencing helps to identify which genes are activated in different conditions in order to provide valuable insights in different biological processes.

In this first part, an R and edgeR package will be used to analyze RNA sequencing data. A Drosophila melanogaster dataset sample will be loaded to compare the gene expression between two stages of larvae, and identify which genes are significantly different between them.

### Installing and Loading the devtools package

The following code installs the devtools package and loads it into the R session. The devtools package will provide tools for package development and will allow to install packages directly from GitHub



::: {.cell}

```{.r .cell-code}
#install.packages("devtools")
library(devtools)
```

::: {.cell-output .cell-output-stderr}

```
Warning: package 'devtools' was built under R version 4.4.3
```


:::

::: {.cell-output .cell-output-stderr}

```
Loading required package: usethis
```


:::
:::



### Installing and Loading the rbioinfcookbook package from GitHub

The following code installs the rbioinfcookbook package from GitHub, allowing to install R packages directly from repositories.



::: {.cell}

```{.r .cell-code}
#devtools::install_github("danmaclean/rbioinfcookbook")
library(rbioinfcookbook)
```
:::



### Installing and Loading forcats, edgeR, and Biobase Packages

The following code will install and load essential R packages for data manipulation and bioinformatics analysis. The forcats package is installed using the function install.packages(), while the edgeR and Biobase packages, which are from the Bioconductor repository, are installed using the function BiocManager::install(). Finally, the function library() will load the forcats and edgeR packages into the R session to be used.



::: {.cell}

```{.r .cell-code}
#install.packages("forcats")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("edgeR")

#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("Biobase")

library(forcats)
```

::: {.cell-output .cell-output-stderr}

```
Warning: package 'forcats' was built under R version 4.4.3
```


:::

```{.r .cell-code}
library(edgeR)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: limma
```


:::
:::



### Loading the Biobase Package

The following code will load the Biobase package, which is a package that provides tools for organizing and analyzing biological data, especially for genomics research.



::: {.cell}

```{.r .cell-code}
library(Biobase)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: BiocGenerics
```


:::

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'BiocGenerics'
```


:::

::: {.cell-output .cell-output-stderr}

```
The following object is masked from 'package:limma':

    plotMA
```


:::

::: {.cell-output .cell-output-stderr}

```
The following objects are masked from 'package:stats':

    IQR, mad, sd, var, xtabs
```


:::

::: {.cell-output .cell-output-stderr}

```
The following objects are masked from 'package:base':

    anyDuplicated, aperm, append, as.data.frame, basename, cbind,
    colnames, dirname, do.call, duplicated, eval, evalq, Filter, Find,
    get, grep, grepl, intersect, is.unsorted, lapply, Map, mapply,
    match, mget, order, paste, pmax, pmax.int, pmin, pmin.int,
    Position, rank, rbind, Reduce, rownames, sapply, saveRDS, setdiff,
    table, tapply, union, unique, unsplit, which.max, which.min
```


:::

::: {.cell-output .cell-output-stderr}

```
Welcome to Bioconductor

    Vignettes contain introductory material; view with
    'browseVignettes()'. To cite Bioconductor, see
    'citation("Biobase")', and for packages 'citation("pkgname")'.
```


:::
:::



## Estimating Gene Expression Differences with edgeR

In the following section, the edgeR package will be used to analyze and identify which genes are more or less active in different conditions between samples. To practice, a sample data set from Drosophila melanogaster will be used, which has data from 147 samples and 15,000 genes. To estimate the differential expression the Trimmed Mean of M-values method will be used to ensure that differences in the size of the sample do not affect the results. Then, the Generalized Linear Model method will be used to compare the gene expression levels between different group samples. These methods will help to determine which genes demonstrate significant changes in activity under different conditions.

### Converting a Gene Count DataFrame into a Matrix

The function count_dataframe will extract the gene column from a DataFrame and store it in a separate variable called genes. On the other hand, the function count_matrix will remove the gene column from the original DataFrame and convert the remaining data into a matrix .



::: {.cell}

```{.r .cell-code}
genes <- count_dataframe[['gene']]
count_dataframe[['gene']] <- NULL
count_matrix <- as.matrix(count_dataframe)
rownames(count_matrix) <- genes
```
:::



The outcome of this code will be a matrix where each row represents a gene, and the columns contain the corresponding count data.

### Selecting Columns Based on Experimental stages 

The following code will define a list of the experimental stages L1Larvae and L2Larvae. Then, it will identify which columns in the pheno_data data set contain these stages.



::: {.cell}

```{.r .cell-code}
experiments_of_interest <- c("L1Larvae", "L2Larvae")
columns_of_interest <- which(pheno_data[['stage']] %in% experiments_of_interest)
```
:::



The outcome will be a set of column indices that belong to the L1Larvae and L2Larvae stages.

### Converting Selected Sample Stages into Factors 

The following code will extract the stage information for the selected samples L1Larvae and L2Larvae from the pheno_data and convert them into factors using the function forcats::as_factor().



::: {.cell}

```{.r .cell-code}
grouping <- pheno_data[["stage"]] [columns_of_interest] |> forcats::as_factor()
```
:::



The outcome will cause the selected sample stages to be stored as a factor variable.

### Extracting Relevant Gene Expression Data 

The following code will select specific columns from the count_matrix that correspond to the samples L1Larvae and L2Larvae and store them in counts_of_interest.



::: {.cell}

```{.r .cell-code}
counts_of_interest <- count_matrix[,counts = columns_of_interest]
```
:::



The outcome will be a smaller matrix which contains only the gene expression data for the selected samples. This will facilitate the analysis of different gene expressions between two conditions.

### Creating a DGEList Object for Differential Expression Analysis 

The following code will create a DGEList object using the edgeR package. It will take the filtered gene expression data using the function (counts_of_interest) and assign the experimental group labels using the function (grouping)



::: {.cell}

```{.r .cell-code}
count_dge <- edgeR::DGEList(counts = counts_of_interest, group = grouping)
```
:::



The outcome will create an organized data object that contains the gene expression counts and their sample groups.

### Estimating Differential Gene Expression with edgeR

The following code will crate a matrix based on grouped samples, estimate the variation in gene expression data, fit a general linear model to the data, and perform an statistical test to identify deferentially expressed genes. Finally, it will extract the most ranked genes that show significant differences in expression.



::: {.cell}

```{.r .cell-code}
design <- model.matrix(~grouping)
eset_dge <- edgeR::estimateDisp(count_dge, design)
fit <- edgeR::glmQLFit(eset_dge, design)
result <- edgeR::glmQLFTest(fit, coef=2)
topTags(result)
```

::: {.cell-output .cell-output-stdout}

```
Coefficient:  groupingL2Larvae 
               logFC    logCPM        F       PValue          FDR
FBgn0027527 6.318477 11.148756 43306.39 8.921836e-36 1.326588e-31
FBgn0037430 6.557811  9.109132 37053.47 4.458944e-35 2.269743e-31
FBgn0037424 6.417995  9.715826 36957.31 4.579479e-35 2.269743e-31
FBgn0037414 6.336991 10.704514 32230.76 1.878703e-34 6.983608e-31
FBgn0029807 6.334533  9.008720 30679.42 3.125484e-34 9.294565e-31
FBgn0037429 6.623754  8.525136 26529.63 1.399656e-33 3.468581e-30
FBgn0037224 7.056029  9.195077 25539.20 2.072124e-33 4.064613e-30
FBgn0030340 6.176240  8.500866 25406.05 2.186892e-33 4.064613e-30
FBgn0029716 5.167029  8.977840 24890.80 2.700890e-33 4.462171e-30
FBgn0243586 6.966860  7.769756 24146.95 3.698699e-33 5.499595e-30
```


:::
:::



The outcome is a ranked list of genes that are significantly different between sample groups. This helps to identify which genes are more or less active in different conditions.

### Summary

In this analysis, we learned how gene expression changes between the first and second larval stages. We identified genes that are expressed at different levels between these stages. This helps us to understand their potential roles in development. For example, if a gene is deferentially expressed, it means that is important for growth changes happening between these stages. Based on the results, it makes sense that some genes are more active in one stage than the other, as larvae go through significant developmental stages.
