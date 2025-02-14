---
title: "Great Lake Fish Analysis"
author: "Cesia Mendez"
format: html
editor: visual
execute:
  keep-md: true
---




## Great Lakes Fish Analysis

### Abstract

## Introduction

The Great Lakes form part of an ecosystem in the aquatic world and regional fisheries due to their numerous fish species. Sometimes, fish species within the same lake may be determined by the conditions around such lakes. This paper focuses on the species of fish within the Great Lakes and, in particular, tries to find out if there are differences in species among regions within the lakes.\
In this analysis, there are two major variables of interest: species of fish and the region within which they occur. The species variable includes different types of fish such as Lake Sturgeon, Northern Pike, Suckers, White Perch, and Yellow Perch, among others. The region variable refers to different areas reporting their fish populations; examples include the U.S. Total (MI), and Canada (ONT).\
This study aims to investigate how fish species vary among regions in the Great Lakes, which will also shed light on the regional ecological differences and the species distribution patterns.

## Hypothesis

**Null Hypothesis:** The distribution of species does not vary significantly across its different regions. In other words, the species can exist in any region, and there is no relationship between the type of species and the location.

**Alternative Hypothesis:** The species of fish are highly distributed among different regions within the Great Lakes. In other words, some species are more predominant in certain regions, indicating a relationship between the types of species and the regional location.

## Gathering Data



::: {.cell}

```{.r .cell-code}
#install.packages("tidyverse")
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.4     ✔ readr     2.1.5
✔ forcats   1.0.0     ✔ stringr   1.5.1
✔ ggplot2   3.5.1     ✔ tibble    3.2.1
✔ lubridate 1.9.4     ✔ tidyr     1.3.1
✔ purrr     1.0.4     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


:::

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



SOMETHING GOES HERE



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
✔ broom        1.0.7     ✔ rsample      1.2.1
✔ dials        1.3.0     ✔ tune         1.2.1
✔ infer        1.0.7     ✔ workflows    1.1.4
✔ modeldata    1.4.0     ✔ workflowsets 1.1.0
✔ parsnip      1.2.1     ✔ yardstick    1.3.2
✔ recipes      1.1.0     
```


:::

::: {.cell-output .cell-output-stderr}

```
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ scales::discard() masks purrr::discard()
✖ dplyr::filter()   masks stats::filter()
✖ recipes::fixed()  masks stringr::fixed()
✖ dplyr::lag()      masks stats::lag()
✖ yardstick::spec() masks readr::spec()
✖ recipes::step()   masks stats::step()
• Dig deeper into tidy modeling with R at https://www.tmwr.org
```


:::

```{.r .cell-code}
my_data_splits <- initial_split(fishing, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::



## Load and Inspect data

The use of the str() function will provide an overview of the data, including the types of variables and a preview of the data. This will help to identify any issues like missing values. This will confirm the presence of the columns like species and regions which are important for this analysis.



::: {.cell}

```{.r .cell-code}
# View the structure of the test_data
str(test_data)
```

::: {.cell-output .cell-output-stdout}

```
spc_tbl_ [32,853 × 7] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ year       : num [1:32853] 1991 1991 1992 1992 1993 ...
 $ lake       : chr [1:32853] "Erie" "Erie" "Erie" "Erie" ...
 $ species    : chr [1:32853] "American Eel" "American Eel" "American Eel" "American Eel" ...
 $ grand_total: num [1:32853] 1 1 0 0 0 0 0 0 0 0 ...
 $ comments   : chr [1:32853] NA NA NA NA ...
 $ region     : chr [1:32853] "Michigan (MI)" "New York (NY)" "Michigan (MI)" "U.S. Total" ...
 $ values     : num [1:32853] 0 0 0 0 0 0 0 0 0 0 ...
 - attr(*, "spec")=
  .. cols(
  ..   year = col_double(),
  ..   lake = col_character(),
  ..   species = col_character(),
  ..   grand_total = col_double(),
  ..   comments = col_character(),
  ..   region = col_character(),
  ..   values = col_double()
  .. )
 - attr(*, "problems")=<externalptr> 
```


:::
:::



After analyzing the test_data by running the str(test_data) function, it was found that the data set consists of 32,853 rows and 7 columns. The data set contains numeric variables such as year, grand total, and values. In addition to this, the data set contains categorical variables such as region, lake, species, and comments. Based on the results, no issues are present in the column types. The data set is ready to be used for further analysis.

## Count Species by Region

The following coding function will help to calculate the number of occurrences for each fish species across different regions. It will group and count species by region, then it will reshape the data so that species names become column headers. The code fills in any missing values with 0, to make sure there is a full summary of the distribution of species across regions.



::: {.cell}

```{.r .cell-code}
fishing %>%
  count(region, species) %>%
  pivot_wider(names_from = species, values_from = n, values_fill = list(n = 0))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 24 × 52
   region      `Amercian Eel` `American Eel` `Blue Pike` Bowfin Buffalo Bullhead
   <chr>                <int>          <int>       <int>  <int>   <int>    <int>
 1 Canada (ON…            130              9         187    107      47       20
 2 Georgian B…              0              0           0      0      29        0
 3 Green Bay …              0              0           0      0       1        0
 4 Green Bay …              0              0           0      0       1        0
 5 Huron Prop…              0              0           0      0      29        0
 6 Illinois (…              0              0           0      0       1        0
 7 Indiana (I…              0              0           0      0       1        0
 8 MI State T…              0              0           0      0       1        0
 9 Mich. Prop…              0              0           0      0       1        0
10 Mich. Prop…              0              0           0      0       1        0
# ℹ 14 more rows
# ℹ 45 more variables: Bullheads <int>, Burbot <int>, Carp <int>,
#   `Channel Catfish` <int>, `Channel Catfish and Bullheads` <int>,
#   `Channel catfish` <int>, Chubs <int>, Cisco <int>, `Cisco and Chubs` <int>,
#   `Cisco and chubs` <int>, Crappie <int>, Drum <int>,
#   `Freshwater Drum` <int>, `Gizzard Shad` <int>, Goldfish <int>,
#   Herring <int>, `Lake Sturgeon` <int>, `Lake Trout` <int>, …
```


:::
:::



This is a format table where rows correspond to regions, and columns correspond to species of fish. The value inside the table is the number of recorded occurrences of the species for that region. If the species does not occur in that region, the value is 0. This summary will allow to analyze how fish species are distributed across different regions.

## Visualizing Species Distribution



::: {.cell}

```{.r .cell-code}
# Select top 5 species by total count from the test_data
top_species <- test_data %>%
  group_by(species) %>%
  summarise(total_count = sum(values, na.rm = TRUE)) %>%  # Assuming 'values' represents species count
  top_n(5, total_count)  # Selecting the top 5 species

# Filter the data for only the top species
filtered_data <- test_data %>%
  filter(species %in% top_species$species)

# Define regions to focus on (adjust this list based on your preference)
regions_of_interest <- c("Michigan (MI)", "New York (NY)", "Ohio (OH)", "Canada (ONT)", "Wisconsin (WI)")

# Filter the data for only the regions of interest
filtered_data <- filtered_data %>%
  filter(region %in% regions_of_interest)

# Create the bar plot for the top 5 species by region
ggplot(filtered_data, aes(x = species, y = values, fill = region)) +  # Use 'values' for species count
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Top 5 Species Distribution by Region",
       x = "Fish Species",
       y = "Count of Species") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  theme_minimal()
```

::: {.cell-output .cell-output-stderr}

```
Warning: Removed 323 rows containing missing values or values outside the scale range
(`geom_bar()`).
```


:::

::: {.cell-output-display}
![](Great-Lake-Fish-Analysis_files/figure-html/unnamed-chunk-5-1.png){width=672}
:::
:::
