mtcars-data-anaysis
================
Adrian Manriza
2023-10-16

# Introduction

Motor Trend Car Road Tests

The data was extracted from the 1974 Motor Trend US magazine, and
comprises fuel consumption and 10 aspects of automobile design and
performance for 32 automobiles (1973–74 models).

Looking at the dataset

``` r
head(mtcars)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

``` r
summary(mtcars)
```

    ##       mpg             cyl             disp             hp       
    ##  Min.   :10.40   Min.   :4.000   Min.   : 71.1   Min.   : 52.0  
    ##  1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8   1st Qu.: 96.5  
    ##  Median :19.20   Median :6.000   Median :196.3   Median :123.0  
    ##  Mean   :20.09   Mean   :6.188   Mean   :230.7   Mean   :146.7  
    ##  3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0   3rd Qu.:180.0  
    ##  Max.   :33.90   Max.   :8.000   Max.   :472.0   Max.   :335.0  
    ##       drat             wt             qsec             vs        
    ##  Min.   :2.760   Min.   :1.513   Min.   :14.50   Min.   :0.0000  
    ##  1st Qu.:3.080   1st Qu.:2.581   1st Qu.:16.89   1st Qu.:0.0000  
    ##  Median :3.695   Median :3.325   Median :17.71   Median :0.0000  
    ##  Mean   :3.597   Mean   :3.217   Mean   :17.85   Mean   :0.4375  
    ##  3rd Qu.:3.920   3rd Qu.:3.610   3rd Qu.:18.90   3rd Qu.:1.0000  
    ##  Max.   :4.930   Max.   :5.424   Max.   :22.90   Max.   :1.0000  
    ##        am              gear            carb      
    ##  Min.   :0.0000   Min.   :3.000   Min.   :1.000  
    ##  1st Qu.:0.0000   1st Qu.:3.000   1st Qu.:2.000  
    ##  Median :0.0000   Median :4.000   Median :2.000  
    ##  Mean   :0.4062   Mean   :3.688   Mean   :2.812  
    ##  3rd Qu.:1.0000   3rd Qu.:4.000   3rd Qu.:4.000  
    ##  Max.   :1.0000   Max.   :5.000   Max.   :8.000

First, I want to try cluster analysis.

Clustering, meaning to group (or cluster) similar items together based
on their characteristics. The goal is to partition a dataset into
groups, or clusters, where items in the same group are more similar to
each other than to those in other groups.

Preparing data for hierarchical cluster

``` r
dfcars <- mtcars %>%
  rownames_to_column() %>% #add a column name instead of a rowname to car names
  as_tibble() %>% #save as tibble
  select(car = rowname, mpg:hp, wt, qsec, gear, carb) %>% #choosing specific columns
  mutate_at(vars(-car), scale) %>% #make all <dlb> type columns have same scale using z-score
  print()
```

    ## # A tibble: 32 × 9
    ##    car       mpg[,1] cyl[,1] disp[,1] hp[,1]   wt[,1] qsec[,1] gear[,1] carb[,1]
    ##    <chr>       <dbl>   <dbl>    <dbl>  <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
    ##  1 Mazda RX4   0.151  -0.105  -0.571  -0.535 -0.610     -0.777    0.424    0.735
    ##  2 Mazda RX…   0.151  -0.105  -0.571  -0.535 -0.350     -0.464    0.424    0.735
    ##  3 Datsun 7…   0.450  -1.22   -0.990  -0.783 -0.917      0.426    0.424   -1.12 
    ##  4 Hornet 4…   0.217  -0.105   0.220  -0.535 -0.00230    0.890   -0.932   -1.12 
    ##  5 Hornet S…  -0.231   1.01    1.04    0.413  0.228     -0.464   -0.932   -0.503
    ##  6 Valiant    -0.330  -0.105  -0.0462 -0.608  0.248      1.33    -0.932   -1.12 
    ##  7 Duster 3…  -0.961   1.01    1.04    1.43   0.361     -1.12    -0.932    0.735
    ##  8 Merc 240D   0.715  -1.22   -0.678  -1.24  -0.0278     1.20     0.424   -0.503
    ##  9 Merc 230    0.450  -1.22   -0.726  -0.754 -0.0687     2.83     0.424   -0.503
    ## 10 Merc 280   -0.148  -0.105  -0.509  -0.345  0.228      0.253    0.424    0.735
    ## # ℹ 22 more rows

Calculating

``` r
hc <- dfcars %>%
  dist %>% #distance
  hclust #agglomerative, analyze single then group
```

    ## Warning in dist(.): NAs introduced by coercion

Plotting dendogram

``` r
invisible(dev.hold()) #keep the graphics device open
#plot the car names
hc %>% plot(labels = dfcars$car, 
           cex = 0.8, #label size
           hang = -1) #bottom alignment

#draw boxes
hc %>% rect.hclust(k = 5, #k = number of groups
                   border = 2:6) #border color, colors 2-6 from color palette
```

![](mtcars-data-anaysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
invisible(dev.flush()) #close the graphics device
```
