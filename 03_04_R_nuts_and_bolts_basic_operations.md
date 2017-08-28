R Nuts and Bolts: Basic Operations
========================================================
author:
date:
autosize: true

Subsetting
========================================================

There are different operators that can be used to extract subsets of R objects.

- `[` always returns an object of the same class as the original; can be used to select more than one element

- `[[` is used to extract elements of a list or a data frame; it can only be used to extract a single element and the class of the returned object will not necessarily be a list or data frame

- `$` is used to extract elements of a list or data frame by name; semantics are similar to that of `[[`.

Subsetting a Vector
========================================================
<font size = "6px">
```r
> x <- c("a", "b", "c", "c", "d", "a")

# Using an index
> x[1]
[1] "a"

> x[2]
[1] "b"

> x[1:4]
[1] "a" "b" "c" "c"

# Using a boolean condition
> x[x > "a"]
[1] "b" "c" "c" "d"

> u <- x > "a"
> u
[1] FALSE TRUE TRUE TRUE TRUE FALSE
> x[u]
[1] "b" "c" "c" "d"
```
</font>

Subsetting a Matrix
========================================================

Matrices can be subsetted in the usual way with (_i,j_) type indices. By default, when a single element of a matrix is retrieved, it is returned as a vector of length 1 rather than a 1 x 1 matrix. This behavior can be turned off by setting `drop = FALSE`.

<font size = "6px">
```r
> x <- matrix(1:6, 2, 3)

#Get the element in the first row, second col
> x[1, 2]
[1] 3

#Get the element in the second row, first col
> x[2, 1]
[1] 2
```
</font>

Indices can also be missing, and this has a special meaning. Similarly, subsetting a single column or a single row will give you a vector, not a matrix (by default).

<font size = "6px">
```r
# Get all of the elements in the first row
> x[1, ]
[1] 1 3 5

> x[1, , drop = FALSE]
     [,1] [,2] [,3]
[1,]    1    3    5

# Get all of the elements in the second col
> x[, 2]
[1] 3 4
```
</font>

Subsetting a List
========================================================

<font size = "6px">
```r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")

> x[1]
$foo
[1] 1 2 3 4

> x[[1]]
[1] 1 2 3 4

> x$bar
[1] 0.6

> x[["bar"]]
[1] 0.6

> x["bar"]
$bar
[1] 0.6

> x[c(1, 3)]
$foo
[1] 1 2 3 4

$baz
[1] "hello"
```
</font>

Subsetting a List (Cont'd)
========================================================

The `[[` operator can be used with _computed_ indices; `$` can only be used with literal names.

<font size = "6px">
```r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
> name <- "foo"
> x[[name]]  ## computed index for ‘foo’
[1] 1 2 3 4
> x$name     ## element ‘name’ doesn’t exist!
NULL
> x$foo
[1] 1 2 3 4
```
</font>

Partial matching of names is allowed with `[[` and `$`.

<font size = "6px">
```r
> x <- list(aardvark = 1:5)
> x$a
[1] 1 2 3 4 5
> x[["a"]]
NULL
> x[["a", exact = FALSE]]
[1] 1 2 3 4 5
```
</font>

Hands-On (15 minutes)
========================================================


```r
library(swirl)
swirl()
#.....
# Select the "R Programming" entry
# Select the "Subsetting Vectors" entry [6]
```


Removing NAs
========================================================

A common task is to remove missing values (`NA`s).

<font size = "6px">
```r
> x <- c(1, 2, NA, 4, NA, 5)
> bad <- is.na(x)
> x[!bad]
[1] 1 2 4 5
```
</font>

---

<font size = "6px">
```r
> airquality[1:6, ]

  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA     NA  14.3   56     5   5
6    28     NA  14.9   66     5   6

> good <- complete.cases(airquality)

> airquality[good, ][1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
7    23     299  8.6   65     5   7
```
</font>

Hands-On
========================================================


```r
library(swirl)
swirl()
#.....
# Select the "R Programming" entry
# Select the "Missing Values" entry [5]
```

Built-in functions
========================================================

__Numeric functions__:

- `abs(x)`, the absolute value
- `sqrt(x)`, the square root
- `ceiling(x)`, ceiling(3.475) is 4
- `floor(x)`, floor(3.475) is 3
- `trunc(x)`, trunc(3.99) is 3
- `round(x, digits = n)`, round(3.475, 2) is 3.48
- ...

---

__Character functions__:

- `paste(..., sep = "")`, concatenate strings with a specific separator
    - (see `?paste` for more info)

<font size = "6px">

```r
paste("My", "name", "is", "...", sep = " ")
[1] "My name is ..."

paste("id", 1:3, sep = "_")
[1] "id_1" "id_2" "id_3"
```
</font>

- `grep(pattern, x, ...)`, search for pattern in x
    - (see `?grep` for more info)

<font size = "6px">

```r
grep("a", c("A", "a", "b", "a", "C"), fixed = TRUE)
[1] 2 4

grep("a", c("A", "a", "b", "a", "C"), ignore.case = TRUE)
[1] 1 2 4
```
</font>

Built-in functions
========================================================

__Statistical functions__:

- `rnorm()`, the normal distribution
- `dunif()`, the uniform distribution
- `mean()`, the mean
- `sd()`, the standard deviation
- and much more ...

__A more comprehensive list of built-in functions__

From "R in Action": http://www.statmethods.net/management/functions.html

`str` and `summary` functions
========================================================

The `str` function displays the internal structure of an R object, mostly used as a diagnostic function.

<font size = "6px">

```r
str(mtcars)
'data.frame':	32 obs. of  11 variables:
 $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
 $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
 $ disp: num  160 160 108 258 360 ...
 $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
 $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
 $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
 $ qsec: num  16.5 17 18.6 19.4 17 ...
 $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
 $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
 $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
 $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```
</font>

---

The `summary` function is a generic function used to produce summaries/ basic statistics for the provided R object.

<font size = "6px">

```r
summary(mtcars[,1:3])
      mpg             cyl             disp      
 Min.   :10.40   Min.   :4.000   Min.   : 71.1  
 1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8  
 Median :19.20   Median :6.000   Median :196.3  
 Mean   :20.09   Mean   :6.188   Mean   :230.7  
 3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0  
 Max.   :33.90   Max.   :8.000   Max.   :472.0  
```
</font>
