## Boxplot

Previously, we learned how to draw histograms and density plots. When visualizing 
the distribution of a continuous variable, there's yet another type of plot we 
can use, namely, the boxplot. A boxplot shows the minimum, lower quartile 
(25th percentile), median, mean, upper quartile (75th percentile) and maximum.
It also shows outliers that are distributed far away from the majority of data 
points. Let's start with an example by making a boxplot for `budget`.

A>
```r
library(ezplot)
plt = mk_boxplot(films)
p = plt(yvar = "budget")
p = add_labs(p, ylab = "budget ($)", title = "Distribution of Budget") 
scale_axis(p, axis = "y", scale = "log10") # use log10 scale on y-axis
```

![Distribution of Budget](images/boxplot_budget-1.png)

Notice we didn't specify `xvar` inside the function `plt()`. This is because 
the default value for `xvar` is "1" and this already allows us to visualize 
the overall `budget` values. The number 5944 is the total number of 
observations. If we want to show `budget` distribution by a categorical 
variable, we can supply a value to `xvar`. Let's do an example. 
The films were made between 1913 and 2014, and it'll be interesting to see how 
budget changed over the years. To find out, we can draw a boxplot of `budget` 
over `year_cat`. 

A>
```r
p = plt(xvar = "year_cat", yvar = "budget")
scale_axis(p, "y", scale = "dollar") # apply dollar scale to y-axis 
```

![Distribution of Budget Over the Years](images/boxplot_bt_vs_year_cat_p1-1.png)

We see budget has increased over the years. Also, there are 231 films released 
between 1913 and 1950, and 4594 films released between 1990 and 2014. Yes, 
ezplot is smart enough to tally these numbers and display them at the top of 
each boxplot. We can choose not to show them by setting `label_size = 0` inside
of `plt()`. Try it.  

But the x variable must be character or factor type, and cannot be integer or 
numeric. For example, if we try to plot `budget` by `year`,  it'll throw an 
error because `year` is integer type.

A>
```r
plt(xvar = "year", yvar = "budget") # throws error since "year" is integer
```
A>
```
Error in plt(xvar = "year", yvar = "budget"): The x variable, year, is integer or numeric. Change to factor or character.
```

To make it work, we can change `year` to factor first. 

A>
```r
library(dplyr)
plt = mk_boxplot(films %>% filter(year %in% 2010:2014) %>%
                         mutate(year = factor(year)))
plt("year", "budget", notched = T) # draw notched boxplot
```

![Distribution of Budget 2010 to 2014](images/boxplot_bt_vs_year_p1-1.png)

Notice the "notches" or narrowing of the box around the median. We did that by 
setting `notched = T` inside of `plt()`. Notches are useful in offering a rough 
guide to significance of difference of medians; if the notches of two boxes do 
not overlap, this offers evidence of a statistically significant difference 
between the medians.

In addtion to `xvar` and `yvar`, we can also supply a `fillby` varname to
draw boxplot for separate groups. For example, we can show the distribution 
of users' average ratings for profitable and unprofitable films at each period.

A>
```r
plt = mk_boxplot(films)
plt("year_cat", "rating", fillby = "made_money", legend_title = "Is profitable?",
    legend_pos = "top", notched = T)
```

![Distribution of Avg Ratings, profitable vs. unprofitable films.](images/boxplot_rating_vs_year_cat_by_made_money-1.png)

For homework, try the following exercises.

1. Read the document of `mk_boxplot()` and run the examples. You can pull up the 
document by running `?mk_boxplot` in Rstudio. 
2. Draw a plot to show how the distributions of `boxoffice` change over the years.
3. Draw a plot to show how the distributions of `rating` change over the years.
4. Draw a plot to show how the distributions of `length` change over the years.
5. The `films` dataset has a variable `mpaa` that has the MPAA rating of each 
film. What type of a variable is it? Can you draw a plot to show the 
distribution of `boxoffice` at each MPAA rating?
6. The `films` dataset has a bunch of variables that record the genres, 
for example, `action`, `comedy`, and etc. Can you draw a plot to show the 
distribution of `boxoffice` for each genre?
