#R #data-structure

The basic type of [[R]] is `vector`. (store **collections** of elements)

- `vector` is like the array or list in others which can hold only one type.
- `attr()` get/set attributes of a `vector`, `attributes()` get all attributes
    - Attributes should generally be thought of as ephemeral.
- S3 vector has `class` attribute and other meta info
    - `factor` stores categorical data
    - `ordered()` to create ordered factor (levels low to high)

<table><tr>
<td style="border: none;"><img height=300 src="https://i.imgur.com/QjiWrk1.png"/> </td>
<td style="border: none;"><img height=200 src="https://i.imgur.com/Ks7MWSE.png"/> </td>
</tr></table>

- vector can only hold one type
- list can hold more than one type

## Atomic Vector

Atomic vector can only hold one type.

`c()` to make longer vectors (`c(1, c("a", "2"))` ⇒ `"1" "a" "2"`)

### Primary Types and Scalars

- 4 primary types: `logical`, `integer`, `double`, `character`
- scalar is a special syntax to create an individual value.

| Type          | Value                             | Example                    |
| ------------- | --------------------------------- | -------------------------- |
| `logical`     | `TRUE` or `FALSE` (`T` or `F`)    |                            |
| `double`      | decimal, scientific, hexadecimal  | `1.234`, `1.23e4`, `0xabe` |
| `integer`     | like `double` but followed by `L` | `123L`, `1e4L`, `0xabeL`   |
| `character`   | surrounded by `"` or `'`          | `"hello"`                  |
| Missing Value | `NA` Not Applicable               | `NA`                       |

### Subsetting

- **Positive integers** ==return== elements at the specified positions.
- **Negative integers** ==exclude== elements at the specified positions.
- **Logical vectors** select elements where the corresponding logical value is `TRUE`.
- **Nothing** returns the original vector.
- **character vectors** return elements with matching names if the vector is named.

Example:

```r
x <- c(2.1, 4.2, 3.3, 5.4)

## Positive Integers
x[c(3, 1)]  # 3.3 2.1
x[order(x)] # 2.1 3.3 4.2 5.4 (order will return indices not vector)
# Duplicate indices will duplicate values
x[c(1, 1)]  # 2.1 2.1
# Real numbers are silently truncated to integers
x[c(2.1, 2.9)] # 4.2 4.2

## Negative Integers
x[-c(3, 1)] # 4.2 5.4

## Logical Vectors
x[x > 3] # 4.2 3.3 5.4  (x > 3 will return a logical vector)

## Named Vectors
y <- setNames(x, letters[1:4])
#>   a   b   c   d 
#> 2.1 4.2 3.3 5.4
y[c("d", "c", "a")] # 5.4 3.3 2.1
y[c("a", "a", "a")] # 2.1 2.1 2.1
```

#### Subsetting and assignment

All subsetting operators can be combined with assignment to modify selected values of an input vector.

```r
x <- 1:5
x[c(1, 2)] <- c(101, 102)

x # 101 102   3   4   5
```
### Testing and Coercion

- **Testing**: `is.*()` functions: `is.logical()`, `is.integer()`, `is.character()`
- **Explicit Coercion**: `as.*()` functions:
- **Implicit Coercion**:

- other ⇒ character
```r
c("a", 1) # "a" "1" (string, not "a" 1 )
```

- logical ⇒ numeric (`sum`, `mean`)
```r
x <- c(FALSE, FALSE, TRUE)
as.numeric(x) # 0 0 1
# Total number of TRUEs
sum(x)    # 1
# Proportion that are TRUE
mean(x)   # 0.333
```

## Attributes of Vectors

Other vectors (except primary vectors) have attributes to save meta info, including matrix, array, factor, datetime.

e.g. `matrix` has `dim` attribute to save the dimension info

```r
structure(1:10, dim = c(2, 5)) %>% sloop::s3_class()
#> [1] "matrix"  "integer" "numeric"
```

Special attributes:
- [[#Names `names`]]
- [[#Dimensions `dim`]] => Matrix and Array
### Get/Set Attributes

- get/set `attr(var, "attr")`
- get all attributes `attributes(var)`

```r
a <- 1:3
attr(a, "x") <- "abcdef"
attr(a, "x") # "abcdef"

attr(a, "y") <- 4:6
str(attributes(a))
#> List of 2
#>  $ x: chr "abcdef"
#>  $ y: int [1:3] 4 5 6

# Or equivalently
a <- structure(
  1:3, 
  x = "abcdef",
  y = 4:6
)
str(attributes(a))
#> List of 2
#>  $ x: chr "abcdef"
#>  $ y: int [1:3] 4 5 6
```

### Names `names`

`names` is a special attribute of the `vector`

```r
# When creating it: 
x <- c(a = 1, b = 2, c = 3)

# By assigning a character vector to names()
x <- 1:3
names(x) <- c("a", "b", "c")

# Inline, with setNames():
x <- setNames(1:3, c("a", "b", "c"))

attribtes(x)
# $names
# [1] "a" "b" "c"
```

### Dimensions `dim`

Adding a `dim` attribute to a vector allows it to behave like a 2-dimensional **matrix** or a multi-dimensional **array**.

You can create matrices and arrays with `matrix()` and `array()`, or by using the assignment form of `dim()`:

```r
# Two scalar arguments specify row and column sizes
x <- matrix(1:6, nrow = 2, ncol = 3)
x
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6

# One vector argument to describe all dimensions
y <- array(1:12, c(2, 3, 2))
y
#> , , 1
#> 
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
#> 
#> , , 2
#> 
#>      [,1] [,2] [,3]
#> [1,]    7    9   11
#> [2,]    8   10   12

# You can also modify an object in place by setting dim()
z <- 1:6
dim(z) <- c(3, 2)
z
#>      [,1] [,2]
#> [1,]    1    4
#> [2,]    2    5
#> [3,]    3    6
```

#### Vector vs Matrix vs Array

| Vector                                                | Matrix                                                                                                                     | Array                                                |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| [`names()`](https://rdrr.io/r/base/names.html)        | [`rownames()`](https://tibble.tidyverse.org/reference/rownames.html), [`colnames()`](https://rdrr.io/r/base/colnames.html) | [`dimnames()`](https://rdrr.io/r/base/dimnames.html) |
| [`length()`](https://rdrr.io/r/base/length.html)      | [`nrow()`](https://rdrr.io/r/base/nrow.html), [`ncol()`](https://rdrr.io/r/base/nrow.html)                                 | [`dim()`](https://rdrr.io/r/base/dim.html)           |
| [`c()`](https://rdrr.io/r/base/c.html)                | [`rbind()`](https://rdrr.io/r/base/cbind.html), [`cbind()`](https://rdrr.io/r/base/cbind.html)                             | `abind::abind()`                                     |
| —                                                     | [`t()`](https://rdrr.io/r/base/t.html)                                                                                     | [`aperm()`](https://rdrr.io/r/base/aperm.html)       |
| [`is.null(dim(x))`](https://rdrr.io/r/base/NULL.html) | [`is.matrix()`](https://rdrr.io/r/base/matrix.html)                                                                        | [`is.array()`](https://rdrr.io/r/base/array.html)    |

#### Matrices and Arrays Subsetting

Three Ways to subset higher-dimensional structures.

- With a single vector. (return a **vector**, NOT matrix or array)
- With multiple vectors.
- With a matrix. (return a **vector**)

```r
vals <- outer(1:5, 1:4, FUN = "paste", sep = ",")

vals
#      [,1]  [,2]  [,3]  [,4]
# [1,] "1,1" "1,2" "1,3" "1,4"
# [2,] "2,1" "2,2" "2,3" "2,4"
# [3,] "3,1" "3,2" "3,3" "3,4"
# [4,] "4,1" "4,2" "4,3" "4,4"
# [5,] "5,1" "5,2" "5,3" "5,4"

## Single Vector
vals[c(4, 15)]
# [1] "4,1" "5,3"

## Multiple Vectors
vals[1:2, 2:3]
#      [,1]  [,2] 
# [1,] "1,2" "1,3"
# [2,] "2,2" "2,3"

vals[1:2, ]
#      [,1]  [,2]  [,3]  [,4]
# [1,] "1,1" "1,2" "1,3" "1,4"
# [2,] "2,1" "2,2" "2,3" "2,4"

## Matrix ncol = 2 for 2D Array, 3 for 3D
select <- matrix(ncol = 2, byrow = TRUE, c(
  1, 1,
  3, 1,
  2, 4
))
vals[select]
#> [1] "1,1" "3,1" "2,4"
```

## S3 Atomic Vectors

[[R S3]] Vector has a special `class` attribute so that **generic** function can perform differently according to different S3 Vector.

Every S3 object is built on top of a base type, and often stores additional information in other attributes. (e.g. `factor`, Datetime in `POSIXct`)

<img height=300 src="https://i.imgur.com/QjiWrk1.png"/>

### Factor

A `factor` is a vector that can contain only predefined values. It is used to store **categorical** data.

- `factor` use `integer` to store data
- `factor` has a `levels` attributes to save categorical value info
- `table()` function to count values

```r
x <- factor(c("a", "b", "b", "a"))
x
#> [1] a b b a
#> Levels: a b

typeof(x)
#> [1] "integer"
attributes(x)
#> $levels
#> [1] "a" "b"
#> 
#> $class
#> [1] "factor"
```

<img height=150 src="https://i.imgur.com/hTChv9T.png"/>

#### Ordered Factor

If levels have order, use ordered factor. e.g. low, medium, high

```r
grade <- ordered(c("b", "b", "a", "c"), levels = c("c", "b", "a"))
grade
#> [1] b b a c
#> Levels: c < b < a
attributes(grade)
#> $levels
#> [1] "c" "b" "a"
#> 
#> $class
#> [1] "ordered" "factor" 
```

### Date `date`

Date vectors are built on top of double vectors. They have class “Date” and no other attributes:

```r
today <- Sys.Date()
typeof(today)
#> [1] "double"
attributes(today)
#> $class
#> [1] "Date"
```

The value of the double (which can be seen by stripping the class), represents the number of days since 1970-01-01:

```r
date <- as.Date("1970-02-01")
unclass(date)
#> [1] 31
```

### Date-times `POSIXct`

Two ways of storing date-time information:

- `POSIXct`(calendar time), and `POSIXlt`(local time).
- POSIX is short for Portable Operating System Interface

We will focus on `POSIXct` (time zone only control the format)

```r
now_ct <- as.POSIXct("2018-08-01 22:00", tz = "UTC")
now_ct
#> [1] "2018-08-01 22:00:00 UTC"
typeof(now_ct)
#> [1] "double"
as.double(now_ct)  # seconds from 1970 Jan 01 at midnight GMT
#> [1] 1533160800
attributes(now_ct)
#> $class
#> [1] "POSIXct" "POSIXt" 
#> 
#> $tzone
#> [1] "UTC"
```

### Durations `difftime`

Durations: the amount of time between pairs of dates or date-times

- on top of `doubles`
- `units` attribute to store the time unit.

```r
one_week_1 <- as.difftime(1, units = "weeks")
one_week_1 #> Time difference of 1 weeks

typeof(one_week_1)   #> [1] "double"
attributes(one_week_1)
#> $class
#> [1] "difftime"
#> 
#> $units
#> [1] "weeks"

one_week_2 <- as.difftime(7, units = "days")
one_week_2 #> Time difference of 7 days

typeof(one_week_2)   #> [1] "double"
attributes(one_week_2)
#> $class
#> [1] "difftime"
#> 
#> $units
#> [1] "days"
```

## References

- [https://adv-r.hadley.nz/vectors-chap.html](https://adv-r.hadley.nz/vectors-chap.html)
- [https://adv-r.hadley.nz/subsetting.html](https://adv-r.hadley.nz/subsetting.html)