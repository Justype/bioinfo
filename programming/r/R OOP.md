#R #data-structure #algorithms #OOP #advanced 

[[Object-oriented Programming]] (OOP). OOP is a little more challenging in R.

- Multiple OOP systems in R:
    - [[R S3]] and [[R S4]] from R base (generic function, traditional R)
    - [[R RC]] from R base
    - [[R R6]] from [R6 package](https://r6.r-lib.org/articles/Introduction.html) (encapsulated method)
- `sloop` package to check OOP type
    - Use `sloop::s3_class()` instead of `class` (class is bad on base type)

## Why OOP?

The main reason to use OOP is **polymorphism**, which means that a developer can consider a function’s interface separately from its implementation.

e.g. `summary()` can produce different outputs for `numeric` and `factor`

```r
diamonds <- ggplot2::diamonds
summary(diamonds$carat)
#>    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#>    0.20    0.40    0.70    0.80    1.04    5.01
summary(diamonds$cut)
#>      Fair      Good Very Good   Premium     Ideal 
#>      1610      4906     12082     13791     21551
```

This is method dispatch, which allows different behavior of `summary()` function.

```r
# S3 single dispatch
x <- 1:10  # A numeric object

# Generic function
summary <- function(object, ...) UseMethod("summary")

# Default method for "summary"
summary.default <- function(object) cat("Summary of default object\n")

# Method for numeric objects
summary.numeric <- function(object) cat("Summary of numeric object: Mean =", mean(object), "\n")

# Call dispatch
summary(x)  # Dispatches to summary.numeric
# Output: Summary of numeric object: Mean = 5.5
```

## Main Components

- **Class**: Blueprint of an object
- **Fields**: A variable declared in class that stores data or state related to instances of that class.
- **Methods**: A function defined
    - Encapsulation (include in the class: `object$method()`)
    - Method dispatch (external **generic** function: `function(object)`)
- **Inheritance**: Allowing a class (child) to inherit fields and methods from another class (parent)

## OOP in R

| OOP      | Description                                                    | Source                                                        |
| -------- | -------------------------------------------------------------- | ------------------------------------------------------------- |
| [[R S3]] | Relies on common conventions                                   | ship with R                                                   |
| [[R S4]] | More guarantees and greater encapsulation                      | ship with R                                                   |
| [[R RC]] |                                                                | ship with R                                                   |
| [[R R6]] | public and private methods<br/>active bindings<br/>inheritance | [R6 package](https://r6.r-lib.org/articles/Introduction.html) |

### sloop package

```r
sloop::otype(1:10)    # base
sloop::otype(mtcars)  # S3
mle_obj <- stats4::mle(function(x = 1) (x - 2) ^ 2)
sloop::otype(mle_obj) # S4
```

## Base Type

`is.object()` to tell if a variable is OO object or not

```r
# A base object:
is.object(1:10)    # FALSE
sloop::otype(1:10) # "base"

# An OO object:
is.object(mtcars)    # TRUE
sloop::otype(mtcars) # "S3"
```

Don’t use `class()`, use `sloop::s3_class()`

`class()` returns misleading results when applied to **base** objects.

```r
x <- matrix(1:4, nrow = 2)
is.object(x)       # FALSE
typeof(x)          # "integer"
class(x)           # "matrix" "array"
sloop::s3_class(x) # "matrix"  "integer" "numeric"
```

### `typeof()` Return the Base Type

```r
typeof(NULL) # "NULL"
typeof(1L)   # "integer"
typeof(1i)   # "complex"

typeof(mean) # "closure"
typeof(`[`)  # "special"
typeof(sum)  # "builtin"
```

## References

- [https://adv-r.hadley.nz/oo.html](https://adv-r.hadley.nz/oo.html)
- [https://adv-r.hadley.nz/base-types.html](https://adv-r.hadley.nz/base-types.html)