
In this section, we are going to explore the dataset, gapminder, in R. This dataset contains information on the life expectancy by country from the years 1952 to 2007, divided into 5 year chunks. There is also information present on GDP and population, for each country.

Let's do some exploration and some summary statistics! But first, let's import the dataset using the library() command:

``` r
library(gapminder)
```

Now, let's calculate some things to understand the dataset even more. Firstly, what is the average life expectancy for the entire world in 1952? Then, let's compare that to the average life expectancy of the world in 2007!

``` r
g <- as.data.frame(gapminder)

old <- subset(g, g[,"year"] == 1952)
new <- subset(g, g[,"year"] == 2007)

mean(old$lifeExp, rm.NA=TRUE)
```

    ## [1] 49.05762

``` r
mean(new$lifeExp, rm.NA=TRUE)
```

    ## [1] 67.00742

As we can see in the above anaylsis. The average life expectancy around the world has dramatically increased. We can do a similar analysis with the average GDP in the world too!

``` r
mean(old$gdpPercap, rm.NA=TRUE)
```

    ## [1] 3725.276

``` r
mean(new$gdpPercap, rm.NA=TRUE)
```

    ## [1] 11680.07

Again, a dramatic increase, which is to be expected.

What if we want to explore the richest and poorest countries in the world? Well, let's look at the most recent year, and determine which one is the richest and which one is the poorest.

We can use the "which.max" and "which.min" functions to help us find those countries. Then, we can print out the name of the country.

``` r
richest <- new[which.max(new$gdpPercap),]
poorest <- new[which.min(new$gdpPercap),]

richest$country
```

    ## [1] Norway
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

``` r
poorest$country
```

    ## [1] Congo, Dem. Rep.
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

So, Norway is the "richest" country in terms of GDP per capita, and Democratic Republic of Congo is the "poorest".

Finally, let's look at population growth. Which country has had the biggest *relative* change in population. That is to say, which country has experience the most population growth between 1952 and 2007? Luckily, we've already subsetting our data frame into 1952 data and 2007 data. Then, we should just look at the percent change amongst all of those countries, then find the largest. We can also find the smallest change too!

``` r
# Begin by reducing our datasets to only country name and population value
old <- old[c("country", "pop")]
new <- new[c("country", "pop")]

# Rename the columns
names(old)[names(old) == 'pop'] <- 'pop_1952'
names(new)[names(new) == 'pop'] <- 'pop_2007'

# Merge the datasets
combined <- merge(old,new)

# Let's look at the combined dataframe to make sure it merged correctly.
head(combined)
```

    ##       country pop_1952 pop_2007
    ## 1 Afghanistan  8425333 31889923
    ## 2     Albania  1282697  3600523
    ## 3     Algeria  9279525 33333216
    ## 4      Angola  4232095 12420476
    ## 5   Argentina 17876956 40301927
    ## 6   Australia  8691212 20434176

``` r
# Calculate percent change.
percent_change <- ((combined$pop_2007 - combined$pop_1952) * 100 / combined$pop_1952)

# Add this as a new column to our datset
combined["percent_change"] <- percent_change

# Let's find the biggest and lowest percent change
biggest <- combined[which.max(percent_change),]
lowest <- combined[which.min(percent_change),]

# Print out the countries.
biggest
```

    ##    country pop_1952 pop_2007 percent_change
    ## 72  Kuwait   160000  2505559       1465.974

``` r
lowest
```

    ##     country pop_1952 pop_2007 percent_change
    ## 16 Bulgaria  7274900  7322858      0.6592256

Based on our analysis, we can see that Kuwait had the biggest percent change, with over a 1400% increase in population. That's an increase of around 14 times in population! Moreover, Bulgaria had the least percent change, with less than 1% increase.

There is so much interesting stuff located in this dataset, and this is only the tip of the iceberg!
