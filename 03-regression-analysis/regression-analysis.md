What Makes a Strong GDP
================
Rtists
11/14/2019

    ## ── Attaching packages ─────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

    ## 
    ## Attaching package: 'skimr'

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

Your regression analysis results go here. At a minimum, the regression
analysis should include the following:

  - Statement of the research question and modeling obejctive
    (prediction, inference, etc.)
  - Description of the response variable
  - Updated exploratory data analysis, incorporating any feedback from
    the proposal
  - Explanation of the modeling process and why you chose those metohds,
    incorporating any feedback from the proposal
  - Output of the final model
  - Discussion of the assumptions for the final model
  - Interpretations / interesting findings from the model coefficients
  - Additional work of other models or analylsis not included in the
    final model.

*Use proper headings as needed.*

### Question and Objective

### Response Variable

### Exploratory Data Analysis

    ## # A tibble: 6 x 2
    ## # Groups:   cat_inflation [6]
    ##   cat_inflation        n
    ##   <chr>            <int>
    ## 1 Dangerously High    40
    ## 2 Deflation            5
    ## 3 Healthy             65
    ## 4 High                43
    ## 5 Low                 28
    ## 6 <NA>                 4

    ## Skim summary statistics
    ##  n obs: 185 
    ##  n variables: 16 
    ## 
    ## ── Variable type:character ─────────────────────────────────────────────────────────────────────────────
    ##         variable missing complete   n min max empty n_unique
    ##    cat_inflation       4      181 185   3  16     0        5
    ##          Country       0      185 185   4  32     0      185
    ##  GovInterference       0      185 185   5  10     0        5
    ##           Region       0      185 185   6  28     0        5
    ## 
    ## ── Variable type:numeric ───────────────────────────────────────────────────────────────────────────────
    ##          variable missing complete   n     mean       sd    p0     p25
    ##  CorporateTaxRate       3      182 185    23.89     8.88   0     20   
    ##               GDP       3      182 185   698.04  2427.86   0.2   26.2 
    ##         GDPGrowth       2      183 185     3.47     5.85 -14      1.8 
    ##         GDPperCap       4      181 185 20854.44 22381.72 677   4586   
    ##       GovSpending       4      181 185    33.87    15.52  10.6   24.5 
    ##     IncomeTaxRate       3      182 185    28.23    13.4    0     20   
    ##         Inflation       4      181 185    10.61    80.73  -0.9    1.3 
    ##        Population       1      184 185    40.37   145.52   0.1    2.77
    ##        PublicDebt       4      181 185    56.32    34.2    0     34.9 
    ##        TariffRate       4      181 185     5.96     5.54   0      2   
    ##         TaxBurden       7      178 185    22.19    10.17   1.6   14.12
    ##      Unemployment       6      179 185     7.39     5.68   0.1    3.75
    ##       p50      p75     p100     hist
    ##     25       30        50   ▁▂▂▇▆▂▁▁
    ##     83.75   413.47  23159.1 ▇▁▁▁▁▁▁▁
    ##      3.2      4.7      70.8 ▁▇▁▁▁▁▁▁
    ##  12724    29521    124529   ▇▃▂▁▁▁▁▁
    ##     32.3     40.3     139.2 ▅▇▂▁▁▁▁▁
    ##     30       35        60   ▂▆▃▇▇▅▂▁
    ##      2.7      5.3    1087.5 ▇▁▁▁▁▁▁▁
    ##      9.15    29.62   1390.1 ▇▁▁▁▁▁▁▁
    ##     49.4     69.9     236.4 ▃▇▃▂▁▁▁▁
    ##      4.3      8.7      50   ▇▃▁▁▁▁▁▁
    ##     20.75    30.02     47   ▂▅▇▆▅▅▂▂
    ##      5.7      9.35     27.3 ▅▇▅▂▁▁▁▁

### Methods & Modelling

### Final Model

#### Create the model

#### Model Diagnostics

(influential points, leverage points)

### Discusson of Assumptions

### Interpretation and Other Interesting Findings

### Additional Information
