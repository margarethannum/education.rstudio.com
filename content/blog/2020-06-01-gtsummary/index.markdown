---
title: Presentation-Ready Summary Tables
author:
  - '[Daniel Sjoberg](http://www.danieldsjoberg.com/)'
  - '[Margie Hannum](https://margarethannum.github.io/about.html)'
  - '[Karissa Whiting](https://github.com/karissawhiting)'
date: '2020-06-01'
categories:
  - learn
tags:
  - rmarkdown
description: |
  The {gtsummary} package summarizes data sets, regression models, and more using sensible defaults with highly customizable capabilities, and is compatible with multiple R Markdown output types.
slug: gtsummary
photo:
  url: https://unsplash.com/photos/hk2gSD_S5j0
  author: Daniel von Appen
---



We are thrilled to introduce you to the [{gtsummary} package](http://www.danieldsjoberg.com/gtsummary/)!<a href='https://github.com/ddsjoberg/gtsummary'><img src='logo.png' align="right" height="200" /></a>

The {gtsummary} package provides an elegant and flexible way to create publication-ready analytical and summary tables in R.

  The motivation behind the package stems from our work as statisticians, where every day we summarize datasets and regression models in R, share these results with collaborators, and eventually include them in published manuscripts. Many of our colleagues had our own scripts to create the tables we needed, and even then would often need to modify the formatting in a document editor later, which did not lead to **reproducible results**.

  At the time we created the package, we had several ideas in mind for our ideal table summary package. We also wanted our tables to be able to take advantage of all the features in RStudio's newly released [{gt}](https://gt.rstudio.com/) package, which offers a variety of table customization options like spanning column headers, table footnotes, stubhead label, row group labels and more. So, {gtsummary} was born! 
  
  Here's what you can do with {gtsummary}:

- [**Summarize data frames or tibbles**](http://www.danieldsjoberg.com/gtsummary/articles/tbl_summary.html) to present descriptive statistics, compare group demographics (e.g creating a Table 1 for medical journals), and more!
- [**Summarize regression models**](http://www.danieldsjoberg.com/gtsummary/articles/tbl_regression.html). Using `broom::tidy()` in the background, {gtsummary} plays nicely with many model types (lm, glm, coxph, glmer etc.). You may also use custom functions to summarize regression models that do not currently have [{broom}](https://broom.tidymodels.org/index.html) tidiers.
- [**Customize gtsummary tables**](http://www.danieldsjoberg.com/gtsummary/reference/index.html#section-general-formatting-styling-functions) using a growing list of formatting/styling functions: everything from which statistics and tests to use to how many decimal places to round to, bolding labels, indenting categories and more!
- [**Report statistics inline**](http://www.danieldsjoberg.com/gtsummary/articles/tbl_summary.html#inline_text) from summary tables and regression summary tables in **R markdown**. Make your reports completely reproducible! 
- [**Leverage compatibility with multiple R Markdown outputs**](http://www.danieldsjoberg.com/gtsummary/articles/rmarkdown.html) to create beautiful, reproducible reports in a variety of formats (HTML, PDF, Word, RTF)


Install {gtsummary} from CRAN with the following code: 


```r
install.packages("gtsummary")  
```

## Summarize descriptive statistics

Throughout the post we will use an example dataset of 200 subjects treated with either Drug A or Drug B, with a mix of categorical, dichotomous, and continuous demographic and response data. The dataset has label attributes (using the [{labelled}](http://larmarange.github.io/labelled/) package) for column names. 


```r
sm_trial <- trial %>% select(trt, age, response, grade)

head(sm_trial)
```

<!--html_preserve--><table class="huxtable" style="border-collapse: collapse; margin-bottom: 2em; margin-top: 2em; width: 20%; margin-left: 0%; margin-right: auto;  ">
<col><col><col><col><tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0.4pt 0pt 0.4pt 0.4pt; padding: 4pt 4pt 4pt 4pt; font-weight: bold;">trt</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; border-style: solid solid solid solid; border-width: 0.4pt 0pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt; font-weight: bold;">age</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; border-style: solid solid solid solid; border-width: 0.4pt 0pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt; font-weight: bold;">response</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0.4pt 0.4pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt; font-weight: bold;">grade</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0pt 0.4pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">Drug A</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">23</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">0</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0pt 0pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">II</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0pt 0.4pt; padding: 4pt 4pt 4pt 4pt;">Drug B</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt;">9</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt;">1</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0pt 0pt; padding: 4pt 4pt 4pt 4pt;">I</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0pt 0.4pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">Drug A</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">31</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">0</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0pt 0pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">II</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0pt 0.4pt; padding: 4pt 4pt 4pt 4pt;">Drug A</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt;"></td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt;">1</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0pt 0pt; padding: 4pt 4pt 4pt 4pt;">III</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0pt 0.4pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">Drug A</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">51</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">1</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0pt 0pt; padding: 4pt 4pt 4pt 4pt; background-color: rgb(242, 242, 242);">III</td>
</tr>
<tr>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0.4pt 0.4pt; padding: 4pt 4pt 4pt 4pt;">Drug B</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt;">39</td>
<td style="vertical-align: top; text-align: right; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt;">0</td>
<td style="vertical-align: top; text-align: left; white-space: nowrap; border-style: solid solid solid solid; border-width: 0pt 0.4pt 0.4pt 0pt; padding: 4pt 4pt 4pt 4pt;">I</td>
</tr>
</table>
<!--/html_preserve-->


In **one line of code** we can summarize the overall demographics of the dataset!

Notice some nice default behaviors:  
  ♥ Detects variable types of input data and calculates descriptive statistics  
  ♥ Variables coded as 0/1, TRUE/FALSE, and Yes/No are presented dichotomously  
  ♥ Recognizes `NA` values as "missing" and lists them as unknown  
  ♥ Label attributes automatically printed  
  ♥ Variable levels indented and footnotes added 


```r
tbl_summary_1 <- tbl_summary(sm_trial)
```

<p align="center"><img src="tbl_summary_1.png" width=30%></p>



**Start customizing by adding arguments and functions**

Next you can start to customize the table by using arguments of the `tbl_summary()` function, as well as pipe the table through additional {gtsummary} functions to add more information, like p-value to compare across groups and overall demographic column.  


```r
tbl_summary_2 <- sm_trial %>%
  tbl_summary(by = trt) %>%
  add_p() %>%
  add_overall() %>% 
  bold_labels()
```

<p align="center"><img src="tbl_summary_2.png" width=60%></p>

**Customize further using formula syntax and tidy selectors**

Most arguments to `tbl_summary()` and `tbl_regression()` require formula syntax:

> select variables ~ specify what you want to do

* To select, use quoted or unquoted variables, or minus sign to negate (e.g. `age` or `"age"` to select, `-age` to deselect)
* Or use any {tidyselect} functions, e.g. `contains("stage") ~ ...`, including type selectors 
* To specify what you want to do, some arguments use [{glue}](https://github.com/tidyverse/glue) syntax where whatever is in the curly brackets gets evaluated and passed directly into the string. e.g `statistic = ... ~ "{mean} ({sd})"`


```r
tbl_summary_3 <- sm_trial %>%
  tbl_summary(
    by = trt,
    statistic = list(
      all_continuous() ~ "{mean} ({sd})",
      all_categorical() ~ "{n} / {N} ({p}%)"), 
    label = age ~ "Patient Age") %>%
  add_p(test = all_continuous() ~ "t.test",
        pvalue_fun = function(x) style_pvalue(x, digits = 2))
```

<p align="center"><img src="tbl_summary_3.png" width=50%></p>



## Summarize regression models

First, create a logistic regression model to use in examples. 


```r
m1 <- glm(response ~ trt + grade + age, 
          data = trial,
          family = binomial) 
```


`tbl_regression()` accepts regression model object as input. Uses {broom} in the background, outputs table with nice defaults:

  ♥ Reference groups added to the table  
  ♥ Sensible default number rounding and formatting  
  ♥ Label attributes printed  
  ♥ Common model types detected and appropriate header added with footnote
  

```r
tbl_reg_1 <- tbl_regression(m1, exponentiate = TRUE)
```


<p align="center"><img src="tbl_regression_1.png" width=40%></p>

## Join two or more tables

Oftentimes we must present results for multiple outcomes of interest, and there are many other reasons you might want to join two summary tables together. We've got you covered!

In this example we can use `tbl_merge()` to merge two gtsummary objects side-by-side. There is also a `tbl_stack()` function to place tables on top of each other. 


```r
library(survival)

tbl_reg_3 <- 
  coxph(Surv(ttdeath, death) ~ trt + grade + age, 
        data = trial) %>%
  tbl_regression(exponentiate = TRUE)

tbl_reg_4 <-
  tbl_merge(
    tbls = list(tbl_reg_1, tbl_reg_3), 
    tab_spanner = c("**Tumor Response**", "**Time to Death**") 
  ) 
```


<p align="center"><img src="tbl_regression_4.png" width=60%></p>

## Report results inline

Tables are important, but we often need to report results in-line in a report. Any statistic reported in a {gtsummary} table can be extracted and reported in-line in a R Markdown document with the `inline_text()` function.

`inline_text(tbl_reg_1, variable = trt, level = "Drug B")`

1.13 (95% CI 0.60, 2.13; p=0.7)

- The pattern of what is reported can be modified with the `pattern = ` argument.  

- Default is `pattern = "{estimate} ({conf.level*100}% CI {conf.low}, {conf.high}; {p.value})"`.

## gtsummary + R Markdown

The **{gtsummary}** package was written to be a companion to the **{gt}** package from RStudio.
But not all output types are supported by the **{gt}** package (yet!).
Therefore, we have made it possible to print **{gtsummary}** tables with various engines.

Review the **[gtsummary + R Markdown](http://www.danieldsjoberg.com/gtsummary/articles/rmarkdown.html)** vignette for details.

<a href="http://www.danieldsjoberg.com/gtsummary/articles/rmarkdown.html">
<img src="gt_output_formats.PNG" width="55%" style="display: block; margin: auto;" />
</a>

## Using external customization functions

It's natural a {gtsummary} package user would want to customize the aesthetics of the table with some of the many functions available in the print engines listed above. 

Once you convert a {gtsummary} object to another kind of object (e.g. {gt}), every function compatible that object will be available to use! 

Example workflow and code using {gt} customization:

1. Create a {gtsummary} table.  
1. Convert the table to a {gt} object with the [`as_gt()`](http://www.danieldsjoberg.com/gtsummary/reference/as_gt.html) function. (Also available: [`as_flextable()`](http://www.danieldsjoberg.com/gtsummary/reference/as_flextable.html), [`as_kable_extra()`](http://www.danieldsjoberg.com/gtsummary/reference/as_kable_extra.html)...  
1. Continue formatting as a {gt} table with any [{gt}](https://gt.rstudio.com/) function. 
    

```r
tbl_summary_5 <- sm_trial %>%
  # create a gtsummary table
  tbl_summary(by = trt) %>%
  # convert from gtsummary object to gt object
  as_gt() %>%
  # modify with gt functions
  gt::tab_header("Table 1: Baseline Characteristics") %>% 
  gt::tab_spanner(
    label = "Randomization Group",  
    columns = starts_with("stat_")
  ) %>% 
  gt::tab_options(
    table.font.size = "small",
    data_row.padding = gt::px(1)) 
```


<p align="center"><img src="tbl_summary_5.png" width=50%></p>



## Additional features

There are a few other functions we'd like you to know about! 

- [`tbl_uvregression()`](http://www.danieldsjoberg.com/gtsummary/reference/tbl_uvregression.html) to make tables summarizing univariable regression models (input a dataset, specify model and parameters... make a UVA regression table easily!)
- [`tbl_survfit()`](http://www.danieldsjoberg.com/gtsummary/reference/tbl_survfit.html) function to summarize survival models
- [`tbl_crossfit()`](http://www.danieldsjoberg.com/gtsummary/reference/tbl_cross.html) to make beautiful cross tables
- Use [themes](http://www.danieldsjoberg.com/gtsummary/dev/articles/themes.html) to format your table for specific journal/aesthetic requirements! 

**Additional customization options**

See the full list of gtsummary functions [here](http://www.danieldsjoberg.com/gtsummary/reference/index.html). You can use them to do all sorts of things to your tables, like:

- Use **custom functions** for calculating p-values and reporting any statistic for continuous variables (including user-written functions)
- Bold and italicize labels and levels
- Present **missing data** in various ways
- **Sort variables** by significance (`sort_p()`); sort categorical variables by frequency  
- Calculate **cell percents and row percents** (default is column-wide)  
- Only report p-values for select variables (`add_p(include = ...)`); report q-values (like false discovery rate)  
- Set global **rounding options** with themes (for estimates, confidence intervals, and p-values)

There is a growing [gallery of tables](http://www.danieldsjoberg.com/gtsummary/articles/gallery.html) which highlights some of the many customization options! 

## Wrap-up

The {gtsummary} package website contains [well-documented functions](http://www.danieldsjoberg.com/gtsummary/reference/index.html), detailed [tutorials](http://www.danieldsjoberg.com/gtsummary/articles/), and [examples](http://www.danieldsjoberg.com/gtsummary/articles/gallery.html)! 

If you have any questions on usage, please post to StackOverflow and use the [gtsummary tag](https://stackoverflow.com/questions/tagged/gtsummary?tab=Newest). We try to answer questions ASAP! You can also report bugs or make feature requests by submitting an issue on [GitHub](https://github.com/ddsjoberg/gtsummary/issues). 

May your code be short, your tables beautiful, and your reports fully reproducible! 

Happy coding!

<iframe src="https://giphy.com/embed/13AcmSNW5O7WV2" width="480" height="426" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/post-how-tenorgif-13AcmSNW5O7WV2">via GIPHY</a></p>
