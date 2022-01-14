---
title: "02 - Code"
date: "2022-01-10"
type: docs
bibliography: "../../../static/bib/references.bib"
csl: "../../../static/bib/chicago-fullnote-bibliography-no-bib.csl"
tags: ["code"]
editor_options: 
  chunk_output_type: console
---

``` r
library(tidyverse)
```

# What R looks like

Code you can type and run:

``` r
## Inside code chunks, lines beginning with a # character are comments
## Comments are ignored by R

my_numbers <- c(1, 1, 2, 4, 1, 3, 1, 5) # Anything after a # character is ignored as well
```

Output:

``` r
my_numbers 
```

    ## [1] 1 1 2 4 1 3 1 5

By convention, code output in documents is prefixed by `##`

``` r
my_numbers 
```

    ## [1] 1 1 2 4 1 3 1 5

Also by convention, outputting vectors, etc, gets a counter keeping track of the number of elements. For example,

``` r
letters
```

    ##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r" "s"
    ## [20] "t" "u" "v" "w" "x" "y" "z"

# It’s a calculator

-   Arithmetic

``` r
(31 * 12) / 2^4
```

    ## [1] 23.25

``` r
sqrt(25)
```

    ## [1] 5

``` r
log(100)
```

    ## [1] 4.60517

``` r
log10(100)
```

    ## [1] 2

-   Logic

``` r
4 < 10
```

    ## [1] TRUE

``` r
4 > 2 & 1 > 0.5 # The "&" means "and"
```

    ## [1] TRUE

``` r
4 < 2 | 1 > 0.5 # The "|" means "or"
```

    ## [1] TRUE

``` r
4 < 2 | 1 < 0.5
```

    ## [1] FALSE

-   Boolean and Logical operators

Logical equality and inequality (yielding a `TRUE` or `FALSE` result) is done with `==` and `!=`. Other logical operators include `<`, `>`, `<=`, `>=`, and `!` for negation.

``` r
## A logical test
2 == 2
```

    ## [1] TRUE

``` r
## This will cause an error, because R will think you are trying to assign a value
# 2 = 2

## Error in 2 = 2 : invalid (do_set) left-hand side to assignment
```

``` r
3 != 7
```

    ## [1] TRUE

## Evaluating logical tests

Here’s a gotcha. You might think you could write `3 < 5 & 7` and have it be interpreted as “Three is less than five and also less than seven \[True or False?\]”:

``` r
3 < 5 & 7
```

    ## [1] TRUE

It seems to work!

But now try `3 < 5 & 1`, where your intention is “Three is less than five and also less than one \[True or False?\]”

``` r
3 < 5 & 1
```

    ## [1] TRUE

What’s happening is that `3 < 5` is evaluated first, and resolves to TRUE, leaving us with the expression `TRUE` `& 1`. R interprets this as `TRUE` `& as.logical(1)`. In Boolean algebra, `1` resolves to TRUE. Any other number is FALSE. So,

``` r
TRUE & as.logical(1)
```

    ## [1] TRUE

``` r
3 < 5 & 3 < 1
```

    ## [1] FALSE

You have to make your comparisons explicit.

# Logic and floating point arithmetic

Let’s evaluate `0.6 + 0.2 == 0.8`

``` r
0.6 + 0.2 == 0.8
```

    ## [1] TRUE

Now let’s try `0.6 + 0.3 == 0.9`

``` r
0.6 + 0.3 == 0.9
```

    ## [1] FALSE

Er. That’s not right.

Welcome to floating point math!

In Base 10, you can’t precisely express fractions like\] 1/3 and 1/9. They come out as repeating decimals: 0.3333… or 0.1111… You *can* cleanly represent fractions that use a prime factor of the base, which in the case of Base 10 are 2 and 5.

Computers represent numbers as binary (i.e. Base 2) floating-points. In Base 2, the only prime factor is 2. So 1/5 or 1/10 in binary would be repeating.

When you do binary math on repeating numbers and convert back to decimals you get tiny leftovers, and this can mess up *logical* comparisons of equality. The `all.equal()` function exists for this purpose.

``` r
print(.1 + .2)
```

    ## [1] 0.3

``` r
print(.1 + .2, digits=18)
```

    ## [1] 0.300000000000000044

``` r
all.equal(.1 + .2, 0.3)
```

    ## [1] TRUE

See e.g. <https://0.30000000000000004.com>

For now, “Be very careful about doing logical comparisons on floating-point numbers” is not a bad rule.

# 1. Everything in R has a name

``` r
my_numbers # We created this a few minutes ago
```

    ## [1] 1 1 2 4 1 3 1 5

``` r
letters  # This one is built-in
```

    ##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r" "s"
    ## [20] "t" "u" "v" "w" "x" "y" "z"

``` r
pi  # Also built-in
```

    ## [1] 3.141593

# Some names are forbidden

Or it’s a *really* bad idea to try to use them

``` r
TRUE
FALSE
Inf
NaN 
NA 
NULL

for
if
while
break
function
```

# 2. Everything is an object

There are a few built-in objects.

``` r
letters
```

    ##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r" "s"
    ## [20] "t" "u" "v" "w" "x" "y" "z"

``` r
pi
```

    ## [1] 3.141593

``` r
LETTERS
```

    ##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
    ## [20] "T" "U" "V" "W" "X" "Y" "Z"

# 3. You can create objects

In fact, this is mostly what we will be doing

Objects are created by assigning a thing to a name:

``` r
## name... gets ... this stuff
my_numbers <- c(1, 2, 3, 1, 3, 5, 25, 10)

## name ... gets ... the output of the function `c()`
your_numbers <- c(5, 31, 71, 1, 3, 21, 6, 52)
```

The `c()` function *combines* or *concatenates* things

The core thing we do in R is *create objects* by *assigning a thing to a name*. That thing is usually the output of some *function*. There are a lot of built-in functions.

We can create an object with the `c()` function and the *assignment operator*, `<-`. The `c()` function concatenates things together.

# The assignment operator

-   The assignment operator performs the action of creating objects

-   Use a keyboard shortcut to write it:

-   Press `option` *and* `-` on a Mac

-   Press `alt` *and* `-` on Windows

# Assignment with `=`

-   You can use “`=`” as well as “`<-`” for assignment

``` r
my_numbers = c(1, 2, 3, 1, 3, 5, 25)

my_numbers
```

    ## [1]  1  2  3  1  3  5 25

On the other hand, “`=`” has a different meaning when used in functions.

I’m going to use “`<-`” for assigment throughout. Just be consistent either way.

# 4. Do things to objects with functions

``` r
## this object... gets ... the output of this function
my_numbers <- c(1, 2, 3, 1, 3, 5, 25, 10)

your_numbers <- c(5, 31, 71, 1, 3, 21, 6, 52)
```

``` r
my_numbers
```

    ## [1]  1  2  3  1  3  5 25 10

Functions can be identified by the parentheses after their names.

``` r
my_numbers 
```

    ## [1]  1  2  3  1  3  5 25 10

``` r
## If you run this you'll get an error
mean()
```

## What functions usually do

-   They take **inputs** to **arguments**

-   They perform **actions**

-   They produce, or return, **outputs**

``` r
## Get the mean of what? Of x.
## You need to tell the function what x is
mean(x = my_numbers)
```

    ## [1] 6.25

``` r
mean(x = your_numbers)
```

    ## [1] 23.75

If you don’t *name* the arguments, R assumes you are providing them in the order the function expects.

``` r
mean(your_numbers)
```

    ## [1] 23.75

What arguments? Which order? Read the function’s help page.

``` r
help(mean)
```

``` r
## quicker
?mean
```

-   Arguments often tell the function what to do in specific circumstances

``` r
missing_numbers <- c(1:10, NA, 20, 32, 50, 104, 32, 147, 99, NA, 45)

mean(missing_numbers)
```

    ## [1] NA

``` r
mean(missing_numbers, na.rm = TRUE)
```

    ## [1] 32.44444

Or select from one of several options:

``` r
## Look at ?mean to see what `trim` does
mean(missing_numbers, na.rm = TRUE, trim = 0.1)
```

    ## [1] 27.25

There are all kinds of functions. They return different things.

``` r
summary(my_numbers)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00    1.75    3.00    6.25    6.25   25.00

You can assign the output of a function to a name, which turns it into an object. (Otherwise it’ll send its output to the console.)

``` r
my_summary <- summary(my_numbers)

my_summary
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00    1.75    3.00    6.25    6.25   25.00

Objects hang around in your work environment until they are overwritten by you, or are deleted.

``` r
## rm() function removes objects
rm(my_summary)

my_summary

## Error: object 'my_summary' not found
```

## Functions can be nested

``` r
c(1:20)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

``` r
mean(c(1:20))
```

    ## [1] 10.5

``` r
summary(mean(c(1:20)))
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    10.5    10.5    10.5    10.5    10.5    10.5

``` r
names(summary(mean(c(1:20))))
```

    ## [1] "Min."    "1st Qu." "Median"  "Mean"    "3rd Qu." "Max."

``` r
length(names(summary(mean(c(1:20)))))
```

    ## [1] 6

Nested functions are evaluated from the inside out.

-   # Use the pipe operator: `%>%`

Instead of nesting functions in parentheses, we can use the *pipe operator*:

``` r
c(1:20) %>% mean() %>% summary() %>% names() %>%  length()
```

    ## [1] 6

Read this operator as “**and then**”

Better, vertical space is free in R:

``` r
c(1:20) %>% 
  mean() %>% 
  summary() %>% 
  names() %>% 
  length()
```

    ## [1] 6

Piped operations make code more readable

Not great, Bob:

``` r
  serve(stir(pour_in_pan(whisk(crack_eggs(get_from_fridge(eggs), into = "bowl"), len = 40), temp = "med-high")))
```

Notice how the first thing you read is the last operation performed.

Really not much better:

``` r
serve(
  stir(
    pour_in_pan(
      whisk(
        crack_eggs(
          get_from_fridge(eggs), 
        into = "bowl"), 
      len = 40), 
    temp = "med-high")
  )
)
```

Much nicer:

``` r
eggs %>% 
  get_from_fridge() %>% 
  crack_eggs(into = "bowl") %>% 
  whisk(len = 40) %>% 
  pour_in_pan(temp = "med-high") %>% 
  stir() %>% 
  serve()
```

We’ll still use nested parentheses quite a bit, often in the context of a function working inside a pipeline. But it’s good not to have too many levels of nesting.

# Now showing at an R near you: `|>`

The pipe operator **`%>%`** is not part of Base R. It was introduced in a package called `magrittr`.

It’s been so successful, a version of it has been incorporated into Base R.

It is denoted by **`|>`**. But! It does not *quite* replace **`%>%`** in every case.\*

The magrittr pipe will continue to work.

# Functions are bundled into packages

Packages are loaded into your working environment using the `library()` function.

``` r
## A package containing a dataset rather than functions
library(gapminder)

gapminder
```

    ## # A tibble: 1,704 × 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 1,694 more rows

You need only *install* a package once (and occasionally update it). But you must *load* the package in each R session before you can access its contents.

``` r
## Do at least once for each package. Once done, not needed each time.
install.packages("palmerpenguins", repos = "http://cran.rstudio.com")

## Needed sometimes, especially after an R major version upgrade.
update.packages(repos = "http://cran.rstudio.com")
```

``` r
## To load a package, usually at the start of your RMarkdown document or script file
library(palmerpenguins)
penguins
```

    ## # A tibble: 344 × 8
    ##    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
    ##    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
    ##  1 Adelie  Torgersen           39.1          18.7               181        3750
    ##  2 Adelie  Torgersen           39.5          17.4               186        3800
    ##  3 Adelie  Torgersen           40.3          18                 195        3250
    ##  4 Adelie  Torgersen           NA            NA                  NA          NA
    ##  5 Adelie  Torgersen           36.7          19.3               193        3450
    ##  6 Adelie  Torgersen           39.3          20.6               190        3650
    ##  7 Adelie  Torgersen           38.9          17.8               181        3625
    ##  8 Adelie  Torgersen           39.2          19.6               195        4675
    ##  9 Adelie  Torgersen           34.1          18.1               193        3475
    ## 10 Adelie  Torgersen           42            20.2               190        4250
    ## # … with 334 more rows, and 2 more variables: sex <fct>, year <int>

# 5. Objects come in types and classes

I’m going to speak somewhat loosely here for now, and gloss over some distinctions between object classes and data structures, as well as kinds of objects and their attributes.

The object inspector in RStudio is your friend.

You can ask an object what it is.

``` r
class(my_numbers)
```

    ## [1] "numeric"

``` r
typeof(my_numbers)
```

    ## [1] "double"

Objects can have more than one (nested) class:

``` r
summary(my_numbers)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00    1.75    3.00    6.25    6.25   25.00

``` r
my_smry <- summary(my_numbers) # remember, outputs can be assigned to a name, creating an object

class(summary(my_numbers)) # functions can be nested, and are evaluated from the inside out
```

    ## [1] "summaryDefault" "table"

``` r
class(my_smry) # equivalent to the previous line
```

    ## [1] "summaryDefault" "table"

``` r
typeof(my_smry)
```

    ## [1] "double"

``` r
attributes(my_smry)
```

    ## $names
    ## [1] "Min."    "1st Qu." "Median"  "Mean"    "3rd Qu." "Max."   
    ## 
    ## $class
    ## [1] "summaryDefault" "table"

``` r
## In this case, the functions extract the corresponding attribute
class(my_smry)
```

    ## [1] "summaryDefault" "table"

``` r
names(my_smry)
```

    ## [1] "Min."    "1st Qu." "Median"  "Mean"    "3rd Qu." "Max."

## Vector types can’t be heterogenous

Objects can be manually or automatically coerced from one class to another. Take care.

``` r
class(my_numbers)
```

    ## [1] "numeric"

``` r
my_new_vector <- c(my_numbers, "Apple")

my_new_vector # vectors are homogeneous/atomic
```

    ## [1] "1"     "2"     "3"     "1"     "3"     "5"     "25"    "10"    "Apple"

``` r
class(my_new_vector)
```

    ## [1] "character"

``` r
my_dbl <- c(2.1, 4.77, 30.111, 3.14519)
is.double(my_dbl)
```

    ## [1] TRUE

``` r
my_dbl <- as.integer(my_dbl)

my_dbl
```

    ## [1]  2  4 30  3

# A table of data is a kind of list

``` r
gapminder # tibbles and data frames can contain vectors of different types
```

    ## # A tibble: 1,704 × 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # … with 1,694 more rows

``` r
class(gapminder)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
typeof(gapminder) # hmm
```

    ## [1] "list"

Underneath, most complex R objects are some kind of list with different components.

-   A *data frame* is a list of vectors of the same length, where the vectors can be of different types (e.g. numeric, character, logical, etc)

-   A *tibble* is an enhanced data frame

# Some classes are nested

-   Base R’s trusty `data.frame`

``` r
library(socviz)
titanic
```

    ##       fate    sex    n percent
    ## 1 perished   male 1364    62.0
    ## 2 perished female  126     5.7
    ## 3 survived   male  367    16.7
    ## 4 survived female  344    15.6

``` r
class(titanic)
```

    ## [1] "data.frame"

``` r
## The `$` idiom picks out a named column here; 
## more generally, the named element of a list
titanic$percent  
```

    ## [1] 62.0  5.7 16.7 15.6

-   The Tidyverse’s enhanced `tibble`

``` r
## tibbles are build on data frames 
titanic_tb <- as_tibble(titanic) 
class(titanic)
```

    ## [1] "data.frame"

``` r
titanic_tb
```

    ## # A tibble: 4 × 4
    ##   fate     sex        n percent
    ##   <fct>    <fct>  <dbl>   <dbl>
    ## 1 perished male    1364    62  
    ## 2 perished female   126     5.7
    ## 3 survived male     367    16.7
    ## 4 survived female   344    15.6

``` r
class(titanic_tb)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

Tidyverse tools are generally *type safe*, meaning their functions return the same type of thing every time, or fail if they cannot do this. So it’s good to know about the various data types.

# 6. Arithmetic on vectors

In R, all numbers are vectors of different sorts. Even single numbers (“scalars”) are conceptually vectors of length 1.

-   Arithmetic on vectors follows a series of *recycling rules* that favor ease of expression of vectorized, “elementwise” operations.

-   See if you can predict what the following operations do:

``` r
my_numbers
```

    ## [1]  1  2  3  1  3  5 25 10

``` r
result1 <- my_numbers + 1
```

``` r
result1
```

    ## [1]  2  3  4  2  4  6 26 11

``` r
result2 <- my_numbers + my_numbers
```

``` r
result2
```

    ## [1]  2  4  6  2  6 10 50 20

``` r
two_nums <- c(5, 10)

result3 <- my_numbers + two_nums
```

``` r
result3
```

    ## [1]  6 12  8 11  8 15 30 20

``` r
three_nums <- c(1, 5, 10)

result4 <- my_numbers + three_nums
```

    ## Warning in my_numbers + three_nums: longer object length is not a multiple of
    ## shorter object length

``` r
result4
```

    ## [1]  2  7 13  2  8 15 26 15

Note that you get a *warning* here. It’ll still do it, though! Don’t ignore warnings until you understand what they mean.
