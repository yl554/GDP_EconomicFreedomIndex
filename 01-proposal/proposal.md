PROJECT TITLE
================
NAME HERE
TODAY’S DATE

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(readxl)
```

``` r
setwd("/cloud/project")
economic_data <- read_excel("02-data/economic_data.xlsx",
col_types = c("text", "text", "text",
"numeric", "numeric", "numeric",
"numeric", "numeric", "numeric",
"numeric", "numeric", "numeric",
"numeric", "numeric", "numeric"))
```

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M49 / R49C13: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in D80 / R80C4: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G80 / R80C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M89 / R89C13: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in E90 / R90C5: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in F90 / R90C6: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G90 / R90C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in J90 / R90C10: got '$40.0 (2015 est.)'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in L90 / R90C12: got '$1,700 (2015 est.)'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in N90 / R90C14: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in O90 / R90C15: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M92 / R92C13: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in D100 / R100C4: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G100 / R100C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in D101 / R101C4: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G101 / R101C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in H101 / R101C8: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in I101 / R101C9: got '38,000 ppl.'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in J101 / R101C10: got '$6.1 CHF (2014 )'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in K101 / R101C11: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in L101 / R101C12: got '$139,100 (2009 est.)'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M101 / R101C13: got '2.1 (2016)'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in N101 / R101C14: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in O101 / R101C15: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M115 / R115C13: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in M148 / R148C13: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in D154 / R154C4: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in E154 / R154C5: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in F154 / R154C6: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G154 / R154C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in H154 / R154C8: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in L154 / R154C12: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in N154 / R154C14: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in O154 / R154C15: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G162 / R162C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in H162 / R162C8: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in J162 / R162C10: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in K162 / R162C11: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in L162 / R162C12: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in N162 / R162C14: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in O162 / R162C15: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in E184 / R184C5: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in F184 / R184C6: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in G184 / R184C7: got 'N/A'

    ## Warning in read_fun(path = enc2native(normalizePath(path)), sheet_i =
    ## sheet, : Expecting numeric in H184 / R184C8: got 'N/A'

``` r
happiness_data <- read_excel("02-data/happiness_data.xlsx")

country_data <- full_join(economic_data, happiness_data, by = "Country")
country_data
```

    ## # A tibble: 193 x 16
    ##    Country Region GovInterference TariffRate IncomeTaxRate CorporateTaxRate
    ##    <chr>   <chr>  <chr>                <dbl>         <dbl>            <dbl>
    ##  1 Afghan… Asia-… Repressive             7              20               20
    ##  2 Albania Europe Moderate               1.1            23               15
    ##  3 Algeria Middl… Extensive              8.8            35               23
    ##  4 Angola  Sub-S… Extensive              9.4            17               30
    ##  5 Argent… Ameri… Moderate               7.5            35               30
    ##  6 Armenia Europe Moderate               2.1            26               20
    ##  7 Austra… Asia-… Limited                1.2            45               30
    ##  8 Austria Europe Moderate               2              50               25
    ##  9 Azerba… Asia-… Moderate               5.2            25               20
    ## 10 Bahamas Ameri… Moderate              18.6             0                0
    ## # … with 183 more rows, and 10 more variables: TaxBurden <dbl>,
    ## #   GovSpending <dbl>, Population <dbl>, GDP <dbl>, GDPGrowth <dbl>,
    ## #   GDPperCap <dbl>, Unemployment <dbl>, Inflation <dbl>,
    ## #   PublicDebt <dbl>, Happiness_Score <dbl>

## Section 1. Introduction

## Section 2. Analysis plan

``` r
ggplot(mapping = aes(x = GovInterference), data = economic_data) +
geom_bar(fill = "cornflowerblue") +
labs(title = "Bar Graph of Government Inteference in Economy", x  = "Levels of Government Interference", y = "Frequency")
```

![](proposal_files/figure-gfm/EDA-1.png)<!-- -->

``` r
ggplot(mapping = aes(x = TariffRate), data = economic_data) +
geom_histogram(fill = "cornflowerblue") +
labs(title = "Histogram of Tariff Rate", x  = "Tariff Rate", y = "Frequency")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 4 rows containing non-finite values (stat_bin).

![](proposal_files/figure-gfm/EDA-2.png)<!-- -->

``` r
ggplot(mapping = aes(x = IncomeTaxRate), data = economic_data) +
geom_histogram(fill = "cornflowerblue") +
labs(title = "Histogram of Income Tax Rate", x  = "Income Tax Rate", y = "Frequency")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](proposal_files/figure-gfm/EDA-3.png)<!-- -->

``` r
ggplot(mapping = aes(x = CorporateTaxRate), data = economic_data) +
geom_histogram(fill = "cornflowerblue") +
labs(title = "Histogram of Corporate Tax Rate", x  = "Corporate Tax Rate", y = "Frequency")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](proposal_files/figure-gfm/EDA-4.png)<!-- -->

## Section 3. Regression Analysis Plan

## Section 4. References

## The Data
