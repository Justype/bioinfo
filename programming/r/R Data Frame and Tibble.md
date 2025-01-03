#R #data-structure #todo 

| Feature            | Data Frame            | Tibble                       |
| ------------------ | --------------------- | ---------------------------- |
| Printing           | Prints all rows.      | Summarizes rows and columns. |
| Character Columns  | Converts to factors.  | Keeps as characters.         |
| Subsetting         | Vector or data frame. | Always a tibble.             |
| Error Messages     | Generic.              | Informative and detailed.    |
| Non-Standard Names | Not allowed.          | Supported with backticks.    |
| Package Dependency | None.                 | Requires `tibble`.           |

A **data frame** is a named list of vectors with attributes:

- `names` (column names)
- `row.names`
- class `data.frame`

Data frame also has constraint and special functions:

- The length of each of its vectors must be the same
- `rownames()`, `colnames()`, `nrow()`, `ncol()`

```r
df1 <- data.frame(x = 1:3, y = letters[1:3])
typeof(df1)
#> [1] "list"
attributes(df1)
#> $names
#> [1] "x" "y"
#> 
#> $class
#> [1] "data.frame"
#> 
#> $row.names
#> [1] 1 2 3
```

## Creating

- `data.frame()` - `stringsAsFactors = F` to suppress conversion
    - Column Names of Data Frame cannot start with Number have space
- `tibble()` - no coercing by default
    - allows you to refer to variables created during construction
        - `tibble(x = 1:3, y = x * 2)`

```r
df <- data.frame(
  x = 1:3, 
  y = c("a", "b", "c")
)
str(df)
#> 'data.frame':    3 obs. of  2 variables:
#>  $ x: int  1 2 3
#>  $ y: chr  "a" "b" "c"
```


## Row Names

- Data frames allow you to label each row with a name, a character vector containing only unique values:

```r
df3 <- data.frame(
  age = c(35, 27, 18),
  hair = c("blond", "brown", "black"),
  row.names = c("Bob", "Susan", "Sam")
)
df3
#>       age  hair
#> Bob    35 blond
#> Susan  27 brown
#> Sam    18 black
```

# References

- [https://adv-r.hadley.nz/vectors-chap.html](https://adv-r.hadley.nz/vectors-chap.html)
- [https://adv-r.hadley.nz/subsetting.html](https://adv-r.hadley.nz/subsetting.html)