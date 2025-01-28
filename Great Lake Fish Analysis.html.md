---
title: "Great Lake Fish Analysis"
author: "Cesia Mendez"
format: html
editor: visual
execute:
  keep-md: true
---




## Great Lake Fish Analysis 

Commercial fish catch data were published by the Great Lakes Fishery Commission in 1962 and covered the period 1867-1960. A supplement covering the years 1961-1968 was released in 1970, and a revised edition covering the years 1867-1977 was published in 1979. This third update of a web-based version covers the period 1867-2015. The intent is to update at approximately five-year intervals. The files are intended for open use by the public. We ask only that the commission be acknowledged when these records are used in presentations and publications.

## Getting Data 

Data will be updated here



::: {.cell}

```{.r .cell-code}
fishing <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2021/2021-06-08/fishing.csv')
```

::: {.cell-output .cell-output-stderr}

```
Rows: 65706 Columns: 7
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (4): lake, species, comments, region
dbl (3): year, grand_total, values

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::

```{.r .cell-code}
stocked <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2021/2021-06-08/stocked.csv')
```

::: {.cell-output .cell-output-stderr}

```
Warning: One or more parsing issues, call `problems()` on your data frame for details,
e.g.:
  dat <- vroom(...)
  problems(dat)
```


:::

::: {.cell-output .cell-output-stderr}

```
Rows: 56232 Columns: 31
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (17): LAKE, STATE_PROV, SITE, ST_SITE, STAT_DIST, LS_MGMT, SPECIES, STRA...
dbl (14): SID, YEAR, MONTH, DAY, LATITUDE, LONGITUDE, GRID, NO_STOCKED, YEAR...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::
:::

::: {.cell}

```{.r .cell-code}
head(fishing)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 7
   year lake  species      grand_total comments region            values
  <dbl> <chr> <chr>              <dbl> <chr>    <chr>              <dbl>
1  1991 Erie  American Eel           1 <NA>     Michigan (MI)          0
2  1991 Erie  American Eel           1 <NA>     New York (NY)          0
3  1991 Erie  American Eel           1 <NA>     Ohio (OH)              0
4  1991 Erie  American Eel           1 <NA>     Pennsylvania (PA)      0
5  1991 Erie  American Eel           1 <NA>     U.S. Total             0
6  1991 Erie  American Eel           1 <NA>     Canada (ONT)           1
```


:::
:::

::: {.cell}

```{.r .cell-code}
#install.packages("tidymodels")
library(tidymodels)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching packages ────────────────────────────────────── tidymodels 1.2.0 ──
```


:::

::: {.cell-output .cell-output-stderr}

```
✔ broom        1.0.7     ✔ recipes      1.1.0
✔ dials        1.3.0     ✔ rsample      1.2.1
✔ dplyr        1.1.4     ✔ tibble       3.2.1
✔ ggplot2      3.5.1     ✔ tidyr        1.3.1
✔ infer        1.0.7     ✔ tune         1.2.1
✔ modeldata    1.4.0     ✔ workflows    1.1.4
✔ parsnip      1.2.1     ✔ workflowsets 1.1.0
✔ purrr        1.0.2     ✔ yardstick    1.3.1
```


:::

::: {.cell-output .cell-output-stderr}

```
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ purrr::discard() masks scales::discard()
✖ dplyr::filter()  masks stats::filter()
✖ dplyr::lag()     masks stats::lag()
✖ recipes::step()  masks stats::step()
• Use tidymodels_prefer() to resolve common conflicts.
```


:::

```{.r .cell-code}
my_data_splits <- initial_split(fishing, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::

::: {.cell}

```{.r .cell-code}
head(test_data)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 7
   year lake  species      grand_total comments region        values
  <dbl> <chr> <chr>              <dbl> <chr>    <chr>          <dbl>
1  1991 Erie  American Eel           1 <NA>     New York (NY)      0
2  1991 Erie  American Eel           1 <NA>     Ohio (OH)          0
3  1991 Erie  American Eel           1 <NA>     U.S. Total         0
4  1991 Erie  American Eel           1 <NA>     Canada (ONT)       1
5  1992 Erie  American Eel           0 <NA>     Michigan (MI)      0
6  1992 Erie  American Eel           0 <NA>     New York (NY)      0
```


:::
:::

::: {.cell}

```{.r .cell-code}
tail(test_data)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 7
   year lake        species      grand_total comments region          values
  <dbl> <chr>       <chr>              <dbl> <chr>    <chr>            <dbl>
1  2008 Saint Clair Sheepshead             6 <NA>     Canada (ONT)         6
2  2008 Saint Clair Suckers                6 <NA>     U.S. Total (MI)     NA
3  2008 Saint Clair Suckers                6 <NA>     Canada (ONT)         6
4  2008 Saint Clair White Bass             4 <NA>     Canada (ONT)         4
5  2008 Saint Clair Yellow Perch          NA <NA>     U.S. Total (MI)     NA
6  2008 Saint Clair Yellow Perch          NA <NA>     Canada (ONT)        NA
```


:::
:::



### Hypotheses 

1.  Do the number of species vary by region?
2.  Do the types of species vary by region?
3.  Does the number of species in a region depend on the resources available in that region?
