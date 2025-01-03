#R #data-structure #algorithms #function

Functions are special object in [[R]].

- Definition and Assign to a variable: `name <- function(x) expression`
- [[#Lexical Scoping]]: Searching variables based on the function definition.

There are two way to exit the function:

1. Return by `return()` or implicit return (last expression)
    - `invisible()` to prevent printing return value
2. Error by `stop`
    - `on.exit(expression, add = T)` Set up an exit handler (no matter error or not)
        - undo global settings, like `setwd()` or `options()`

## Function Fundamentals

### Function Components

- `formals` - list of arguments that control how you call the function
- `body` - code inside the function
- `environment` - how the function finds the values associated with the names

```r
f02 <- function(x, y) {
  # A comment
  x + y
}

formals(f02)
#> $x
#> 
#> $y

body(f02)
#> {
#>     x + y
#> }

environment(f02)
#> <environment: R_GlobalEnv>
```

### Function is an object

- R functions are objects in their own right
- create a function with `function` and bind it to a name with `<-` or `=`
- **anonymous function** does not bind to a name

```r
funs <- list(
  half = function(x) x / 2,
  double = function(x) x * 2
)

funs$double(10)
#> [1] 20
```

### Function Composition

- `function_name()`
- Nested
- Save the intermediate results as variables
- Pipe: `magrittr`: [https://magrittr.tidyverse.org/](https://magrittr.tidyverse.org/)
    - `x %>% f()` is equivalent to `f(x)`;
    - `x %>% f(y)` is equivalent to `f(x, y)`;
    - `x %>% f(y, .)` is equivalent to `f(y, x)`.

```r
x <- runif(100)

# nested
sqrt(mean(square(deviation(x))))
#> [1] 0.274

# intermediate variable
out <- deviation(x)
out <- square(out)
out <- mean(out)
out <- sqrt(out)
out
#> [1] 0.274

# magrittr
library(magrittr)
x %>%
  deviation() %>%
  square() %>%
  mean() %>%
  sqrt()
#> [1] 0.274
```

### Primitive Function

- only in base package
- `typeof` - `builtin` or `special`
- primarily in C not R

```r
sum # function (..., na.rm = FALSE)  .Primitive("sum")
`[` # .Primitive("[")
`+` # function (e1, e2)  .Primitive("+")
```

### Special Function `%name%`

```r
`%name%` <- function(param1, param2) { # expression }
arg1 %name% arg2

`%plus%` <- function(x, y) { x + y }
1 %plus% 2
```

> [!question] Dplyr `%>%`
> Have you noticed the `%>%` function from [[dplyr]] or magrittr package? Can you achieve similar things using this special function?

## Lexical Scoping

Scoping determines how variable names are resolved in functions.

**Lexical scoping** uses the structure of the code (the lexical environment) to find the value of a variable. (based on how a function is defined, not how it is called)

4 primary rules:

- **Name masking** - names defined inside a function mask names defined outside a function.
- **Functions vs Variables** - `function()` vs `variable`
- **Fresh Start** - After calling the function, the variables in the function will be cleaned.
- **Dynamic lookup** - R looks for values when the function is run, not when the function is created.

```r
# Masking
x <- 10
a <- function(){x = 20; x}
a() # 20 not 10

# Functions vs Variables
do.call(function(){a = 5; a()}, list()) # 20 a() calls function a
do.call(function(){a = 5; a}, list())   # 5

# Dynamic Lookup
x <- 2
d <- function() x + 1
d() # == 3

x <- 4
d() # == 5
```

## Lazy Evaluation Arguments

In R arguments are only evaluated if accessed.

### Default Arguments

Default values can be defined in terms of other arguments, or even in terms of variables defined later in the function:

```r
h04 <- function(x = 1, y = x * 2, z = a + b) {
  a <- 10
  b <- 100
  
  c(x, y, z)
}

h04()
#> [1]   1   2 110
```

### Missing Arguments (default or user)

`missing()` - determine if an argument’s value comes from the user or from a default

```r
h06 <- function(x = 10) {
  list(missing(x), x)
}
str(h06())
#> List of 2
#>  $ : logi TRUE
#>  $ : num 10
str(h06(10))
#> List of 2
#>  $ : logi FALSE
#>  $ : num 10
```

## `...`

`...` dot-dot-dot is a special argument, which can take any number of additional arguments.

- Passing `...` to Another Function
- Use `list(...)` to parse additional arguments

```r
ddd <- function(x = 10, ...) {
  other_args <- list(...)
  print(other_args)
}

ddd(x = 1, y = 2)
# $y
# [1] 2
```

## Exiting a Function

- return a value
- throw an error

### Return a value (Implicit or Explicit)

- Implicitly, where the last evaluated expression is the return value
- Explicitly, by calling `return()`

```r
test <- function(x) {
  if (x < 10) {
    0           # Implicit
  } else {
    return(10)  # Explicit
  }
}
test(5)    #> [1] 0
test(15)   #> [1] 10
```

#### Invisible Return

- Most functions return visibly: calling the function in an interactive context prints the result.
- `invisible()` to prevent printing the return

```r
test <- function() 1
test()    #> [1] 1

test_invisible <- function() invisible(1)
test_invisible()        # no output
print(test_invisible()) # 1
```

### Errors

If a function cannot complete its assigned task, it should throw an error with `stop()`, which immediately terminates the execution of the function.

```r
test_error <- function() {
  stop("I'm an error")
  return(10)
}
test_error()
#> Error in test_error(): I'm an error
```

#### Exit Handlers

`on.exit()` to set up an **exit handler**: undo global changes no matter how a function exits.

Always set `add = TRUE` when using `on.exit()`. If you don’t, each call to `on.exit()` will overwrite the previous exit handler.

```r
test <- function(x) {
  cat("Hello\n")
  on.exit(cat("Goodbye!\n"), add = TRUE)
  
  if (x) {
    return(10)
  } else {
    stop("Error")
  }
}

test(TRUE)
#> Hello
#> Goodbye!
#> [1] 10

test(FALSE)
#> Hello
#> Error in test(FALSE): Error
#> Goodbye!
```

Exit handlers are useful to place clean-up code next to the that requires clean-up

```r
with_dir <- function(dir, code) {
  old <- setwd(dir)
  on.exit(setwd(old), add = TRUE)

  force(code)
}

getwd()
#> [1] "/Users/runner/work/adv-r/adv-r"
with_dir("~", getwd())
#> [1] "/Users/runner"
```

`after = F` run first when exit

```r
test <- function() {
  on.exit(message("a"), add = TRUE, after = FALSE)
  on.exit(message("b"), add = TRUE, after = FALSE)
}

test()
#> b
#> a
```

## References

