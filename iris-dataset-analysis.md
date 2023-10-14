iris-dataset-analysis
================
Adrian M
2023-10-14

``` r
# install.packages("tidyverse")
# install.packages("ggplot2")
# library(tidyverse)
library(ggplot2)
```

Copying iris data set to variable ‘df’

``` r
df <- iris
```

A quick view of the dataset

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

?qplot

Using qplot to show a bar plot, `fill` to color the bars. Check the
number of records per Species

``` r
qplot(Species, 
      fill = Species,
      data = df)
```

    ## Warning: `qplot()` was deprecated in ggplot2 3.4.0.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

qplot automatically decides which plot to use based on supplied
variables, looks like. 50 records per species.

Trying qplot if it will show a scatterplot

``` r
qplot(Petal.Length, 
      Petal.Width, 
      data = df)
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Add some colors by Species

``` r
qplot(Petal.Length, 
      Petal.Width, 
      color = Species,
      data = df)
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Looks good but documentation says qplot is deprecated (no longer useful
or better options exist) So, going to try and explore ggplot instead

?ggplot

``` r
ggplot(data = df, 
       mapping = aes(x = Petal.Length, 
                     y = Petal.Width))
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

It’s blank. Of course since the `aes()` or aesthetic is only present
here. I need to specify which type of plot to use (unlike qplot?)

``` r
ggplot(data = df, 
       mapping = aes(x = Petal.Length, 
                     y = Petal.Width)) +
  geom_point()
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Pros: more control over what you want to display versus qplot which does
it automatically. Also, it says we can drop the specifications for the
`data=` and `mapping=` arguments as long as we supply the correct
arguments in the correct order. Nice. Add some colors

``` r
ggplot(df,
       aes(x = Petal.Length,
           y = Petal.Width,
           col = Species)) +
  geom_point()
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Maybe I can drop the `x=` and `y=` specifications too for the `aes()`
method?

?aes() help(aes)

Also, ggplot uses layers so it can overlap multiple plots, from one
dataset or more(!). I’ll to move the `aes()` inside the `geom_point`
layer and try some parameters

``` r
ggplot(df) +
  geom_point(aes(Petal.Length,
                 Petal.Width,
                 col = Species,   #color by species 
                 shape = Species, #shape by species
                 size = 3,       #size of datapoints
                 stroke = 1))     #thickness of borders of datapoints, specific to geom_point
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Try to overlap two layers, petal and sepal

``` r
ggplot(df) +
  geom_point(aes(Petal.Length,
             Petal.Width,
             col = Species)) +
  geom_point(aes(Sepal.Length,
             Sepal.Width,
             col = Species))
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

I can’t distinguish which is which. Maybe try to change the color per
variable

``` r
ggplot(df) +
  geom_point(aes(Petal.Length,
                 Petal.Width,
                 shape = Species), 
             col = "Red") +        #put the col= argument outside aes()
  geom_point(aes(Sepal.Length,
                 Sepal.Width,
                 shape = Species),
             col = "Blue")
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

This look too busy and unusable but I can move forward and explore
ggplot further.

Try to see some relation between the measurements (petal, sepal, length
and width) but this time, show the plots side by side using cowplot
package

install.packages(“cowplot”)

``` r
library(cowplot)
```

    ## Warning: package 'cowplot' was built under R version 4.3.1

``` r
gg1 <- ggplot(df, aes(col = Species)) +
  geom_point(aes(Petal.Length,
                 Petal.Width), 
             size = 2, 
             alpha = 0.7,
             show.legend = FALSE)

gg2 <- ggplot(df, aes(col = Species)) +
  geom_point(aes(Sepal.Length,
                 Sepal.Width), 
             size = 2, 
             alpha = 0.7) +
  theme(legend.position = "top",
        legend.title = element_blank())

plot_grid(gg1, gg2, ncol = 2) #from cowplot
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

Looks crowded but I can see here that Petal Length & Width have positive
correlation while Sepal length & width have positive correlation only
with setosa

OK I’m going to try histogram plots. Some basic plots to see the
distribution of all the measurements

``` r
#I made a function since this will repeat 4 times (petal & sepal, width & length)
create_histogram <- function(data, column_name, num_bins = 25) #create the function along with the accepted parameters
  ggplot(data) +
  geom_histogram(aes_string(column_name), 
                 bins = num_bins,
                 alpha = 0.7) + #flexibility in number of histogram bins
  theme_minimal() #makes all plots consistent

#Running the function, supplying the appropriate parameters, then storing each plot to a corresponding variable
hist_petal_length <- create_histogram(df, "Petal.Length")
```

    ## Warning: `aes_string()` was deprecated in ggplot2 3.0.0.
    ## ℹ Please use tidy evaluation idioms with `aes()`.
    ## ℹ See also `vignette("ggplot2-in-packages")` for more information.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
hist_petal_width <- create_histogram(df, "Petal.Width")
hist_sepal_length <- create_histogram(df, "Sepal.Length")
hist_sepal_width <- create_histogram(df, "Sepal.Width")

#Now plot the 4 in a 2x2 grid using plot_grid from cowplot
plot_grid(hist_petal_length, 
          hist_petal_width, 
          hist_sepal_length, 
          hist_sepal_width, 
          ncol = 2, 
          nrow = 2)
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
The petal length & width appears to have a gap in a certain section of
the distribution. The group on the left could be the setosa, similar to
the gaps on the previous scatterplots. The sepal length is spread across
the measurements but the width looks to have a central tendency towards
“3”

I’ll take a closer look at petals using overlapping density plot

``` r
ggplot(df) +
  geom_histogram(aes(x = Petal.Length,
                   y = after_stat(density),
                   fill = Species),
               bins = 30,
               alpha = 0.5,
               position = "identity") + 
  geom_density(aes(x = Petal.Length, 
                   col = Species),
               size = 1.2)
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
Identifying the species by colors confirms that the gap is because of
the iris specie setosa

Can we try using box plots to explore this dataset more?

?geom_boxplot

``` r
#make another function for the boxplot
create_boxplot <- function(data, column_name, fillBy = "Species")
  ggplot(data,
       aes_string(x = fillBy,     
           y = column_name, 
           fill = fillBy)) + 
  geom_boxplot() + 
  theme_minimal() +
  theme(legend.position = "none") +
  coord_flip() #swap the axes to make the display horizontal

gg_boxp_petalL <- create_boxplot(df, "Petal.Length")
gg_boxp_petalW <- create_boxplot(df, "Petal.Width")
gg_boxp_SepalL <- create_boxplot(df, "Sepal.Length")
gg_boxp_SepalW <- create_boxplot(df, "Sepal.Width")

plot_grid(gg_boxp_petalL,
          gg_boxp_petalW,
          gg_boxp_SepalL,
          gg_boxp_SepalW,
          ncol = 2,
          nrow = 2)
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
It looks normally distributed but there are possible outliers. Taking at
closer look at Sepal width

``` r
ggplot(df,
       aes(
         x = Species,
         y = Sepal.Width,
         fill = Species)) +
  geom_boxplot() +
  geom_jitter(position = position_jitter(width = 0.2),
              alpha = 0.7) +
  theme_minimal() +
  coord_flip()
```

![](iris-dataset-analysis_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->
Some data points are clearly outside lines extending outwards the box
but I cannot conclude that these are erroneous data. Since I cannot
gather more information about these “outlier” measurements for this
dataset, I will assume they are correct data for now. This is also true
for the rest of the box plots.

\~tbc..
