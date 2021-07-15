ANCOVA test for `pos.score`\~`pre.score`+`scenario`\*`achievement`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “pos.score”](#ancova-plots-for-the-dependent-variable-pos.score)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for pos.score
    [../data/table-for-pos.score.csv](../data/table-for-pos.score.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | achievement | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr | symmetry | skewness | kurtosis |
|:-------------|:------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|:---------|---------:|---------:|
| gamified     | lower       | pos.score |   8 | 8.625 |    8.5 |   7 |  10 | 1.061 | 0.375 | 0.887 | 1.25 | YES      |    0.029 |   -1.556 |
| gamified     | upper       | pos.score |   8 | 9.250 |    9.0 |   8 |  10 | 0.707 | 0.250 | 0.591 | 1.00 | few data |    0.000 |    0.000 |
| non.gamified | lower       | pos.score |   7 | 7.143 |    7.0 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 | few data |    0.000 |    0.000 |
| non.gamified | upper       | pos.score |   7 | 6.857 |    7.0 |   5 |   8 | 1.069 | 0.404 | 0.989 | 1.00 | YES      |   -0.472 |   -1.267 |
| NA           | NA          | pos.score |  30 | 8.033 |    8.0 |   5 |  10 | 1.351 | 0.247 | 0.505 | 2.00 | YES      |   -0.220 |   -0.773 |

![](/home/rstudio/report/ancova/c352a5ebc926d6c6/results/ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(

)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|           | var       |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----------|:----------|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| pos.score | pos.score |  30 |    0.602 |   -0.398 | NO       |     0.946 | Shapiro-Wilk | 0.129 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | achievement | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower       | pos.score |   8 | 8.625 |    8.5 |   7 |  10 | 1.061 | 0.375 | 0.887 | 1.25 | YES       | Shapiro-Wilk |     0.912 | 0.366 | ns       |
| gamified     | upper       | pos.score |   8 | 9.250 |    9.0 |   8 |  10 | 0.707 | 0.250 | 0.591 | 1.00 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | lower       | pos.score |   7 | 7.143 |    7.0 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | upper       | pos.score |   7 | 6.857 |    7.0 |   5 |   8 | 1.069 | 0.404 | 0.989 | 1.00 | YES       | Shapiro-Wilk |     0.894 | 0.294 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["pos.score"]], x=covar, y="pos.score", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](/home/rstudio/report/ancova/c352a5ebc926d6c6/results/ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|             | var       | method         | formula                           |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------------|:----------|:---------------|:----------------------------------|----:|--------:|--------:|----------:|------:|:---------|
| pos.score.1 | pos.score | Levene’s test  | `.res`\~`scenario`\*`achievement` |  30 |       3 |      26 |     0.679 | 0.573 | ns       |
| pos.score.2 | pos.score | Anova’s slopes | `.res`\~`scenario`\*`achievement` |  30 |       3 |      22 |     0.123 | 0.945 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|             | scenario     | achievement | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr |
|:------------|:-------------|:------------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|
| pos.score.1 | gamified     | lower       | pos.score |   8 | 8.625 |    8.5 |   7 |  10 | 1.061 | 0.375 | 0.887 | 1.25 |
| pos.score.2 | gamified     | upper       | pos.score |   8 | 9.250 |    9.0 |   8 |  10 | 0.707 | 0.250 | 0.591 | 1.00 |
| pos.score.3 | non.gamified | lower       | pos.score |   7 | 7.143 |    7.0 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 |
| pos.score.4 | non.gamified | upper       | pos.score |   7 | 6.857 |    7.0 |   5 |   8 | 1.069 | 0.404 | 0.989 | 1.00 |

![](/home/rstudio/report/ancova/c352a5ebc926d6c6/results/ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var       | Effect               | DFn | DFd |    SSn |   SSd |      F |     p |   ges | p.signif |
|:----------|:---------------------|----:|----:|-------:|------:|-------:|------:|------:|:---------|
| pos.score | pre.score            |   1 |  25 | 14.885 | 8.204 | 45.361 | 0.000 | 0.645 | \*\*\*\* |
| pos.score | scenario             |   1 |  25 | 23.096 | 8.204 | 70.381 | 0.000 | 0.738 | \*\*\*\* |
| pos.score | achievement          |   1 |  25 |  2.903 | 8.204 |  8.845 | 0.006 | 0.261 | \*\*     |
| pos.score | scenario:achievement |   1 |  25 |  0.043 | 8.204 |  0.131 | 0.720 | 0.005 | ns       |

### Pairwise comparison

| var       | scenario     | achievement | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----------|:-------------|:------------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| pos.score | NA           | lower       | gamified | non.gamified |    1.694 |    1.080 |     2.308 | 0.298 |     5.681 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | NA           | upper       | gamified | non.gamified |    1.851 |    1.218 |     2.484 | 0.307 |     6.025 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | gamified     | NA          | lower    | upper        |   -0.718 |   -1.308 |    -0.127 | 0.287 |    -2.502 | 0.019 | 0.019 | \*           |
| pos.score | non.gamified | NA          | lower    | upper        |   -0.560 |   -1.242 |     0.121 | 0.331 |    -1.693 | 0.103 | 0.103 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var       | scenario     | achievement |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----------|:-------------|:------------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| pos.score | gamified     | lower       |   8 |  8.502 | 8.625 |    8.083 |     8.920 | 1.061 |   0.575 |   0.203 |
| pos.score | gamified     | upper       |   8 |  9.219 | 9.250 |    8.802 |     9.636 | 0.707 |   0.573 |   0.203 |
| pos.score | non.gamified | lower       |   7 |  6.808 | 7.143 |    6.350 |     7.265 | 0.900 |   0.588 |   0.222 |
| pos.score | non.gamified | upper       |   7 |  7.368 | 6.857 |    6.896 |     7.841 | 1.069 |   0.607 |   0.229 |

### Ancova plots for the dependent variable “pos.score”

``` r
plots <- twoWayAncovaPlots(sdat[["pos.score"]], "pos.score", between
, aov[["pos.score"]], pwc[["pos.score"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `pos.score` \~ `scenario`

``` r
plots[["scenario"]]
```

![](/home/rstudio/report/ancova/c352a5ebc926d6c6/results/ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `pos.score` \~ `achievement`

``` r
plots[["achievement"]]
```

![](/home/rstudio/report/ancova/c352a5ebc926d6c6/results/ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “pre.score”, ANCOVA tests
with independent between-subjects variables “scenario” (gamified,
non.gamified) and “achievement” (lower, upper) were performed to
determine statistically significant difference on the dependent varibles
“pos.score”. For the dependent variable “pos.score”, there was
statistically significant effects in the factor “pre.score” with
F(1,25)=45.361, p&lt;0.001 and ges=0.645 (effect size) and in the factor
“scenario” with F(1,25)=70.381, p&lt;0.001 and ges=0.738 (effect size)
and in the factor “achievement” with F(1,25)=8.845, p=0.006 and
ges=0.261 (effect size).

Pairwise comparisons using the Estimated Marginal Means (EMMs) were
computed to find statistically significant diferences among the groups
defined by the independent variables, and with the p-values ajusted by
the method “bonferroni”. For the dependent variable “pos.score”, the
mean in the scenario=“gamified” (adj M=8.502 and SD=1.061) was
significantly different than the mean in the scenario=“non.gamified”
(adj M=6.808 and SD=0.9) with p-adj&lt;0.001; the mean in the
scenario=“gamified” (adj M=9.219 and SD=0.707) was significantly
different than the mean in the scenario=“non.gamified” (adj M=7.368 and
SD=1.069) with p-adj&lt;0.001; the mean in the achievement=“lower” (adj
M=8.502 and SD=1.061) was significantly different than the mean in the
achievement=“upper” (adj M=9.219 and SD=0.707) with p-adj=0.019.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.
