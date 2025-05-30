---
title: "RNA-seq Part II"
author: "Cesia Mendez"
format: html
editor: visual
execute:
  keep-md: true
---




# **Performing Quantitative RNA-seq, Part II**

In the following project, RNA- sequence data from Arabidopsis thaliana will be analyzed and visualized using heat maps to identify patterns in gene expression across different ecotypes and plant parts. The ComplexHeatmap package in R will be used to generate a heat map, with color palettes to enhance visual interpretation. The heat map will help reveal trends in gene expression, highlighting which genes are more active in specific conditions.

### Installing the ComplexHeatmap Package for RNA-seq Visualization

The following code ensures that the BiocManager package is available, which is required to install the Bioconductor packages in R. If the BiocManager package is not already installed, the script installs it. Then, it uses the BiocManager package to install the ComplexHeatmap package, which is essential for generating heat maps to visualize RNA- sequence data.



::: {.cell}

```{.r .cell-code}
#if (!require("BiocManager", quietly = TRUE))
    #install.packages("BiocManager")

#BiocManager::install("ComplexHeatmap")
```
:::



After running the code, the ComplexHeatmap package will be installed, allowing us to create visually formative heat maps for large genomic data sets.

### Installing Required Packages for Heatmap Visualization

The following code installs four essential R packages needed for generating and customizing heat maps. The viridisLite package will provide color palettes optimized for data visualization, the stringr package allows to manipulate the text data easily, the RColorBrewer package offers a collection of color suitable for categorical and sequential data, and the circlize package enables advanced color mapping and notes functions for complex visualization.



::: {.cell}

```{.r .cell-code}
#install.packages("viridisLite")
#install.packages("stringr")
#install.packages("RColorBrewer")
#install.packages("circlize")
```
:::



Once installed, these packages will provide the necessary tools to structure and enhance readability of heat maps, facilitating the identification of gene expression patterns across different conditions.

### Creating a Heat Map to Visualize Gene Expression

A heat map will be created using the ComplexHeatmap package in R. This will be used to display gene expression levels, making it easier to see which genes are active in different conditions.



::: {.cell}

```{.r .cell-code}
library(ComplexHeatmap)
```

::: {.cell-output .cell-output-stderr}

```
Loading required package: grid
```


:::

::: {.cell-output .cell-output-stderr}

```
========================================
ComplexHeatmap version 2.22.0
Bioconductor page: http://bioconductor.org/packages/ComplexHeatmap/
Github page: https://github.com/jokergoo/ComplexHeatmap
Documentation: http://jokergoo.github.io/ComplexHeatmap-reference

If you use it in published research, please cite either one:
- Gu, Z. Complex Heatmap Visualization. iMeta 2022.
- Gu, Z. Complex heatmaps reveal patterns and correlations in multidimensional 
    genomic data. Bioinformatics 2016.


The new InteractiveComplexHeatmap package can directly export static 
complex heatmaps into an interactive Shiny app with zero effort. Have a try!

This message can be suppressed by:
  suppressPackageStartupMessages(library(ComplexHeatmap))
========================================
```


:::
:::



By the end of this exercise, a heat map will be generated that will visually represent gene expression data. This will help to identify trends and differences in gene activity across samples.

### Setting Up Color Palettes for RNA-seq Data Visualization

The following code will load for R packages that will help create color palettes and format annotations for visualizing RNA-seq data. The viridisLite package will provide color schemes that are easy to interpret, the stringr package helps to manipulate text, the RColorBrewer package offers a variety of color palettes, and the circlize package allows to custom color gradients.



::: {.cell}

```{.r .cell-code}
library(viridisLite)
```

::: {.cell-output .cell-output-stderr}

```
Warning: package 'viridisLite' was built under R version 4.4.3
```


:::

```{.r .cell-code}
library(stringr)
```

::: {.cell-output .cell-output-stderr}

```
Warning: package 'stringr' was built under R version 4.4.3
```


:::

```{.r .cell-code}
library(RColorBrewer)
library(circlize)
```

::: {.cell-output .cell-output-stderr}

```
Warning: package 'circlize' was built under R version 4.4.3
```


:::

::: {.cell-output .cell-output-stderr}

```
========================================
circlize version 0.4.16
CRAN page: https://cran.r-project.org/package=circlize
Github page: https://github.com/jokergoo/circlize
Documentation: https://jokergoo.github.io/circlize_book/book/

If you use it in published research, please cite:
Gu, Z. circlize implements and enhances circular visualization
  in R. Bioinformatics 2014.

This message can be suppressed by:
  suppressPackageStartupMessages(library(circlize))
========================================
```


:::
:::



After running this code, the necessary tools for applying colors to heat maps and other visualizations will be available, making it easier to identify patterns in gene expression data.

### Loading the RNA-seq Data Set

The following code loads the rbioinfcookbook package, which contains the RNA- seq data set that will be used to create a heat map. The data set includes gene expression data for Arabidopsis thaliana across different ecotypes and plant parts.



::: {.cell}

```{.r .cell-code}
library(rbioinfcookbook)
```
:::



The package is successfully loaded, allowing access to the data set needed for further analysis.

### Analyzing Gene Expression Patterns in Arabidopsis thaliana

The following code converts the data into a matrix and transforms it using a logarithm to make patterns easier to observe. Then, the sample names are split to extract two important pieces of information: the plant's genetic variation and the plant part, such as the leaf or root.



::: {.cell}

```{.r .cell-code}
mat <- log(as.matrix(at_tf_gex[ , 5:55]))
ecotype <- stringr::str_split(colnames(mat), ",", simplify = TRUE)[,1]
part <- stringr::str_split(colnames(mat), ",", simplify = TRUE)[,2]
```
:::



The processed data will be ready to be visualized in a heat map, where gene expression differences can be compared across ecotypes and plant parts. This will help to identify the trends and variations in gene activity.

### Creating Color Schemes for Heat Map Visualization

The following code sets up color schemes to improve the readability of the heat map. It defines a color gradient for gene expression values using the magma palette from the viridisLite package. Additionally, it assigns different colors to different plant ecotypes and parts using color palettes from the RColorBrewer package, ensuring clear visual separation in the heat map.



::: {.cell}

```{.r .cell-code}
data_col_func <- circlize::colorRamp2(seq(0, max(mat), length.out = 6), viridisLite::magma(6))

ecotype_colors <- c(RColorBrewer::brewer.pal(12, "Set3"), RColorBrewer::brewer.pal(5, "Set1"))
names(ecotype_colors) <- unique(ecotype)

part_colors <- RColorBrewer::brewer.pal(3, "Accent")
names(part_colors) <- unique(part)
```
:::



The generated color schemes will be applied to the heat map, making it easier to interpret patterns in gene expression across different ecotypes and plant parts.

### Adding Annotations to a Heat Map

The following code creates two types of annotations for the heat map. The top annotation labels each sample with its ecotype and plant part, using specific colors for easy identification. The side annotation marks each gene with a point representing its length, helping to compare gene sizes visually.



::: {.cell}

```{.r .cell-code}
top_annot <- HeatmapAnnotation("Ecotype" = ecotype, "Plant Part" = part, col = list("Ecotype" = ecotype_colors, "Plant Part" = part_colors), annotation_name_side = "left")

side_annot <- rowAnnotation(length = anno_points(at_tf_gex$Length, pch = 16, size = unit(1, "mm"), axis_param = list(at = seq(1, max(at_tf_gex$Length), length.out = 4)),))
```
:::



The labels and colors for ecotype and plant part will be displayed at the top in the heat map, making it easier to distinguish each group. On the side, small points will show gene lengths, allowing to compare the gene sizes.

### Creating a Heat Map to Visualize Gene Expression

The following code will create a heat map to display gene expression levels using the RNA-seq data. The heat map will group similar genes and samples together based on their expression patterns. Colors will represent different expression levels, and the annotations will help to indicate ecotypes and plant parts.



::: {.cell}

```{.r .cell-code}
ht_1 <- Heatmap(mat, name="log(TPM)", row_km = 6, col = data_col_func, top_annotation = top_annot, right_annotation = side_annot, cluster_columns = TRUE, column_split = ecotype, show_column_names = FALSE, column_title = " ")

ComplexHeatmap::draw(ht_1)
```

::: {.cell-output-display}
![](RNA-seq-Part-II_files/figure-html/unnamed-chunk-9-1.png){width=672}
:::
:::



The heat map show clusters of genes with similar expression patterns, making it easier to identify trends across different ecotypes and plant parts. This visualization helps to understand how gene expression varies under different conditions.
