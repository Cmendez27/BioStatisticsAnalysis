---
title: "Palmer Penguins Initial Analysis"
author: "Cesia Mendez"
format: html
editor: visual
execute:
  keep-md: true
---




## Palmer Penguins 

The Palmer Penguins data set is one of the most common resources for data analysis and visualization with complex observations from three species: Adélie, Chinstrap, and Gentoo. The penguins live on different islands in the Palmer Archipelago near Antarctica. In this data set, there is information about the species, island location, bill length and depth, flipper length, body mass, and sex. Therefore, it is also an easy, ecologically more relevant analog of the classical Iris data set and a real example to conduct statistical analyses, machine learning, and data visualization in R and Python. It is highly accessible to researchers and instructors for teaching applied explorations in biological data.



::: {.cell}

```{.r .cell-code}
#Load the tidyverse
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.4     ✔ readr     2.1.5
✔ forcats   1.0.0     ✔ stringr   1.5.1
✔ ggplot2   3.5.1     ✔ tibble    3.2.1
✔ lubridate 1.9.4     ✔ tidyr     1.3.1
✔ purrr     1.0.2     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


:::

```{.r .cell-code}
#Read the penguins_samp1 data file from github
penguins <- read_csv("https://raw.githubusercontent.com/mcduryea/Intro-to-Bioinformatics/main/data/penguins_samp1.csv")
```

::: {.cell-output .cell-output-stderr}

```
Rows: 44 Columns: 8
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (3): species, island, sex
dbl (5): bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g, year

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::

```{.r .cell-code}
#See the first six rows of the data we've read in to our notebook
penguins %>% head()
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 8
  species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
  <chr>   <chr>           <dbl>         <dbl>             <dbl>       <dbl>
1 Gentoo  Biscoe           59.6          17                 230        6050
2 Gentoo  Biscoe           48.6          16                 230        5800
3 Gentoo  Biscoe           52.1          17                 230        5550
4 Gentoo  Biscoe           51.5          16.3               230        5500
5 Gentoo  Biscoe           55.1          16                 230        5850
6 Gentoo  Biscoe           49.8          15.9               229        5950
# ℹ 2 more variables: sex <chr>, year <dbl>
```


:::
:::



The data set is represented in a table of six rows and eight columns of measurements of Gentoo penguins from Biscoe Island across different years. The variables include species, island, and numeric measurements such as bill length (in mm), bill depth (in mm), flipper length (in mm), and body mass in grams. The data set also provides the sex of the penguins, listed as all males, and the year of observation, which runs from 2007 to 2009. The measurements of penguins have consistent flipper lengths at approximately 230 mm, with small variations in bill dimensions and body mass, indicating uniformity in the sample population's physical characteristics.
