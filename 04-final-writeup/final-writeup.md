What makes a country economically great?
================
RTists
12/7/19

## Section 1: Introduction (includes introduction and exploratory data analysis)

GDP is a measure of the total market value of all goods and services
produced within a country over a period of time. As emerging markets
continue to industrialize, the question of how one achieves a larger GDP
is becomes critical to policy selection; failure to design policies that
encourage GDP growth can cause widespread suffering for a country’s
population. Consider the case of Venezuela, where GDP-friendly policies
were shunned and massive unrest appeared as a nearly direct consequence.

The use of multiple linear regression in the study of GDP and GDP growth
is not novel. The approach has been widely employed by economists. For
instance, Anghelache et. al has employed MLR in analyzing the influence
of final consumption and gross investment on Romania’s GDP over time.
Urrutia et. al did the same with Philippines’s real GDP. (Refer to
Additional Work section for citations)

Our research question is this: what variables explain the size of an
economy’s GDP and to what is the relative explanatory value of each
variable. The predictor variables used in our analysis are used in the
calculation of the annual Economic Freedom Index. These predictors are
not traditionally used in the multiple linear regression of GDP and we
are interested to know the composite strength of these particular
predictor variables.

### Exploratory Data Analysis

    ## # A tibble: 12 x 1
    ##    Country      
    ##    <chr>        
    ##  1 Dominica     
    ##  2 Iraq         
    ##  3 Kiribati     
    ##  4 Korea, North 
    ##  5 Kosovo       
    ##  6 Libya        
    ##  7 Liechtenstein
    ##  8 Micronesia   
    ##  9 Seychelles   
    ## 10 Somalia      
    ## 11 Syria        
    ## 12 Yemen

#### Response Variable

The response variable is GDP.

![](final-writeup_files/figure-gfm/gdp-vis-1.png)<!-- -->

From our EDA, we decided to log-transform the response variable since
the histogram of GDP (even without outlier) shows a unimodal and
right-skewed distribution. The logGDP distribution on the other hand is
unimodal and symmetric. The mean logGDP is 4.61 and the standard
deviation of its distribution is 2.08. LogGDP’s summary sstatistics are
printed below.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##  variable missing complete   n mean   sd    p0  p25  p50  p75  p100
    ##    logGDP       0      173 173 4.61 2.08 -0.51 3.34 4.49 6.09 10.05
    ##      hist
    ##  ▁▃▅▇▆▅▂▁

#### Univariate Analysis of Predictor Variables

The quantitative predictor variables are TariffRate, IncomeTaxRate,
CorporateTaxRate, TaxBurden, GovSpending, Population, GDPGrowth,
Unemployment, and PublicDebt. Out of these, we will discuss two
predictor variables that need to be modified prior to building the
model: Population and Inflation. (Discussion of the rest of the
variables will be in Additional Work). The distribution of each of the
quantitative predictor variables is shown below.

![](final-writeup_files/figure-gfm/EDA1-1.png)<!-- -->

From the histogram of population without outliers above, the
distribution of population is unimodal and right-skewed. The median is
9.15 and the IQR is 26.85. Based on the subsequent pairs plot (in
multivariate EDA), population has a logarithmic relationship with logGDP
and thus we will apply a logarithmic transform to population below. The
summary statistics of this transformed variable are also
printed.

![](final-writeup_files/figure-gfm/Population%20without%20Outlier-1.png)<!-- -->

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##  variable missing complete   n mean  sd   p0  p25  p50  p75 p100     hist
    ##    logpop       0      173 173 2.16 1.8 -2.3 1.06 2.25 3.45 7.24 ▁▃▃▇▆▃▁▁

The categorical predictor variables are Region, GovInterference, and
Inflation. The distribution of these variables is printed below.

![](final-writeup_files/figure-gfm/EDA2-1.png)<!-- -->

`Inflation` represents the change in prices of goods and services in a
year in the country. The distribution of inflation rate is generally
unimodal and right skewed. The median inflation rate is 2.7% and the IQR
is 4%. These summary statistics are printed below.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##   variable missing complete   n  mean    sd   p0 p25 p50 p75   p100
    ##  Inflation       0      173 173 10.87 82.56 -0.9 1.3 2.8 5.5 1087.5
    ##      hist
    ##  ▇▁▁▁▁▁▁▁

Because we hypothesize that inflation interacts with other variables and
can determine whether the effect of other variables (that have to do
with currency stability) is positive or negative, we have recoded it
into a categorical variable that will make interpretation of these
interaction terms easier.

The categories are as follows: Inflation \> 6% = “Dangerously High”
Inflation 3-4% = “High” Inflation 1-3% = “Healthy” Inflation \<1% =
“Low”

This categorization is informed by extant economic literature (Refer to
Citations)

    ## # A tibble: 4 x 2
    ## # Groups:   cat_inflation [4]
    ##   cat_inflation        n
    ##   <chr>            <int>
    ## 1 Dangerously High    39
    ## 2 Healthy             62
    ## 3 High                42
    ## 4 Low                 30

![](final-writeup_files/figure-gfm/inflation-eda-1.png)<!-- -->

### Multivariate Exploration

![](final-writeup_files/figure-gfm/Scatterplot%20logGDP-1.png)<!-- -->![](final-writeup_files/figure-gfm/Scatterplot%20logGDP-2.png)<!-- -->![](final-writeup_files/figure-gfm/Scatterplot%20logGDP-3.png)<!-- -->

From the scatterplots, we note that logGDP against population
scatterplot shows a curved distribution which resembles a logarithimic
function. This is why we transformed population into the logpop
variable, which has a linear relationship with GDP.

There is some multicollinearity issues between predictor variables. The
relationship between GovSpending and TaxBurden appears very strongly if
not almost linear. Other variables suspect of multicollinearity problems
include tariff rate and unemployment as well as tariff rate and
unemployment.

A similar analysis is conducted between the response and the categorical
variables using side-by-side
boxplots.

![](final-writeup_files/figure-gfm/Boxplots%20for%20response%20vs%20categorical-1.png)<!-- -->

In each case, it seems apparent that across the different levels of each
of our three categorical variables, the mean logGDP values tend to be
different from one another. This suggests that these variables, or at
least some of their levels, may end up in our final model.

We also meancentered the following variables for the modelling:
TaxBurden and
GovSpending.

## Section 2: Regression Analysis (includes the final model and discussion of assumptions)

Our approach is this: running model selection using all three selection
criteria - AIC, BIC, and adjusted R^2 - and comparing all three models
to decide on the final model.

Model selected using AIC: logGDP ~ TariffRate + logpop + GDPGrowth +
TaxBurdenCent + GovSpendingCent + cat\_inflationDangerouslyHigh +
cat\_inflationHigh + cat\_inflationLow + RegionAsia-Pacific +
RegionEurope + RegionMiddleEastandNorthAfrica + RegionSub-SaharanAfrica
+ GovInterferenceLimited + GovInterferenceExtensive +
GovInterferenceRepressive

Model selected using BIC: logGDP ~ TariffRate + logpop + GDPGrowth +
cat\_inflationDangerouslyHigh + cat\_inflationHigh +
GovInterferenceExtensive + RegionSub-Saharan Africa

Model selected using adj R^2: logGDP ~ TariffRate + logpop + GDPGrowth +
TaxBurdenCent + cat\_inflationDangerously High + cat\_inflationHigh +
GovInterferenceLimited + GovInterferenceModerate + RegionSub-Saharan
Africa

Since all three selection criteria included TariffRate + logpop +
GDPGrowth + GovInterference, we will include these four predictors into
our final model. Cat\_inflation and Region will be included in the final
model, although some of their levels are not selected. Briefly, their
explanatory value outweighs the noise they add to the model. This is
discussed in detail in Additional Work. GovSpendingCent is included only
in the AIC equation while TaxBurdenCent is included in both the AIC and
adjR2 equations. Conducting nested F test, we choose to include
TaxBudenCent and not GovSpendingCent in the final model. We also
conducted Nested F test for interaction variables. We found significant
interaction between GovInterference and TariffRate as well as Region and
TariffRate.

(Refer to Additional Work for detailed discussion on Model Section
process)

### Final Model

Final selection of predictor and interaction variables: TariffRate +
logpop + GDPGrowth + TaxBurdenCent + cat\_inflation + GovInterference +
Region + GovInterference x TariffRate + Region x TariffRate

<table>

<thead>

<tr>

<th style="text-align:left;">

term

</th>

<th style="text-align:right;">

estimate

</th>

<th style="text-align:right;">

std.error

</th>

<th style="text-align:right;">

statistic

</th>

<th style="text-align:right;">

p.value

</th>

<th style="text-align:right;">

conf.low

</th>

<th style="text-align:right;">

conf.high

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

(Intercept)

</td>

<td style="text-align:right;">

3.117

</td>

<td style="text-align:right;">

0.268

</td>

<td style="text-align:right;">

11.616

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

2.587

</td>

<td style="text-align:right;">

3.648

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate

</td>

<td style="text-align:right;">

\-0.009

</td>

<td style="text-align:right;">

0.032

</td>

<td style="text-align:right;">

\-0.266

</td>

<td style="text-align:right;">

0.790

</td>

<td style="text-align:right;">

\-0.072

</td>

<td style="text-align:right;">

0.055

</td>

</tr>

<tr>

<td style="text-align:left;">

logpop

</td>

<td style="text-align:right;">

1.006

</td>

<td style="text-align:right;">

0.032

</td>

<td style="text-align:right;">

31.600

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.943

</td>

<td style="text-align:right;">

1.069

</td>

</tr>

<tr>

<td style="text-align:left;">

GDPGrowth

</td>

<td style="text-align:right;">

\-0.057

</td>

<td style="text-align:right;">

0.022

</td>

<td style="text-align:right;">

\-2.596

</td>

<td style="text-align:right;">

0.010

</td>

<td style="text-align:right;">

\-0.101

</td>

<td style="text-align:right;">

\-0.014

</td>

</tr>

<tr>

<td style="text-align:left;">

TaxBurdenCent

</td>

<td style="text-align:right;">

0.020

</td>

<td style="text-align:right;">

0.008

</td>

<td style="text-align:right;">

2.523

</td>

<td style="text-align:right;">

0.013

</td>

<td style="text-align:right;">

0.004

</td>

<td style="text-align:right;">

0.035

</td>

</tr>

<tr>

<td style="text-align:left;">

cat\_inflationDangerously High

</td>

<td style="text-align:right;">

\-0.374

</td>

<td style="text-align:right;">

0.163

</td>

<td style="text-align:right;">

\-2.287

</td>

<td style="text-align:right;">

0.024

</td>

<td style="text-align:right;">

\-0.697

</td>

<td style="text-align:right;">

\-0.051

</td>

</tr>

<tr>

<td style="text-align:left;">

cat\_inflationHigh

</td>

<td style="text-align:right;">

\-0.402

</td>

<td style="text-align:right;">

0.156

</td>

<td style="text-align:right;">

\-2.584

</td>

<td style="text-align:right;">

0.011

</td>

<td style="text-align:right;">

\-0.709

</td>

<td style="text-align:right;">

\-0.095

</td>

</tr>

<tr>

<td style="text-align:left;">

cat\_inflationLow

</td>

<td style="text-align:right;">

0.022

</td>

<td style="text-align:right;">

0.163

</td>

<td style="text-align:right;">

0.137

</td>

<td style="text-align:right;">

0.891

</td>

<td style="text-align:right;">

\-0.300

</td>

<td style="text-align:right;">

0.344

</td>

</tr>

<tr>

<td style="text-align:left;">

GovInterferenceExtensive

</td>

<td style="text-align:right;">

\-1.045

</td>

<td style="text-align:right;">

0.240

</td>

<td style="text-align:right;">

\-4.345

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

\-1.520

</td>

<td style="text-align:right;">

\-0.570

</td>

</tr>

<tr>

<td style="text-align:left;">

RegionAsia-Pacific

</td>

<td style="text-align:right;">

1.117

</td>

<td style="text-align:right;">

0.299

</td>

<td style="text-align:right;">

3.731

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

0.526

</td>

<td style="text-align:right;">

1.708

</td>

</tr>

<tr>

<td style="text-align:left;">

RegionEurope

</td>

<td style="text-align:right;">

0.537

</td>

<td style="text-align:right;">

0.378

</td>

<td style="text-align:right;">

1.420

</td>

<td style="text-align:right;">

0.158

</td>

<td style="text-align:right;">

\-0.210

</td>

<td style="text-align:right;">

1.283

</td>

</tr>

<tr>

<td style="text-align:left;">

RegionMiddle East and North Africa

</td>

<td style="text-align:right;">

0.969

</td>

<td style="text-align:right;">

0.433

</td>

<td style="text-align:right;">

2.237

</td>

<td style="text-align:right;">

0.027

</td>

<td style="text-align:right;">

0.113

</td>

<td style="text-align:right;">

1.824

</td>

</tr>

<tr>

<td style="text-align:left;">

RegionSub-Saharan Africa

</td>

<td style="text-align:right;">

\-0.655

</td>

<td style="text-align:right;">

0.348

</td>

<td style="text-align:right;">

\-1.883

</td>

<td style="text-align:right;">

0.062

</td>

<td style="text-align:right;">

\-1.343

</td>

<td style="text-align:right;">

0.032

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate:GovInterferenceExtensive

</td>

<td style="text-align:right;">

0.080

</td>

<td style="text-align:right;">

0.031

</td>

<td style="text-align:right;">

2.564

</td>

<td style="text-align:right;">

0.011

</td>

<td style="text-align:right;">

0.018

</td>

<td style="text-align:right;">

0.142

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate:RegionAsia-Pacific

</td>

<td style="text-align:right;">

\-0.180

</td>

<td style="text-align:right;">

0.046

</td>

<td style="text-align:right;">

\-3.904

</td>

<td style="text-align:right;">

0.000

</td>

<td style="text-align:right;">

\-0.271

</td>

<td style="text-align:right;">

\-0.089

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate:RegionEurope

</td>

<td style="text-align:right;">

\-0.068

</td>

<td style="text-align:right;">

0.122

</td>

<td style="text-align:right;">

\-0.554

</td>

<td style="text-align:right;">

0.580

</td>

<td style="text-align:right;">

\-0.310

</td>

<td style="text-align:right;">

0.174

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate:RegionMiddle East and North Africa

</td>

<td style="text-align:right;">

\-0.045

</td>

<td style="text-align:right;">

0.067

</td>

<td style="text-align:right;">

\-0.676

</td>

<td style="text-align:right;">

0.500

</td>

<td style="text-align:right;">

\-0.178

</td>

<td style="text-align:right;">

0.087

</td>

</tr>

<tr>

<td style="text-align:left;">

TariffRate:RegionSub-Saharan Africa

</td>

<td style="text-align:right;">

\-0.058

</td>

<td style="text-align:right;">

0.044

</td>

<td style="text-align:right;">

\-1.322

</td>

<td style="text-align:right;">

0.188

</td>

<td style="text-align:right;">

\-0.146

</td>

<td style="text-align:right;">

0.029

</td>

</tr>

</tbody>

</table>

GDP = 3.117 - 0.009(TariffRate) + 1.006(logpop) - 0.057(GDPGrowth) +
0.020(TaxBurdenCent) -0.374(cat\_inflationDangerously High) -
0.402(cat\_inflationHigh) + 0.022(cat\_inflationLow) -
1.045(GovInterferenceExtensive) + 1.117(RegionAsia-Pacific) +
0.537(RegionEurope) +  
0.969(RegionMiddle East and North Africa) - 0.655(RegionSub-Saharan
Africa) + 0.080(TariffRate x GovInterferenceExtensive) -
0.180(TariffRate x RegionAsia-Pacific) - 0.068(TariffRate x
RegionEurope) - 0.045(TariffRate x RegionMiddle East and North Africa) -
0.058(TariffRate:RegionSub-Saharan Africa)

#### Model Diagnostics

    ## # A tibble: 1 x 11
    ##   r.squared adj.r.squared sigma statistic  p.value    df logLik   AIC   BIC
    ##       <dbl>         <dbl> <dbl>     <dbl>    <dbl> <int>  <dbl> <dbl> <dbl>
    ## 1     0.904         0.893 0.680      85.5 1.28e-69    18  -169.  377.  437.
    ## # … with 2 more variables: deviance <dbl>, df.residual <int>

The final R^2 value for the model is .842, implying that approximately
84.2% of the variation in GDP can be well-explained by a linear
relationship with our transformed predictors.

We examined the four model assumptions: linearity, constant variance,
normality, and independence. None of the model assumptions Furthermore,
we examined leverage, cook’s distance, and standardized residual to
check for outliers and assess their influence on our model. VIF is also
calculated to ensure no multi-collinearity between predictor variables.

(Refer to Additional Work for detailed discussion on Model Assumptions)

#### Independence

![](final-writeup_files/figure-gfm/independence-1.png)<!-- --> One may
suspect non-independence to be violated due to spatial correlation. We
included Region as a predictor variable to negate this. Moreover, the
residuals did not show significant cluster effect based on Region. We
also do not expect that the recording of GDP in one country to affect
the recording of GDP in another. Thus, the observations are deduced to
be
independent.

#### Constant Variance & Linearity

![](final-writeup_files/figure-gfm/residual%20vs.%20predicted-1.png)<!-- -->![](final-writeup_files/figure-gfm/residual%20vs.%20predicted-2.png)<!-- -->

From the residual plots above, we conclude the constant variance and
linearity assumptions are satisified. Residuals are all randomly
scattered with no observable pattern, for both quantitative and
categorical
predictors.

#### Normality Check

![](final-writeup_files/figure-gfm/residual%20normality%20check-1.png)<!-- -->

The normality assumption is satisfied since the distribution of
residuals appears to be unimodal and approximately normal. Also, the
normal QQ plot of points fit the theoretical line very well.

#### Leverage, Standardized Residual, Cook’s Distance

    ## Observations: 5
    ## Variables: 16
    ## $ logGDP          <dbl> 4.242765, 3.583519, 6.450312, 5.248602, 6.824591
    ## $ TariffRate      <dbl> 7.0, 1.1, 8.8, 9.4, 7.5
    ## $ logpop          <dbl> 3.569533, 1.064711, 3.725693, 3.339322, 3.786460
    ## $ GDPGrowth       <dbl> 2.5, 3.9, 2.0, 0.7, 2.9
    ## $ TaxBurdenCent   <dbl> -17.193642, 2.706358, 2.306358, -1.593642, 8.606…
    ## $ cat_inflation   <fct> High, Healthy, High, Dangerously High, Dangerous…
    ## $ GovInterference <fct> Extensive, Moderate, Extensive, Extensive, Moder…
    ## $ Region          <chr> "Asia-Pacific", "Europe", "Middle East and North…
    ## $ .fitted         <dbl> 5.139058, 4.471403, 6.551641, 4.457461, 6.492631
    ## $ .se.fit         <dbl> 0.1886207, 0.1868457, 0.2934447, 0.1522129, 0.20…
    ## $ .resid          <dbl> -0.8962932, -0.8878845, -0.1013290, 0.7911412, 0…
    ## $ .hat            <dbl> 0.07683060, 0.07539135, 0.18595507, 0.05003320, …
    ## $ .sigma          <dbl> 0.6785456, 0.6786297, 0.6826367, 0.6795560, 0.68…
    ## $ .cooksd         <dbl> 0.0086886948, 0.0083406800, 0.0003456703, 0.0041…
    ## $ .std.resid      <dbl> -1.3708410, -1.3569230, -0.1650393, 1.1928271, 0…
    ## $ obs_num         <int> 1, 2, 3, 4, 5

![](final-writeup_files/figure-gfm/leverage-1.png)<!-- -->

    ## # A tibble: 59 x 16
    ##    logGDP TariffRate logpop GDPGrowth TaxBurdenCent cat_inflation
    ##     <dbl>      <dbl>  <dbl>     <dbl>         <dbl> <fct>        
    ##  1   6.45        8.8  3.73        2            2.31 High         
    ##  2   2.45       18.6 -0.916       1.3         -5.89 Healthy      
    ##  3   4.25        3.1  0.405       3.2        -16.6  Healthy      
    ##  4   6.53       10.7  5.09        7.1        -13.4  High         
    ##  5   1.65       14.2 -1.20        0.9         11.5  High         
    ##  6   5.19        1.8  2.25        2.4          1.61 Dangerously …
    ##  7   3.66        0.6  0.788       2.2          2.71 High         
    ##  8   3.51        0.5 -0.916       0.5          2.01 Low          
    ##  9   4.16        9.8  2.77        6.9         -7.19 Healthy      
    ## 10   4.49       15.8  3.19        3.2         -6.59 Low          
    ## # … with 49 more rows, and 10 more variables: GovInterference <fct>,
    ## #   Region <chr>, .fitted <dbl>, .se.fit <dbl>, .resid <dbl>, .hat <dbl>,
    ## #   .sigma <dbl>, .cooksd <dbl>, .std.resid <dbl>, obs_num <int>

    ## # A tibble: 59 x 2
    ##    obs_num  .cooksd
    ##      <int>    <dbl>
    ##  1       3 0.000346
    ##  2      10 0.0498  
    ##  3      11 0.00381 
    ##  4      12 0.00377 
    ##  5      13 0.000301
    ##  6      14 0.0122  
    ##  7      21 0.0218  
    ##  8      23 0.00132 
    ##  9      28 0.00498 
    ## 10      29 0.000855
    ## # … with 49 more rows

There are 8 points with high leverage. This becauseome observations have
significantly high population, others have lower TaxBurdenCent, while
the rest have lower corporate taxrate than most
points.

![](final-writeup_files/figure-gfm/standardized%20residuals-1.png)<!-- -->

    ## [1] 8

There are 8 observations with std. residuals considered large. Assuming
a N(0,1) distribution, 8/173 = 4.62% of the observations have std.
residuals \> 2.

![](final-writeup_files/figure-gfm/cook%20distace-1.png)<!-- -->

Even though there are 8 points with high leverage and high standardized
residues, none of them has a cook distance \> 1. Therefore, we conclude
they do not have significant influence on model coefficients and include
them.

#### Multi-Collinearity in Model

    ## # A tibble: 17 x 2
    ##    names                                             x
    ##    <chr>                                         <dbl>
    ##  1 TariffRate                                     7.44
    ##  2 logpop                                         1.21
    ##  3 GDPGrowth                                      1.37
    ##  4 TaxBurdenCent                                  2.39
    ##  5 cat_inflationDangerously High                  1.74
    ##  6 cat_inflationHigh                              1.66
    ##  7 cat_inflationLow                               1.42
    ##  8 GovInterferenceExtensive                       5.12
    ##  9 RegionAsia-Pacific                             5.95
    ## 10 RegionEurope                                   9.97
    ## 11 RegionMiddle East and North Africa             5.21
    ## 12 RegionSub-Saharan Africa                       8.71
    ## 13 TariffRate:GovInterferenceExtensive            7.89
    ## 14 TariffRate:RegionAsia-Pacific                  6.05
    ## 15 TariffRate:RegionEurope                        6.16
    ## 16 TariffRate:RegionMiddle East and North Africa  5.04
    ## 17 TariffRate:RegionSub-Saharan Africa           15.8

There is only variable that exceeds the threshold of 10:
TariffRate:RegionSub-Saharan Africa. We choose to keep the interaction
variables between TariffRate and all the levels of Region for the
following reasons. One, no other value is close to 15.758. Two, its high
multicollinearity is not entirely surprising considering that the region
has considerably higher tariff rates compared to all other regions.
Three, VIF values for the interaction between TariffRate and other
levels of Region are not high and we want to retain their explanatory
value. (Refer to Additional Work for detailed discussion)

### Coefficient Interpretations

For detailed interpretations of each coefficient, please see additional
work below. A brief sample of an interpretation of a coefficient is
below.

For countries who are in the Asia Pacific, as their TariffRate increases
by 1%, the median GDP of the country is expected to be multiplied by a
factor of 0.8352702, holding all other variables constant.

As the tax burden increases by one percentage point, the expected median
GDP of the country is multiplied by a factor of 0.9801987, holding all
other variables constant. As the tax burden increases, we can expect the
GDP of the country to be lower.

### Interesting Findings

Through the investigation we came across several interesting findings
that we will describe below.

The first and most notable is that none of our interaction effect terms
involving inflation ended up in our final model as they did not have a
significant effect on the response. We tried five different interaction
effects with inflation: The effects between inflation and GDP growth,
inflation, inflation and the log of the population, and inflation and
the tariff rate. We hypothesized that the effect of these quantitative
predictors may depend on the rate of inflation as this represents the
stability of the currency which drives the country’s economy, but this
ended up not being the case.

Furthermore, we noticed a negative coefficient for GDP growth - while
it’s initially surprising, countries with very high GDP growth tend to
be developing countries who have lots of room for growth. Thus, while
growth is high, they have less overall development than other countries.

One major set of indicators/variables that did not make it into our
final model were the tax rates: both corporate and income taxes. This is
peculiar because there is almost a constant discussion by political
parties and governments about how raising taxes (for those on the left)
or lowering taxes (for those on the right) is necessary to stimulate the
economy and/or boost production of goods and services. Our model on the
other hand suggests that a relationship may not be as clear as
traditional economists would claim.

![](final-writeup_files/figure-gfm/tax-1.png)<!-- -->

Indeed, according to the above plot, there doesn’t seem to be any
visually evident relationship between GDP and either corporate or income
tax rate. Instead, it seems that a tax rate that’s modest but not too
high or too low is perhaps indicative of a high GDP.

## Section 3: Discussion and Limitations

In our final model, we included the tariff rate, population, GDP growth,
tax burden, inflation, region, and the interaction between region and
tariff rate. The goal of the model isn’t prediction; instead, it allows
us to determine the effect of a particular indicator or variable on the
expected GDP, which we have described at length in the interpretation of
each coefficient below. Our model has allowed us to say, for example,
that being located in the Asia-Pacific region means the expected GDP of
the country is higher than being located in Europe.

While this model does provide many insights into the role of various
economic indicators in GDP, there are various limitations that can be
resolved in future investigations. The most obvious limitation is that
we decided, for the purposes of simplicity, to remove the 12
observations in our dataset that didn’t have complete data reported.
While this represents only a small subset of the total observations,
this certainly does decrease the validity of our data. This is
especially true because the countries that we removed for this reason,
such as North Korea, Iraq, Somalia, etc. have certain similar
characteristics such as high government interference, high tariff rates,
etc. that effectively mean we are removing an entire “group” of
countries from our dataset. However, it is likely that were we to have
data on these countries, the figures would not be accurately
reported/would be inflated by the countries’ governments.

Our overall methods for building and verifying the model were sound. By
examining all three indicators of the model, AIC, BIC, and Adjusted R^2,
we were able to choose a model that best fit our needs. Additionally, we
conducted a full diagnosis of the model including looking at leverage
points and whether these points exceeded a variety of thresholds such as
Cook’s Distance and VIF. One potential issue with our methods were that
our threshold for multicollinearity may have been a little lenient for
the types of variables we were looking at. It is definitely true that
there are relationships between the economic indicators that we looked
at: for example, corporate tax rate may very well be a function of
income tax rate in many countries. Or, for example, the rate of GDP
growth is related to the GDP itself. Because of this, we had a VIF value
for certain observations that was coming up to a value of 9, which while
not exceeding our threshold of 10, is still something to examine
further. If we were to restart or go further with our investigation, we
would resolve many of the questions of multicollinearity as well as
somewhat strange outcomes with the variables our model chose by adding
data for each country pertaining to non-economic factors. By adding
these variables, a potential final model would not only likely have more
predictive power but would have a better chance of avoiding
multicollinearity as instead of including 17 variables relating to
economics we would have fewer.

## Section 4: Conclusion

While we have already discussed the specifics of the model and its
coefficients, looking at it from a broader view has certainly shed some
light on what really contributes to the GDP of country and what perhaps
doesn’t. The first takeaway is that a lot of the GDP of a country is
actually out of the government of the country’s control. This is evident
in that region was an extremely important aspect of our model, as the
regions stood in as their own variables as well as interacting with
other variables. Region is likely important in that it serves as a proxy
for access to natural resources as well as shipping routes and ports.
These factors are out of a government’s control for the most part.
Additionally, the GDP is actually not just the result of the actions of
a government in a vacuum but rather the result of broader, international
efforts and cooperation. The variable TariffRate was crucial in our
model, serving as its own coefficient as well as interacting with other
variables. This is likely because TariffRate serves as a indicator for
not only the average rate of import taxes but also whether or not a
country has trade unions and agreements with other countries and is open
to free trade, which is an extremely important aspect of the GDP of a
country. Finally, our model has shown us that the most important
predictors of GDP involve long-term investments made by the government
to improve the country’s economy, such as population growth, inflation
(currency stability), etc. Short-term fixes such as marginal tax rate
changes or debt are not nearly as effective. This indicates that
short-term losses from actions like cutting inflation have long-term
benefits when looking at GDP. By keeping their currencies stable, their
populations healthy, and their trade agreements fair, governments can
increase their GDP without having to resort to these short-term
measures.

## Section 5: Additional Work

### Detailed interpretation of model coefficients

We have interpreted each of the coefficients in our model below:

For countries who don’t have “extensive” government interference and are
in the Americas, as their TariffRate increases by 1%, the median GDP of
the country is expected to be multiplied by a factor of 0.9910404.

For countries who are in the Asia Pacific, as their TariffRate increases
by 1%, the median GDP of the country is expected to be multiplied by a
factor of 0.8352702, holding all other variables constant.

For countries who do have extensive government interference, as the
TariffRate increases by 1%, the median GDP of the country is expected to
be multiplied by a factor of 1.0832871, holding all other variables
constant.

If the population increases by 1 million, GDP is expected to increase by
$2.7346405

As GDPGrowth increases by one percentage point, the expected median GDP
of the country is multiplied by a factor of 0.9445941, holding all other
variables constant. As GDP growth increases, we see a decrease in GDP.

As the tax burden increases by one percentage point, the expected median
GDP of the country is multiplied by a factor of 0.9801987, holding all
other variables constant. As the tax burden increases, we can expect the
GDP of the country to be lower.

If the country’s inflation rate is listed as “Healthy”, this provides no
further input to the model as “Healthy” is the baseline for our model.
However, if the country’s inflation is listed as low, the expected
median GDP of the country is multiplied by a factor of 1.0222438,
holding all other variables constant. If the country’s inflation is
listed as high, the expected median GDP of the country is multiplied by
a factor of 0.6689807, holding all other variables constant. If the
country’s inflation is listed as dangerously high, the expected median
GDP of the country is multiplied by a factor of 0.6879769, holding all
other variables constant.

If the country has extensive government interference, the expected
median GDP of the country is multiplied by a factor of 0.3516918,
holding all other variables constant.

If the country is in the Americas, this provides no further input to the
model as this is the baseline for our model. However, if the country is
in the Asia Pacific region, the expected median GDP of the country is
multiplied by a factor of 3.0556734, holding all other variables
constant. If the country is in Europe, the expected median GDP of the
country is multiplied by a factor of 1.7108666, holding all other
variables constant. If the country is in the Middle East or North
Africa, the expected median GDP of the country is multiplied by a factor
of 2.6353078, holding all other variables constant. If the country is in
Sub-Saharan Africa, the expected median GDP of the country is multiplied
by a factor of 0.5194421, holding all other variables constant.

For countries who are in Europe, as their TariffRate increases by 1%,
the median GDP of the country is expected to be multiplied by a factor
of 0.9342605, holding all other variables constant.

For countries who are in the Middle East or North Africa, as their
TariffRate increases by 1%, the median GDP of the country is expected to
be multiplied by a factor of 0.9559975, holding all other variables
constant.

For countries who are in Sub-Saharan Africa, as their TariffRate
increases by 1%, the median GDP of the country is expected to be
multiplied by a factor of 0.9436499, holding all other variables
constant.

### Detailed Discussion on EDA for every variable

#### Predictor Variables

We will now look at the rest of the variables that we used to predict
GDP, starting with
`TaxBurden`.

![](final-writeup_files/figure-gfm/EDA1%20AW-1.png)<!-- -->![](final-writeup_files/figure-gfm/EDA1%20AW-2.png)<!-- -->

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##   variable missing complete   n  mean    sd  p0 p25  p50  p75 p100
    ##  TaxBurden       0      173 173 22.19 10.25 1.6  14 20.7 30.2   47
    ##      hist
    ##  ▂▅▇▆▅▅▂▂

`TaxBurden` represents the amount of tax paid by the citizens of a
country as a proportion of the GDP of that country. The distribution of
tax burden is unimodal and only slightly right skewed. The mode is
around 14-15%. In general, the tax burden across countries appears
normally distributed. The mean tax burden is 22.19% and the standard
deviation of the distribution is 10.17%.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##     variable missing complete   n mean    sd   p0  p25  p50  p75 p100
    ##  GovSpending       0      173 173 32.2 10.67 10.6 23.6 31.6 39.8 64.2
    ##      hist
    ##  ▂▇▇▇▇▃▁▁

`GovSpending` represents the amount spent by the government as a
percentage of the GDP of the country. The distribution of government
spending is generally symmmetric and unimodal. The mode of the
distribution is around 25%. Since there is minimal skewing, we report
the mean and standard deviation. The mean government spending is 33.87%
of GDP and the distribution has a standard deviation of 15.52%.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##    variable missing complete   n  mean     sd  p0 p25 p50  p75   p100
    ##  Population       0      173 173 42.16 149.89 0.1 2.9 9.5 31.4 1390.1
    ##      hist
    ##  ▇▁▁▁▁▁▁▁

`Population` represents the number of individuals living in a country.
The distribution of population is unimodal and right-skewed. Because
there are two influential points in “population”, we will plot another
graph of population without these two points below.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##    variable missing complete   n  mean     sd  p0 p25 p50  p75   p100
    ##  Population       0      173 173 42.16 149.89 0.1 2.9 9.5 31.4 1390.1
    ##      hist
    ##  ▇▁▁▁▁▁▁▁

The distribution of population is unimodal and right-skewed. The mode of
the distribution is around 1 million. Since the median and IQR are more
robust to skewing, we report them instead as a measures of center and
spread. The median is 9.15 and the IQR is 26.85. Additionally, when
conducting our analysis, we may need to apply a log-transform to make
the distribution of the variable more normal; based on a pairs plot,
population has a logarithmic relationship with logGDP and thus we will
apply a logarithmic transform to population below.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##      variable missing complete   n mean   sd  p0 p25 p50 p75 p100     hist
    ##  Unemployment       0      173 173 7.27 5.67 0.1 3.7 5.5 9.3 27.3 ▆▇▅▂▁▁▁▁

`Unemployment` represents the percantage of the workforce of a country
that is currently not working. The distribution of unemployment is
unimodal and right-skewed. The mode of the distribution is around 4-5%.
Since the median and IQR are more robust to skewing, we report them
instead as a measures of center and spread. The median is 5.7 and the
IQR is 5.6.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##    variable missing complete   n mean  sd p0 p25 p50 p75 p100     hist
    ##  TariffRate       0      173 173 5.61 4.4  0   2 4.2 8.7 18.6 ▇▅▃▃▃▁▁▁

`TariffRate` represents the average percentage tax on imports that the
country has outstanding as of 2019. The distribution of tariff rate is
generally right skewed and unimodal. There are several outlier economies
with 50% tariff rate such as Central African Republic and North Korea.
The mode of the distribution is around 2%. The median tariff rate is 4.3
and the interquartile range of the distribution is 6.7. Because of this
right-skew, log-transforming this variable might be necessary; we will
examine it further in a pairs plot later in the analysis.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##   variable missing complete   n  mean    sd   p0 p25 p50 p75   p100
    ##  Inflation       0      173 173 10.87 82.56 -0.9 1.3 2.8 5.5 1087.5
    ##      hist
    ##  ▇▁▁▁▁▁▁▁

    ## # A tibble: 4 x 2
    ## # Groups:   cat_inflation [4]
    ##   cat_inflation        n
    ##   <chr>            <int>
    ## 1 Dangerously High    39
    ## 2 Healthy             62
    ## 3 High                42
    ## 4 Low                 30

![](final-writeup_files/figure-gfm/EDA2-2-1.png)<!-- -->![](final-writeup_files/figure-gfm/EDA2-2-2.png)<!-- -->

`Region` represents the geographical continent/area that the country is
situated in. The bar graph and piechart of `Region` shows that there is
a relatively equal representation of countries from different regions of
the world. The Americas, Asia-Pacific, Sub-Saharan Africa, and Europe
each represent around 25% of all the countries in the data. The smallest
representation is from the Middle East and North Africa at 8.1%. We are
not too concerned with the distribution because there are 195 countries
in the world and our data has 173 countries. The difference in
distribution across region is likely to be largely reflective of the
actual geographical distribution of nation-states.

`GovInterference` represents the amount of interference that the
government has in the economy as determined by the World Economic Index.
The distribution of government interference shows that most countries
either have extensive or moderate government inteference. The
distribution between the two is relatively
balanced.

![](final-writeup_files/figure-gfm/Income%20Tax%20Rate%20AW-1.png)<!-- -->

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##       variable missing complete   n  mean    sd p0 p25 p50 p75 p100
    ##  IncomeTaxRate       0      173 173 28.78 13.32  0  20  30  35   60
    ##      hist
    ##  ▂▅▂▇▇▅▂▁

`IncomeTaxRate` represents the average tax rate applied to individuals
on their incomes. The distribution of income tax rate is unimodal and
generally symmetric. While its general shape resembles a normal
distribution, there are several values of income tax rate which have
particularly high frequency such as 10%, 25% and 34-35%. The mode of the
distribution occurs at 35%. Since there is relatively minimal skewing,
we report the mean and standard deviation as measures of center and
spread. The mean income tax rate is 28.23% and the standard deviation of
the distribution is 13.4%.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##          variable missing complete   n  mean   sd p0 p25 p50 p75 p100
    ##  CorporateTaxRate       0      173 173 23.95 8.89  0  20  25  30   50
    ##      hist
    ##  ▁▂▂▇▆▂▁▁

`CorporateTaxRate` represents the average tax rate applied to
corporations on their revenues. The distribution of corporate tax rate
is unimodal and only slightly right skewed. The mode of the distribution
is around 28-30%. Since there is minimal skewing, we report the mean and
the standard deviation as measures of center and spread. The mean
corporate tax rate is 23.89% and the standard deviation is 8.88%.

    ## Skim summary statistics
    ##  n obs: 173 
    ##  n variables: 1 
    ## 
    ## ── Variable type:numeric ─────────────────────────────────────────────────────────────────────────────────────────────
    ##    variable missing complete   n  mean   sd p0  p25  p50  p75  p100
    ##  PublicDebt       0      173 173 56.46 33.8  0 35.2 49.4 69.9 236.4
    ##      hist
    ##  ▃▇▃▂▁▁▁▁

`PublicDebt` represents the debt of the country as a percentage of the
country’s GDP. The distribution of public debt is unimodal and right
skewed. There are several outliers with public debt more than 175%. The
mode of the distribution is around 30%. The median public debt is 49.4%
and the interquartile range is 35%.

#### Multivariate Exploration

Finally, we can visualize paired scatter plots of the relationship
between GDP and each of our predictor variables. This is shown below.
Many of the plots are visually difficult and don’t offer good
information as there are outliers that skew the
visualization.

![](final-writeup_files/figure-gfm/scatterplot%20matrix%20GDP%20growth%20AW-1.png)<!-- -->![](final-writeup_files/figure-gfm/scatterplot%20matrix%20GDP%20growth%20AW-2.png)<!-- -->

To better help us visualize the relationship between the predictor and
response variables for most data points, we plotted a scatterplot matrix
with most of the outliers
removed.

![](final-writeup_files/figure-gfm/Scatterplot%20GDP%20without%20outliers%20AW-1.png)<!-- -->![](final-writeup_files/figure-gfm/Scatterplot%20GDP%20without%20outliers%20AW-2.png)<!-- -->

Based on the scatterplots above, we observe that most predictor
variables do not have a clear linear relationship with the response
variable GDP. Thus, we log transform GDP to see if there is a stronger
linear relationship. As seen previously, logGDP has a normal
distribution while GDP is extremely right
skewed.

![](final-writeup_files/figure-gfm/Scatterplot%20logGDP%20AW-1.png)<!-- -->![](final-writeup_files/figure-gfm/Scatterplot%20logGDP%20AW-2.png)<!-- -->

Based on the scatterplots of the response variables against the
quantitative predictor variables, we see that there is now a much
stronger a linear relationship between the two. Particularly, we note
that the logGDP against population scatterplot shows a curved
distribution which resembles a logarithimic function. This is why we
transformed population into the logpop variable, which has a linear
relationship with GDP. Furthermore, the pairs plot validates all of the
transformations we have applied in the univariate analysis, so they will
remain in place for our regression.

Moreover, we also noticed some multicollinearity issues between
predictor variables. In specific, the relationship between GovSpending
and TaxBurden appears very strongly if not almost linear. Other
variables suspect of multicollinearity problems include tariff rate and
unemployment as well as tariff rate and unemployment.

Within this analysis, we have split the pairs plot for visual
convenience, but we did analyze multicollinearity via a full pairs plot
with all of our variables in one visualization; unfortunately, due to
the large number of variables, this isn’t visually convenient and we
have split the plots within the
markdown.

![](final-writeup_files/figure-gfm/Boxplots%20for%20response%20vs%20categorical%20AW-1.png)<!-- -->![](final-writeup_files/figure-gfm/Boxplots%20for%20response%20vs%20categorical%20AW-2.png)<!-- -->

For the relationship between the response and categorical variables, the
various boxplots suggest that there are generally normally distributed
with minimal skewing. This means that a linear regression between log
GDP and the categorical variables of region and government interference
are appropriate.

We will now mean-center a few of the variables investigated thus far to
ensure meaningful intercepts during the regression.

### Detailed Discussion on Model Selection Proess

Our approach to model selection is as such: we will be running model
selection using all three selection criteria - AIC, BIC, and adjusted
R^2 - and thereafter compare the three models to decide on the final
model.

We have three categorical variables here: cat\_inflation, Region, and
GovInterference. We begin by first setting “Healthy” as the baseline for
cat\_inflation and “Moderate” as the baseline for GovInterference. These
are chosen as the baseline conditions of a healthy inflation rate and
moderate government interference are often seen as the norm. Excessive
government intervention and unhealthy inflation - whether is it too low,
high, or dangerously high - are, in contrast, deviant conditions.

#### Selection using AIC

Below, we conduct model selection using AIC. We first use backward
selection then forward selection and check if the final model selected
is different.

    ## Start:  AIC=-100.41
    ## logGDP ~ TariffRate + logpop + Unemployment + cat_inflation + 
    ##     PublicDebt + GovSpendingCent + IncomeTaxRate + CorporateTaxRate + 
    ##     TaxBurdenCent + GDPGrowth + Region + GovInterference
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## - PublicDebt        1      0.00  78.64 -102.404
    ## - IncomeTaxRate     1      0.04  78.67 -102.331
    ## - Unemployment      1      0.64  79.27 -101.009
    ## - CorporateTaxRate  1      0.77  79.40 -100.723
    ## <none>                           78.63 -100.408
    ## - GovSpendingCent   1      2.67  81.30  -96.638
    ## - TariffRate        1      4.56  83.19  -92.658
    ## - TaxBurdenCent     1      4.77  83.40  -92.227
    ## - cat_inflation     3      7.02  85.65  -91.620
    ## - GovInterference   1      6.58  85.21  -88.512
    ## - GDPGrowth         1      9.49  88.12  -82.696
    ## - Region            4     25.99 104.63  -59.000
    ## - logpop            1    402.39 481.02  210.914
    ## 
    ## Step:  AIC=-102.4
    ## logGDP ~ TariffRate + logpop + Unemployment + cat_inflation + 
    ##     GovSpendingCent + IncomeTaxRate + CorporateTaxRate + TaxBurdenCent + 
    ##     GDPGrowth + Region + GovInterference
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## - IncomeTaxRate     1      0.04  78.67 -104.320
    ## - Unemployment      1      0.67  79.30 -102.943
    ## - CorporateTaxRate  1      0.77  79.41 -102.720
    ## <none>                           78.64 -102.404
    ## - GovSpendingCent   1      2.69  81.32  -98.593
    ## - TariffRate        1      4.65  83.28  -94.468
    ## - TaxBurdenCent     1      4.77  83.40  -94.222
    ## - cat_inflation     3      7.02  85.65  -93.620
    ## - GovInterference   1      6.65  85.29  -90.356
    ## - GDPGrowth         1      9.50  88.14  -84.668
    ## - Region            4     26.05 104.69  -60.898
    ## - logpop            1    403.12 481.76  209.179
    ## 
    ## Step:  AIC=-104.32
    ## logGDP ~ TariffRate + logpop + Unemployment + cat_inflation + 
    ##     GovSpendingCent + CorporateTaxRate + TaxBurdenCent + GDPGrowth + 
    ##     Region + GovInterference
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## - Unemployment      1      0.64  79.31 -104.922
    ## <none>                           78.67 -104.320
    ## - CorporateTaxRate  1      1.26  79.93 -103.576
    ## - GovSpendingCent   1      2.65  81.32 -100.591
    ## - TariffRate        1      4.68  83.36  -96.316
    ## - TaxBurdenCent     1      5.15  83.82  -95.354
    ## - cat_inflation     3      7.16  85.84  -95.248
    ## - GovInterference   1      6.70  85.38  -92.172
    ## - GDPGrowth         1      9.47  88.14  -86.662
    ## - Region            4     26.46 105.13  -62.164
    ## - logpop            1    453.15 531.82  224.283
    ## 
    ## Step:  AIC=-104.92
    ## logGDP ~ TariffRate + logpop + cat_inflation + GovSpendingCent + 
    ##     CorporateTaxRate + TaxBurdenCent + GDPGrowth + Region + GovInterference
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## <none>                           79.31 -104.922
    ## - CorporateTaxRate  1      1.12  80.43 -104.498
    ## - GovSpendingCent   1      2.17  81.48 -102.258
    ## - TaxBurdenCent     1      4.51  83.83  -97.348
    ## - TariffRate        1      4.53  83.85  -97.304
    ## - cat_inflation     3      8.48  87.79  -93.356
    ## - GovInterference   1      7.01  86.32  -92.271
    ## - GDPGrowth         1      8.83  88.14  -88.662
    ## - Region            4     27.65 106.96  -61.184
    ## - logpop            1    467.16 546.47  226.983

    ## Start:  AIC=254.65
    ## logGDP ~ 1
    ## 
    ##                    Df Sum of Sq    RSS    AIC
    ## + logpop            1    496.28 248.94  66.96
    ## + TariffRate        1     93.66 651.57 233.41
    ## + Region            4    112.23 633.00 234.41
    ## + GovInterference   1     70.14 675.09 239.55
    ## + IncomeTaxRate     1     23.40 721.83 251.13
    ## + Unemployment      1     16.80 728.43 252.70
    ## <none>                          745.23 254.65
    ## + TaxBurdenCent     1      3.98 741.25 255.72
    ## + CorporateTaxRate  1      3.08 742.14 255.93
    ## + GovSpendingCent   1      2.36 742.86 256.10
    ## + PublicDebt        1      2.24 742.98 256.13
    ## + GDPGrowth         1      0.02 745.21 256.64
    ## + cat_inflation     3      8.73 736.49 258.61
    ## 
    ## Step:  AIC=66.96
    ## logGDP ~ logpop
    ## 
    ##                    Df Sum of Sq    RSS     AIC
    ## + Region            4   124.050 124.89 -44.369
    ## + TariffRate        1    74.792 174.15   7.148
    ## + GovInterference   1    72.280 176.66   9.625
    ## + cat_inflation     3    48.597 200.35  35.389
    ## + GovSpendingCent   1    42.890 206.05  36.248
    ## + TaxBurdenCent     1    42.441 206.50  36.625
    ## + CorporateTaxRate  1    39.303 209.64  39.233
    ## + GDPGrowth         1     9.792 239.15  62.018
    ## <none>                          248.94  66.960
    ## + PublicDebt        1     0.963 247.98  68.290
    ## + IncomeTaxRate     1     0.352 248.59  68.715
    ## + Unemployment      1     0.025 248.92  68.943
    ## 
    ## Step:  AIC=-44.37
    ## logGDP ~ logpop + Region
    ## 
    ##                    Df Sum of Sq    RSS     AIC
    ## + GovInterference   1   18.5885 106.31 -70.248
    ## + cat_inflation     3   15.1889 109.70 -60.802
    ## + TariffRate        1   10.1830 114.71 -57.083
    ## + TaxBurdenCent     1    6.9412 117.95 -52.261
    ## + GDPGrowth         1    6.3745 118.52 -51.432
    ## + GovSpendingCent   1    2.2755 122.62 -45.550
    ## <none>                          124.89 -44.369
    ## + CorporateTaxRate  1    1.1701 123.72 -43.997
    ## + IncomeTaxRate     1    1.0811 123.81 -43.873
    ## + PublicDebt        1    0.3532 124.54 -42.859
    ## + Unemployment      1    0.0003 124.89 -42.370
    ## 
    ## Step:  AIC=-70.25
    ## logGDP ~ logpop + Region + GovInterference
    ## 
    ##                    Df Sum of Sq     RSS     AIC
    ## + GDPGrowth         1    8.6872  97.617 -82.997
    ## + cat_inflation     3   10.2832  96.021 -81.848
    ## + TaxBurdenCent     1    4.5921 101.713 -75.887
    ## + TariffRate        1    4.5888 101.716 -75.882
    ## + GovSpendingCent   1    2.5068 103.798 -72.376
    ## + IncomeTaxRate     1    1.4486 104.856 -70.622
    ## <none>                          106.305 -70.248
    ## + CorporateTaxRate  1    0.0532 106.251 -68.335
    ## + PublicDebt        1    0.0389 106.266 -68.311
    ## + Unemployment      1    0.0160 106.289 -68.274
    ## 
    ## Step:  AIC=-83
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth
    ## 
    ##                    Df Sum of Sq    RSS     AIC
    ## + cat_inflation     3    8.9655 88.652 -93.663
    ## + TariffRate        1    4.8624 92.755 -89.836
    ## + TaxBurdenCent     1    3.4041 94.213 -87.137
    ## <none>                          97.617 -82.997
    ## + IncomeTaxRate     1    0.8360 96.781 -82.485
    ## + GovSpendingCent   1    0.3774 97.240 -81.667
    ## + CorporateTaxRate  1    0.1945 97.423 -81.342
    ## + Unemployment      1    0.1799 97.438 -81.316
    ## + PublicDebt        1    0.0001 97.617 -80.997
    ## 
    ## Step:  AIC=-93.66
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth + cat_inflation
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## + TariffRate        1    4.6454 84.007 -100.975
    ## + TaxBurdenCent     1    2.4079 86.244  -96.427
    ## <none>                          88.652  -93.663
    ## + CorporateTaxRate  1    0.2659 88.386  -92.183
    ## + GovSpendingCent   1    0.0673 88.585  -91.794
    ## + IncomeTaxRate     1    0.0502 88.602  -91.761
    ## + Unemployment      1    0.0177 88.634  -91.698
    ## + PublicDebt        1    0.0103 88.642  -91.683
    ## 
    ## Step:  AIC=-100.97
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth + cat_inflation + 
    ##     TariffRate
    ## 
    ##                    Df Sum of Sq    RSS      AIC
    ## + TaxBurdenCent     1   2.03354 81.973 -103.214
    ## <none>                          84.007 -100.975
    ## + CorporateTaxRate  1   0.18088 83.826  -99.347
    ## + IncomeTaxRate     1   0.08925 83.917  -99.158
    ## + PublicDebt        1   0.01375 83.993  -99.003
    ## + Unemployment      1   0.00162 84.005  -98.978
    ## + GovSpendingCent   1   0.00137 84.005  -98.977
    ## 
    ## Step:  AIC=-103.21
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth + cat_inflation + 
    ##     TariffRate + TaxBurdenCent
    ## 
    ##                    Df Sum of Sq    RSS     AIC
    ## + GovSpendingCent   1   1.54136 80.432 -104.50
    ## <none>                          81.973 -103.21
    ## + CorporateTaxRate  1   0.49334 81.480 -102.26
    ## + Unemployment      1   0.14744 81.826 -101.53
    ## + IncomeTaxRate     1   0.13089 81.842 -101.49
    ## + PublicDebt        1   0.05708 81.916 -101.33
    ## 
    ## Step:  AIC=-104.5
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth + cat_inflation + 
    ##     TariffRate + TaxBurdenCent + GovSpendingCent
    ## 
    ##                    Df Sum of Sq    RSS     AIC
    ## + CorporateTaxRate  1   1.11903 79.313 -104.92
    ## <none>                          80.432 -104.50
    ## + Unemployment      1   0.49951 79.932 -103.58
    ## + IncomeTaxRate     1   0.38006 80.052 -103.32
    ## + PublicDebt        1   0.05886 80.373 -102.62
    ## 
    ## Step:  AIC=-104.92
    ## logGDP ~ logpop + Region + GovInterference + GDPGrowth + cat_inflation + 
    ##     TariffRate + TaxBurdenCent + GovSpendingCent + CorporateTaxRate
    ## 
    ##                 Df Sum of Sq    RSS     AIC
    ## <none>                       79.313 -104.92
    ## + Unemployment   1   0.63868 78.674 -104.32
    ## + PublicDebt     1   0.03383 79.279 -103.00
    ## + IncomeTaxRate  1   0.00994 79.303 -102.94

AIC backward selection: logGDP ~ TariffRate + logpop + cat\_inflation +
GovSpendingCent + CorporateTaxRate + TaxBurdenCent + GDPGrowth + Region
+ GovInterference

AIC forward selection: logGDP ~ TariffRate + logpop + cat\_inflation +
GovSpendingCent + CorporateTaxRate + TaxBurdenCent + GDPGrowth + Region
+ GovInterference

As we have seen from the model selection process above, both forward and
backward selection rendered the same final model. We will however not be
choosing this model immediately. Instead, we will compare it with the
other models generated using other selection criteria such as BIC and
adj
R^2.

| term                               |    estimate | std.error |   statistic |   p.value |    conf.low |   conf.high |
| :--------------------------------- | ----------: | --------: | ----------: | --------: | ----------: | ----------: |
| (Intercept)                        |   3.7419175 | 0.2630588 |  14.2246417 | 0.0000000 |   3.2223521 |   4.2614829 |
| TariffRate                         | \-0.0474462 | 0.0157872 | \-3.0053608 | 0.0030861 | \-0.0786274 | \-0.0162650 |
| logpop                             |   1.0165056 | 0.0333211 |  30.5063234 | 0.0000000 |   0.9506932 |   1.0823179 |
| cat\_inflationDangerously High     | \-0.4256532 | 0.1635835 | \-2.6020545 | 0.0101478 | \-0.7487457 | \-0.1025607 |
| cat\_inflationHigh                 | \-0.5116288 | 0.1549753 | \-3.3013562 | 0.0011896 | \-0.8177193 | \-0.2055382 |
| cat\_inflationLow                  |   0.0415998 | 0.1664769 |   0.2498832 | 0.8030022 | \-0.2872074 |   0.3704069 |
| GovSpendingCent                    | \-0.0195980 | 0.0094324 | \-2.0777415 | 0.0393508 | \-0.0382278 | \-0.0009682 |
| CorporateTaxRate                   | \-0.0119116 | 0.0079780 | \-1.4930639 | 0.1374147 | \-0.0276689 |   0.0038456 |
| TaxBurdenCent                      |   0.0332804 | 0.0110994 |   2.9983836 | 0.0031537 |   0.0113580 |   0.0552028 |
| GDPGrowth                          | \-0.0979726 | 0.0233611 | \-4.1938316 | 0.0000456 | \-0.1441130 | \-0.0518322 |
| RegionAsia-Pacific                 |   0.2626906 | 0.1936575 |   1.3564699 | 0.1768848 | \-0.1198008 |   0.6451819 |
| RegionEurope                       |   0.2204642 | 0.2128560 |   1.0357435 | 0.3019044 | \-0.1999460 |   0.6408744 |
| RegionMiddle East and North Africa |   0.8347061 | 0.2717274 |   3.0718512 | 0.0025057 |   0.2980195 |   1.3713928 |
| RegionSub-Saharan Africa           | \-0.8473786 | 0.1862555 | \-4.5495504 | 0.0000107 | \-1.2152503 | \-0.4795069 |
| GovInterferenceExtensive           | \-0.5116125 | 0.1369156 | \-3.7366985 | 0.0002601 | \-0.7820335 | \-0.2411915 |

#### Selection using BIC

We now conduct model selection using BIC. Same as before, we do both
forward and backward selection for comparison.

    ##                   (Intercept)                    TariffRate 
    ##                    3.84221675                   -0.05541749 
    ##                        logpop cat_inflationDangerously High 
    ##                    1.00198954                   -0.51048522 
    ##             cat_inflationHigh                     GDPGrowth 
    ##                   -0.57317368                   -0.08294272 
    ##      GovInterferenceExtensive      RegionSub-Saharan Africa 
    ##                   -0.66337220                   -1.13792903

    ##                   (Intercept)                    TariffRate 
    ##                    3.84221675                   -0.05541749 
    ##                        logpop cat_inflationDangerously High 
    ##                    1.00198954                   -0.51048522 
    ##             cat_inflationHigh                     GDPGrowth 
    ##                   -0.57317368                   -0.08294272 
    ##      GovInterferenceExtensive      RegionSub-Saharan Africa 
    ##                   -0.66337220                   -1.13792903

From the above, we see that both the forward and backward selection
process rendered the same model.

#### Selection using adj R^2

We now conduct model selection using adj R^2. Same as before, we do both
forward and backward selection for comparison.

    ##                   (Intercept)                    TariffRate 
    ##                    3.74431705                   -0.04998866 
    ##                        logpop cat_inflationDangerously High 
    ##                    1.01165029                   -0.46830454 
    ##             cat_inflationHigh                 TaxBurdenCent 
    ##                   -0.53642530                    0.01104291 
    ##                     GDPGrowth      GovInterferenceExtensive 
    ##                   -0.08029032                   -0.62499945 
    ##      RegionSub-Saharan Africa 
    ##                   -1.12006020

    ##                   (Intercept)                    TariffRate 
    ##                    3.74431705                   -0.04998866 
    ##                        logpop cat_inflationDangerously High 
    ##                    1.01165029                   -0.46830454 
    ##             cat_inflationHigh                 TaxBurdenCent 
    ##                   -0.53642530                    0.01104291 
    ##                     GDPGrowth      GovInterferenceExtensive 
    ##                   -0.08029032                   -0.62499945 
    ##      RegionSub-Saharan Africa 
    ##                   -1.12006020

From the above, we see that both the forward and backward selection
process rendered the same model.

#### Model Comparison between all 3 Selection Criteria

Model selected using AIC: logGDP ~ TariffRate + logpop + GDPGrowth +
TaxBurdenCent + GovSpendingCent + cat\_inflationDangerouslyHigh +
cat\_inflationHigh + cat\_inflationLow + RegionAsia-Pacific +
RegionEurope + RegionMiddleEastandNorthAfrica + RegionSub-SaharanAfrica
+ GovInterferenceLimited + GovInterferenceExtensive +
GovInterferenceRepressive

Model selected using BIC: logGDP ~ TariffRate + logpop + GDPGrowth +
cat\_inflationDangerouslyHigh + cat\_inflationHigh +
GovInterferenceExtensive + RegionSub-Saharan Africa

Model selected using adj R^2: logGDP ~ TariffRate + logpop + GDPGrowth +
TaxBurdenCent + cat\_inflationDangerously High + cat\_inflationHigh +
GovInterferenceLimited + GovInterferenceModerate + RegionSub-Saharan
Africa

From the final equations generated using different selection criteria,
we see that all three selection criteria included TariffRate + logpop +
GDPGrowth + GovInterference in the final model. For that reason, we
decide to include all four of these predictors into our final model.

cat\_inflation is included in all three equations, though for BIC and
adj R^2, the level of low inflation is excluded. This is not surprising
considering that the p-value of the cat\_inflationLow coefficient in the
AIC model is 0.8030022, implying that this coefficient is very likely to
be insignificant.

While there is one level that could be very statistically insignificant,
we will choose to include cat\_inflation in the final model for the
following reasons. One, a categorical variable with 4 levels require us
to use 3 dummy variables. Removing one because it is insignificant will
affect the coefficients of the other levels because the missing level
has to be forcibly added to either of the three category. We will also
not change the categories of inflation rates because the present
classification references economic literature which generally suggests
four levels of inflation rates: low, healthy, high, and dangerously high
(hyperinflation). Two, we also note that the AIC coefficient for
cat\_inflationLow to be extremely small. This means that the regression
model only made very minimal changes to fit the data categorized as Low
inflation, thus we are not too concerned that the inclusion of this
level will significantly impact the accuracy of the regression.

The same is observed for the categorical variable Region. The final BIC
and adj R^2 models selected only RegionSub-SaharanAfrica. In this case,
it is hard to conclude that the other levels of the categories are
statistically insignificant as their p values in the final models are
not extremely high/ close to 1. In fact, the coefficient of
RegionMiddleEastandNorthAfrica has a very low p value at 0.0025057,
while those of RegionEurope and RegionAsia-Pacific are above From these
circumstantial evidence, we are hesitant to recode the Region variable
to have only two values: Sub-Saharan Africa and Not Subsaharan Africa.
There seems to be a statistical difference between being in the Middle
East and North Africa versus Europe or Asia Pacific.

GovSpendingCent is included only in the AIC equation while TaxBurdenCent
is included in both the AIC and adjR2 equations. We conduct a nested F
test to finally decide if these two predictor variables are significant
to the model with predictor variables which we have decided to include
thus far (TariffRate + logpop + GDPGrowth + cat\_inflation +
GovInterference + Region) based on the above reasoning.

Nested F test for GovSpendingCent:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    161 | 84.007 | NA |        NA |    NA |      NA |
|    160 | 84.005 |  1 |     0.001 | 0.003 |   0.959 |

The p value of the nested F test for GovSpendingCent is extremely high
and thus we choose not include it in our model. Moreover, we also do not
accord it high practical significance. The role of government spending
is also highly contested in economic literature depending on which
school of thought you subscribe to. That it is not a significant
determinant of GDP reflect this ambuiguity in academic literature.

Nested F test for TaxBurdenCent:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    161 | 84.007 | NA |        NA |    NA |      NA |
|    160 | 81.973 |  1 |     2.034 | 3.969 |   0.048 |

The p value of the nested F test for TaxBurdenCent = 0.048 \< 0.05. We
thus choose to include TaxBurdenCent into our final model.

#### Testing Interaction Variables

To test for interaction between the categorical and predictor varaibles,
we will conducted nested F tests.

We do so by first testing for interaction between cat\_inflation and the
4 quantitative variables.

Interaction between cat\_inflation and TariffRate:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    157 | 80.249 |  3 |     1.724 | 1.124 |   0.341 |

Interaction between cat\_inflation and logpop:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    157 | 81.133 |  3 |      0.84 | 0.542 |   0.654 |

Interaction between cat\_inflation and GDPGrowth:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    157 | 80.441 |  3 |     1.533 | 0.997 |   0.396 |

Interaction between cat\_inflation and TaxBurdenCent:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    157 | 80.441 |  3 |     1.533 | 0.997 |   0.396 |

As seen from the nested F tests, none of the p values are close to 0.05.
This implies that none of the interaction between the 4 quantitive
predictor variables and cat\_inflation is statistically significant.

We thus include no interaction variables for cat\_inflation.

We now test for interaction between GovInterference and the 4
quantitative variables.

Interaction between GovInterference and TariffRate:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    159 | 80.259 |  1 |     1.714 | 3.396 |   0.067 |

Interaction between GovInterference and logpop:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    159 | 80.833 |  1 |      1.14 | 2.242 |   0.136 |

Interaction between GovInterference and GDPGrowth:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    159 | 81.944 |  1 |     0.029 | 0.056 |   0.813 |

Interaction between GovInterference and TaxBurdenCent:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    159 | 81.973 |  1 |     0.001 | 0.001 |   0.975 |

From the nested F tests above, we see that the p values for the
interaction variables between GovInterference and TaxBurdenCent,
GDPGrowth, logpop are all way above 0.05. Only the p value of the nested
F test for the interaction between GovInterference and TariffRate is
0.067 which is close to 0.05. Since the p value is not far from 0.05 and
we know that the dataset does not have a large number of observation, it
is reasonable to assume that the effect of GovInterference on TariffRate
must have been considerably large for the p value to be so close 0.05
even with a small dataset.

Thus, we choose to include the interaction term
GovInterference\*TariffRate into the final model.

We now test for interaction between Region and the 4 quantitative
variables.

Interaction between Region and TariffRate:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    156 | 74.820 |  4 |     7.153 | 3.729 |   0.006 |

Interaction between Region and logpop:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    156 | 80.486 |  4 |     1.487 | 0.721 |   0.579 |

Interaction between Region and GDPGrowth:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    156 | 78.894 |  4 |     3.079 | 1.522 |   0.198 |

Interaction between Region and TaxBurdenCent:

| Res.Df |    RSS | Df | Sum of Sq |     F | Pr(\>F) |
| -----: | -----: | -: | --------: | ----: | ------: |
|    160 | 81.973 | NA |        NA |    NA |      NA |
|    156 | 78.954 |  4 |     3.019 | 1.491 |   0.207 |

From the nested F tests above, we see that the p values for the
interaction variables between Region and TaxBurdenCent, GDPGrowth,
logpop are all way above 0.05. Only the p value of the nested F test for
the interaction between Region and TariffRate is 0.006 which is smaller
than 0.05. This implies that there is significant interaction between
Region and TariffRate.

Thus, we choose to include the interaction term Region\*TariffRate into
the final model.

### Detailed discussion of Model Assumptions

##### Independence Check

Because our data is about countries, we expect countries in the same
region to have similar GDP and status, which might harm the independence
assumption of our model. So, we made color-coded plot of residuals
vs. region to see if region really affects the independence.

![](final-writeup_files/figure-gfm/AW%20independence-1.png)<!-- -->

By looking at the color-coded residual plot, residual points of each
category seems randomly scattered and don’t have distinguishable shape.
So this finding indicates no cluster effect and thus negates the
possible violation of independence assumption.

Moreover, we do not expect that the recording of GDP in one country to
be affected by the recording of GDP in another. In that sense, the
observations are deduced to be
independent.

##### Constant Variance Check

![](final-writeup_files/figure-gfm/AW%20residual%20vs.%20predicted-1.png)<!-- -->

From the residuals vs. predicted values plot above, we can also conclude
that the linearity assumption is satisfied. The figure shows no obvious
shape or pattern, and therefore, the constant variance assumption is
also
satisfied.

![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-1.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-2.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-3.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-4.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-5.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-6.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-7.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residuals%20vs.%20predictor%20variables-8.png)<!-- -->

The constant variance assumption is further satisfied because the
residuals from each of the residuals vs. quantitative predictor
variables plot is randomly scattered and doesn’t appear any
distinguishable shape. For those categorical predictor variables, the
boxplot of residuals are appproximately centered around 0, sysmetric,
and the interquantile range is roughly the same for each category.
Furthermore, for interaction terms, we made color coded residual plots
according to different categories. And for each category (each color),
the residual points all look randomly scattered. So, we can say that
constant variance assumption is
met.

##### Normality Check

![](final-writeup_files/figure-gfm/AW%20residual%20normality%20check-1.png)<!-- -->![](final-writeup_files/figure-gfm/AW%20residual%20normality%20check-2.png)<!-- -->

The normality assumption is satisfied since the distribution of
residuals appears to be unimodal and approximately normal. Also, the
normal QQ plot of points fit the theoretical line very well.

##### Leverage, Standardized Residual, Cook’s Distance

Lastly, we augmented the model and took a look into leverage, cook’s
distance, and standardized residual to check for outliers and assess
their influence on our model.

    ## Observations: 5
    ## Variables: 16
    ## $ logGDP          <dbl> 4.242765, 3.583519, 6.450312, 5.248602, 6.824591
    ## $ TariffRate      <dbl> 7.0, 1.1, 8.8, 9.4, 7.5
    ## $ logpop          <dbl> 3.569533, 1.064711, 3.725693, 3.339322, 3.786460
    ## $ GDPGrowth       <dbl> 2.5, 3.9, 2.0, 0.7, 2.9
    ## $ TaxBurdenCent   <dbl> -17.193642, 2.706358, 2.306358, -1.593642, 8.606…
    ## $ cat_inflation   <fct> High, Healthy, High, Dangerously High, Dangerous…
    ## $ GovInterference <fct> Extensive, Moderate, Extensive, Extensive, Moder…
    ## $ Region          <chr> "Asia-Pacific", "Europe", "Middle East and North…
    ## $ .fitted         <dbl> 5.139058, 4.471403, 6.551641, 4.457461, 6.492631
    ## $ .se.fit         <dbl> 0.1886207, 0.1868457, 0.2934447, 0.1522129, 0.20…
    ## $ .resid          <dbl> -0.8962932, -0.8878845, -0.1013290, 0.7911412, 0…
    ## $ .hat            <dbl> 0.07683060, 0.07539135, 0.18595507, 0.05003320, …
    ## $ .sigma          <dbl> 0.6785456, 0.6786297, 0.6826367, 0.6795560, 0.68…
    ## $ .cooksd         <dbl> 0.0086886948, 0.0083406800, 0.0003456703, 0.0041…
    ## $ .std.resid      <dbl> -1.3708410, -1.3569230, -0.1650393, 1.1928271, 0…
    ## $ obs_num         <int> 1, 2, 3, 4, 5

![](final-writeup_files/figure-gfm/AW%20leverage-1.png)<!-- -->

    ## # A tibble: 59 x 16
    ##    logGDP TariffRate logpop GDPGrowth TaxBurdenCent cat_inflation
    ##     <dbl>      <dbl>  <dbl>     <dbl>         <dbl> <fct>        
    ##  1   6.45        8.8  3.73        2            2.31 High         
    ##  2   2.45       18.6 -0.916       1.3         -5.89 Healthy      
    ##  3   4.25        3.1  0.405       3.2        -16.6  Healthy      
    ##  4   6.53       10.7  5.09        7.1        -13.4  High         
    ##  5   1.65       14.2 -1.20        0.9         11.5  High         
    ##  6   5.19        1.8  2.25        2.4          1.61 Dangerously …
    ##  7   3.66        0.6  0.788       2.2          2.71 High         
    ##  8   3.51        0.5 -0.916       0.5          2.01 Low          
    ##  9   4.16        9.8  2.77        6.9         -7.19 Healthy      
    ## 10   4.49       15.8  3.19        3.2         -6.59 Low          
    ## # … with 49 more rows, and 10 more variables: GovInterference <fct>,
    ## #   Region <chr>, .fitted <dbl>, .se.fit <dbl>, .resid <dbl>, .hat <dbl>,
    ## #   .sigma <dbl>, .cooksd <dbl>, .std.resid <dbl>, obs_num <int>

    ## # A tibble: 59 x 2
    ##    obs_num  .cooksd
    ##      <int>    <dbl>
    ##  1       3 0.000346
    ##  2      10 0.0498  
    ##  3      11 0.00381 
    ##  4      12 0.00377 
    ##  5      13 0.000301
    ##  6      14 0.0122  
    ##  7      21 0.0218  
    ##  8      23 0.00132 
    ##  9      28 0.00498 
    ## 10      29 0.000855
    ## # … with 49 more rows

There are 8 points with high leverage. After printing them out, we can
see the reason why they have high leverage compared with other points:
some observations have significantly high population (thus logpop is
high), other observations have lower TaxBurdenCent than normal
datapoints, while the rest have lower corporate taxrate than most
points. In conclusion, significant high or low values in predictor
variables lead to high leverage of those
datapoints.

![](final-writeup_files/figure-gfm/AW%20standardized%20residuals-1.png)<!-- -->

    ## [1] 8

There are 8 observations that are considered to have large standardized
residuals with large magnitudes. If ploted the distribution of
standardized residuals using a N(0,1) distribution, we can expect 8/173
= 4.62% of the observations have standardized residuals with magnitude
\> 2. We might need to remove these points from our final data in order
to train a more accurate model.

![](final-writeup_files/figure-gfm/AW%20cook%20distace-1.png)<!-- -->

Even though there are 8 points with high leverage and high standardized
residues, none of them has a cook distance that is larger than 1.
Therefore, we don’t believe that they have significant influence on
model coefficients, so it is safe to keep them in our model.

``` r
tidy(vif(final_model))
```

    ## # A tibble: 17 x 2
    ##    names                                             x
    ##    <chr>                                         <dbl>
    ##  1 TariffRate                                     7.44
    ##  2 logpop                                         1.21
    ##  3 GDPGrowth                                      1.37
    ##  4 TaxBurdenCent                                  2.39
    ##  5 cat_inflationDangerously High                  1.74
    ##  6 cat_inflationHigh                              1.66
    ##  7 cat_inflationLow                               1.42
    ##  8 GovInterferenceExtensive                       5.12
    ##  9 RegionAsia-Pacific                             5.95
    ## 10 RegionEurope                                   9.97
    ## 11 RegionMiddle East and North Africa             5.21
    ## 12 RegionSub-Saharan Africa                       8.71
    ## 13 TariffRate:GovInterferenceExtensive            7.89
    ## 14 TariffRate:RegionAsia-Pacific                  6.05
    ## 15 TariffRate:RegionEurope                        6.16
    ## 16 TariffRate:RegionMiddle East and North Africa  5.04
    ## 17 TariffRate:RegionSub-Saharan Africa           15.8

There is only variable that exceeds the threshold of 10:
TariffRate:RegionSub-Saharan Africa. However, we don’t see another one
that is close to 15.758. Usually we would be concerned if two variables
in the model have close VIF values exceeding 10. This means the two
variables are highly correlated with each other. Since that is not the
case here, we are less concernned about the model validity.

According to other model diagonistics, our model looks good, and it is
not unreasonable to have a high multicollinearity for this particular
interaction variable. Its high multicollinearity means that knowing that
a country is from subsaharan Africa, one can easily predict the
coefficient of the TariffRate predictor variable. This is not entirely
surprising considering that the region has a considerably higher tariff
rates compared to all other
regions.

![](final-writeup_files/figure-gfm/AW%20Tariff%20rate%20by%20region-1.png)<!-- -->

Moreover the VIF values for the interaction between TariffRate and other
levels of Region are not high. This means that they are useful in the
model and we will not want to remove all the other meaningful
interactions.

For the above reasons, we choose to keep the interaction variables
between TariffRate and all the levels of Region.

### Citations

<https://www.heritage.org/index/ranking>

<https://ideas.repec.org/a/rsr/supplm/v61y2013i1p96-104.html>

<https://iopscience.iop.org/article/10.1088/1742-6596/820/1/012008>
