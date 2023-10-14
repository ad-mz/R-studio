iris-dataset-analysis
================
Adrian M
2023-10-14

``` r
# install.packages("vctrs")
# install.packages("tidyverse")
# install.packages("ggplot2")
```

Copying iris dataset to variable df

``` r
df <- iris
```

## Testing scatter plot

![](iris-dataset-analysis_files/figure-gfm/testPlot-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

``` r
head(df)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa
