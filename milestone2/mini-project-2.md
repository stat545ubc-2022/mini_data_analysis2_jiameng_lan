Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
flow_sample
```

    ## # A tibble: 218 × 7
    ##    station_id  year extreme_type month   day  flow sym  
    ##    <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr>
    ##  1 05BB001     1909 maximum          7     7   314 <NA> 
    ##  2 05BB001     1910 maximum          6    12   230 <NA> 
    ##  3 05BB001     1911 maximum          6    14   264 <NA> 
    ##  4 05BB001     1912 maximum          8    25   174 <NA> 
    ##  5 05BB001     1913 maximum          6    11   232 <NA> 
    ##  6 05BB001     1914 maximum          6    18   214 <NA> 
    ##  7 05BB001     1915 maximum          6    27   236 <NA> 
    ##  8 05BB001     1916 maximum          6    20   309 <NA> 
    ##  9 05BB001     1917 maximum          6    17   174 <NA> 
    ## 10 05BB001     1918 maximum          6    15   345 <NA> 
    ## # … with 208 more rows

``` r
glimpse(flow_sample)
```

    ## Rows: 218
    ## Columns: 7
    ## $ station_id   <chr> "05BB001", "05BB001", "05BB001", "05BB001", "05BB001", "0…
    ## $ year         <dbl> 1909, 1910, 1911, 1912, 1913, 1914, 1915, 1916, 1917, 191…
    ## $ extreme_type <chr> "maximum", "maximum", "maximum", "maximum", "maximum", "m…
    ## $ month        <dbl> 7, 6, 6, 8, 6, 6, 6, 6, 6, 6, 6, 7, 6, 6, 6, 7, 5, 7, 6, …
    ## $ day          <dbl> 7, 12, 14, 25, 11, 18, 27, 20, 17, 15, 22, 3, 9, 5, 14, 5…
    ## $ flow         <dbl> 314, 230, 264, 174, 232, 214, 236, 309, 174, 345, 185, 24…
    ## $ sym          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…

My data, flow_sample, is tidy based on the definition of tidy data. The
flow_sample data fits all the three row - observation, column -
variable, cell - value.
<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

As mentioned above, I will untidy the data by generating more column
based on extreme_type and change it back to the original state.

``` r
untiflow <- flow_sample %>%
    pivot_wider(id_cols = c(station_id, year, month, day, sym), 
                names_from = extreme_type,
                values_from = flow)
print(untiflow)
```

    ## # A tibble: 218 × 7
    ##    station_id  year month   day sym   maximum minimum
    ##    <chr>      <dbl> <dbl> <dbl> <chr>   <dbl>   <dbl>
    ##  1 05BB001     1909     7     7 <NA>      314      NA
    ##  2 05BB001     1910     6    12 <NA>      230      NA
    ##  3 05BB001     1911     6    14 <NA>      264      NA
    ##  4 05BB001     1912     8    25 <NA>      174      NA
    ##  5 05BB001     1913     6    11 <NA>      232      NA
    ##  6 05BB001     1914     6    18 <NA>      214      NA
    ##  7 05BB001     1915     6    27 <NA>      236      NA
    ##  8 05BB001     1916     6    20 <NA>      309      NA
    ##  9 05BB001     1917     6    17 <NA>      174      NA
    ## 10 05BB001     1918     6    15 <NA>      345      NA
    ## # … with 208 more rows

``` r
tiflow <- untiflow %>%
  pivot_longer(cols = c(-station_id, -year, -month, -day, -sym),
              names_to = "extreme_type",
              values_to = "flow")
print(tiflow)
```

    ## # A tibble: 436 × 7
    ##    station_id  year month   day sym   extreme_type  flow
    ##    <chr>      <dbl> <dbl> <dbl> <chr> <chr>        <dbl>
    ##  1 05BB001     1909     7     7 <NA>  maximum        314
    ##  2 05BB001     1909     7     7 <NA>  minimum         NA
    ##  3 05BB001     1910     6    12 <NA>  maximum        230
    ##  4 05BB001     1910     6    12 <NA>  minimum         NA
    ##  5 05BB001     1911     6    14 <NA>  maximum        264
    ##  6 05BB001     1911     6    14 <NA>  minimum         NA
    ##  7 05BB001     1912     8    25 <NA>  maximum        174
    ##  8 05BB001     1912     8    25 <NA>  minimum         NA
    ##  9 05BB001     1913     6    11 <NA>  maximum        232
    ## 10 05BB001     1913     6    11 <NA>  minimum         NA
    ## # … with 426 more rows

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *to investigate the distribution of the flow data over the months
    (or over time)*
2.  *to investigate the flow value in a particular month, June, over the
    years*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

The second question is modified, more specific version of the research
question in Milestone 1. In the Milestone 1, the plots I made for the
related two research questions have not been clearly generated, like the
order of x axis. Besides, I want to generate flow distribution plot for
each month.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

The version of data I choose is research1.1. In Milestone 1 , the rest
data were all generated based on research1.1. In flowdate data, I
combined the “year”, “month”, “day” together as the “date”, which can
provide a better view of flow distribution over time. In overmonth data,
I arrange the reserach1.1 by month and remove NA row and remove
irrelevant columns. In juneonly data, only June flow data and
corresponding year have been kept.

``` r
flowdate <- flow_sample%>%
  unite(date, year, month, day, sep = "-") %>% 
  relocate(date, .after = extreme_type)
print(flowdate)
```

    ## # A tibble: 218 × 5
    ##    station_id extreme_type date       flow sym  
    ##    <chr>      <chr>        <chr>     <dbl> <chr>
    ##  1 05BB001    maximum      1909-7-7    314 <NA> 
    ##  2 05BB001    maximum      1910-6-12   230 <NA> 
    ##  3 05BB001    maximum      1911-6-14   264 <NA> 
    ##  4 05BB001    maximum      1912-8-25   174 <NA> 
    ##  5 05BB001    maximum      1913-6-11   232 <NA> 
    ##  6 05BB001    maximum      1914-6-18   214 <NA> 
    ##  7 05BB001    maximum      1915-6-27   236 <NA> 
    ##  8 05BB001    maximum      1916-6-20   309 <NA> 
    ##  9 05BB001    maximum      1917-6-17   174 <NA> 
    ## 10 05BB001    maximum      1918-6-15   345 <NA> 
    ## # … with 208 more rows

``` r
research1.1 <- flow_sample %>% mutate(montha = month.abb[as.numeric(month)])

overmonth <- arrange(research1.1, month) %>% filter(!is.na(month)) %>%
  select(-starts_with("s"))
print(overmonth)
```

    ## # A tibble: 216 × 6
    ##     year extreme_type month   day  flow montha
    ##    <dbl> <chr>        <dbl> <dbl> <dbl> <chr> 
    ##  1  1915 minimum          1    27  6.94 Jan   
    ##  2  1932 minimum          1     5  3.62 Jan   
    ##  3  1948 minimum          1    27  8.41 Jan   
    ##  4  1959 minimum          1     3  6.68 Jan   
    ##  5  1961 minimum          1    19  7.59 Jan   
    ##  6  1969 minimum          1    31  7.5  Jan   
    ##  7  1973 minimum          1     4  6.71 Jan   
    ##  8  1978 minimum          1    30  5.75 Jan   
    ##  9  1982 minimum          1    16  6.15 Jan   
    ## 10  1988 minimum          1     5  5.11 Jan   
    ## # … with 206 more rows

``` r
juneonly <- filter(research1.1, month == 6)
juneonly1 <- juneonly %>% select(year, flow)
flowinjune <- juneonly1 %>% ggplot() +
  geom_col(mapping = aes(year, flow))
print(flowinjune)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
monthdis <- overmonth %>% ggplot() +
  geom_col(mapping = aes(year, flow)) +
  facet_wrap(vars(montha))
print(monthdis)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        - Note that you might first have to *make* a time-based column
          using a function like `ymd()`, but this doesn’t count.
        - Examples of something you might do here: extract the day of
          the year from a date, or extract the weekday, or let 24 hours
          elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        - For example, you could say something like “Investigating the
          day of the week might be insightful because penguins don’t
          work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 1 I reorder the “montha” variable. I choose the
ordering as general month order. In this way, I can tell flow change
over the months.

``` r
library(forcats)
monthdis1 <- overmonth %>% mutate(montha = fct_reorder(montha, month)) %>%
  ggplot() +
  geom_col(mapping = aes(year, flow)) +
  facet_wrap(vars(montha))
print(monthdis1)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2 I chose to group the month has less than 10 flow
values recorded as “other” group, because the more flow values tends to
provide more accurate flow change over the years. In this case, the only
August flow value would be grouped into “other”, which we won’t study
the flow change in August over the years.

``` r
monthdis2 <- overmonth %>% mutate(montha = fct_reorder(montha, month)) %>%
  mutate(montha = fct_lump_min(montha, min = 10, other_level = "other")) %>%
  ggplot() +
  geom_col(mapping = aes(year, flow)) +
  facet_wrap(vars(montha))
print(monthdis2)
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: to investigate the flow value in a particular
month, June, over the years

**Variable of interest**: flow

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
predictmodel <- lm(flow ~ year, overmonth)
summary(predictmodel)
```

    ## 
    ## Call:
    ## lm(formula = flow ~ year, data = overmonth)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -117.03 -102.97    8.45   92.11  367.80 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) 583.1516   479.7697   1.215    0.226
    ## year         -0.2409     0.2443  -0.986    0.325
    ## 
    ## Residual standard error: 112 on 214 degrees of freedom
    ## Multiple R-squared:  0.004523,   Adjusted R-squared:  -0.0001287 
    ## F-statistic: 0.9723 on 1 and 214 DF,  p-value: 0.3252

``` r
onew <- aov(flow ~ year, data = overmonth)
summary(onew)
```

    ##              Df  Sum Sq Mean Sq F value Pr(>F)
    ## year          1   12203   12203   0.972  0.325
    ## Residuals   214 2685706   12550

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I choose to produce a p-value and output a tibble by using broom
package.

``` r
modelvalue <- broom::tidy(predictmodel)
print(modelvalue)
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 (Intercept)  583.      480.        1.22    0.226
    ## 2 year          -0.241     0.244    -0.986   0.325

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
research4.1.2 <- research1.1 %>% 
  group_by(extreme_type, montha) %>%
  summarise(flow_var = var(flow, na.rm = TRUE), flow_mean = mean(flow, na.rm = TRUE), flow_max = max(flow, na.rm = TRUE), flow_median = median(flow, na.rm = TRUE))
```

    ## Warning in max(flow, na.rm = TRUE): no non-missing arguments to max; returning
    ## -Inf

    ## `summarise()` has grouped output by 'extreme_type'. You can override using the
    ## `.groups` argument.

``` r
print(research4.1.2)
```

    ## # A tibble: 11 × 6
    ## # Groups:   extreme_type [2]
    ##    extreme_type montha flow_var flow_mean flow_max flow_median
    ##    <chr>        <chr>     <dbl>     <dbl>    <dbl>       <dbl>
    ##  1 maximum      Aug      NA        174      174         174   
    ##  2 maximum      Jul    3234.       200.     314         182   
    ##  3 maximum      Jun    4279.       218.     466         213   
    ##  4 maximum      May    1750.       194.     289         190.  
    ##  5 minimum      Apr       0.694      6.17     7.53        6.16
    ##  6 minimum      Dec       0.390      6.19     7.88        6.09
    ##  7 minimum      Feb       0.914      6.07     7.98        6.02
    ##  8 minimum      Jan       1.48       6.50     8.41        6.68
    ##  9 minimum      Mar       1.06       6.42     8.44        6.39
    ## 10 minimum      Nov       0.500      6.11     7.16        6.14
    ## 11 minimum      <NA>     NA        NaN     -Inf          NA

``` r
dir.create(here::here("output"))
```

    ## Warning in dir.create(here::here("output")): '/Users/jiamenglan/Desktop/stat545/
    ## mini_data_analysis2_jiameng_lan/output' already exists

``` r
write_csv(research4.1.2, here::here("output", "task4_2_milestone1.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 3.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(predictmodel, here::here("output", "task3_milestone2.RDS"))
modelrds <- readRDS(here::here("output","task3_milestone2.RDS"))
print(modelrds)
```

    ## 
    ## Call:
    ## lm(formula = flow ~ year, data = overmonth)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##    583.1516      -0.2409

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

- In a sentence or two, explains what this repository is, so that
  future-you or someone else stumbling on your repository can be
  oriented to the repository.
- In a sentence or two (or more??), briefly explains how to engage with
  the repository. You can assume the person reading knows the material
  from STAT 545A. Basically, if a visitor to your repository wants to
  explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output, and all data files
  saved from Task 4 above appear in the `output` folder.
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
