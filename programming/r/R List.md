#R #data-structure #todo 


`list` can store different types of data.

- `list()` to create a list. You can put lists inside of a list.
- Technically, list is a special [[R Vector]] which stores the same type data: **references**:

```r
n = c(2, 3, 5) 
s = c("aa", "bb", "cc", "dd", "ee") 
b = c(TRUE, FALSE, TRUE, FALSE, FALSE) 
x = list(n, s, b)   # x contains references of n, s, b

lobstr::obj_addr(x[[1]]) # 0xb0a905816a28
lobstr::obj_addr(n)      # 0xb0a905816a28
```

## Creating `list()`

```r
ls <- list(
  1:3, 
  "a", 
  c(TRUE, FALSE, TRUE), 
  c(2.3, 5.9)
)

typeof(ls)  # "list"

str(ls)
# List of 4
#  $ : int [1:3] 1 2 3
#  $ : chr "a"
#  $ : logi [1:3] TRUE FALSE TRUE
#  $ : num [1:2] 2.3 5.9
```

### Nested List

```r
nested.ls <- list(list(1, 2), c(3, 4))

str(nested.ls)
# List of 2
#  $ :List of 2
#   ..$ : num 1
#   ..$ : num 2
#  $ : num [1:2] 3 4
```

### Combining List `c`

```r
ex.ls <- c(list(1, 2), c(3, 4))

str(ex.ls)
# List of 4
#  $ : num 1
#  $ : num 2
#  $ : num 3
#  $ : num 4
```

## Testing and Coercion

The `typeof()` a list is `list`.

- Test for a list with `is.list()`
- Coerce to a list with `as.list()` or `list` (They act differently)
- Convert list to atomic vector `unlist()`

```r
list(1:3)
# [[1]]
# [1] 1 2 3

as.list(1:3)
# [[1]]
# [1] 1
# 
# [[2]]
# [1] 2
# 
# [[3]]
# [1] 3
```

## List Array with `dim` attribute

List Array can be useful if you want to arrange objects in a grid-like structure.

```r
l <- list(1:3, "a", TRUE, 1.0)
dim(l) <- c(2, 2)
l
#>      [,1]      [,2]
#> [1,] Integer,3 TRUE
#> [2,] "a"       1

l[[1, 1]]
#> [1] 1 2 3
```

## Subsetting

- `ls[indices]` still returns list
- `ls[[index]]` returns item
- `ls$index` returns item (named list only)

### Selecting a single element `[[` or `$`

```r
x <- list(1:3, "a", 4:6)

str(x)
# List of 3
#  $ a: int [1:3] 1 2 3
#  $ b: chr "a"
#  $ c: int [1:3] 4 5 6

str(x[1])    # return list
# List of 1
#  $ a: int [1:3] 1 2 3

str(x[[1]])  # return element not list
# int [1:3] 1 2 3

str(x$a) # $ can only work with names not numbers
# int [1:3] 1 2 3
```

### Out of bounds

|         | `[]`   | `[[]]`  | `$`    |
| ------- | ------ | ------- | ------ |
| integer | `NULL` | `ERROR` |        |
| str     | `NULL` | `NULL`  | `NULL` |

```r
x[10]     # $<NA>  NULL
x[[10]]   # Error in x[[10]] : subscript out of bounds
x$10      # Error: unexpected numeric constant in "x$10"

x["z"]    # $<NA>  NULL
x[["z"]]  # NULL
x$z       # NULL
```

Or you can use [`purrr::pluck()`](https://purrr.tidyverse.org/reference/pluck.html) will always success.

Return default value (`NULL`) if value is missing.

```r
purrr::pluck(x, 10) # NULL
purrr::pluck(x, 10, .default = NA) # NA
```

## References

- https://adv-r.hadley.nz/vectors-chap.html#lists
- https://adv-r.hadley.nz/subsetting.html