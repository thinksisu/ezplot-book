## Web Display

The plots created so far are too small for web display. If you are making an 
[analytic web app](http://app.cabaceo.com/ocpu/github/gmlang/imdb/www/#/bo), 
you want bigger font size, bigger point size and bigger everything. 
Luckily, the function `web_display()` comes to rescue. Often, all you have to 
do is to feed in the plot object, and it'll return a plot object that just 
looks right when shown in a web browser. Below is an example.

A>
```r
library(ezplot)
plt = mk_scatterplot(films)
p = plt("votes", "boxoffice", alpha = 0.2, jitter = T) %>% 
        add_labs(xlab="number of votes", 
                 ylab="boxoffice (in US Dollars)", 
                 title = "Boxoffice vs. Votes (1913-2014)",
                 caption = "Source: IMDB"
                 )
p = scale_axis(p, "y", scale = "log10") # use log10 scale on y-axis
p = scale_axis(p, "x", scale = "log") # use log scale on x-axis
p = add_lm_line(p, eq_ypos = 0.95, eq_xpos = 0.5) 
web_display(p)
```

![plot of chunk unnamed-chunk-4](images/unnamed-chunk-4-1.png)


              