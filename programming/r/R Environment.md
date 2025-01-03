#R #data-structure #advanced

- The environment is the data structure that powers scoping.
- Understanding environments is not necessary for day-to-day use of R.
- R use environments and search path to solve libraries' functions and variables.

## Environment Basics

Environment is similar to named list with four exceptions:

- Every name must be unique.
- The names in an environment are not ordered.
- An environment has a parent.
- Environments are not copied when modified.

### Create Environment

```r
env <- rlang::env(a = 1, b = "test")
rlang::env_print(env)
# <environment: 0xac0b495227e0>
# Parent: <environment: global>
# Bindings:
# • a: <dbl>
# • b: <chr>

rlang::env_names(env)
# [1] "a" "b"

identical(rlang::global_env(), rlang::current_env())
# [1] TRUE
```

### Every Environment Has a Parent

```r
e2a <- rlang::env(d = 4, e = 5)
e2b <- rlang::env(e2a, a = 1, b = 2, c = 3)
rlang::env_parent(e2b)
#> <environment: 0xac0b4ae2fbc8>
rlang::env_print(e2a)
#> <environment: 0xac0b4ae2fbc8>
#> Parent: <environment: global>
#> Bindings:
#> • d: <dbl>
#> • e: <dbl>

rlang::env_parent(e2a)
#> <environment: R_GlobalEnv>
```

### Get/Set/Unbind Environment Bindings

Get

- `env$name`
- `env[["name"]]`
- `rlang::env_get(env, "name")` also find in parents: `inherit = T`

Set

- `env$name = value`
- `env[["name"]] = value`
- `rlang::env_get(env, "name", value)` (also in parent `inherit = T`)
- `rland::env_bind(env, name1 = value1, name2 = value2)`

Unbind

- `rlang::env_unbind(env, "name")`, set env to NULL cannot unbind
- `rlang::env_has(env, "name")`, check binding in an env.

```r
rlang::env_get(e2b, "e")
# Error in `rlang::env_get()`:
# ! Can't find `e` in environment.
# Backtrace:
#  1. rlang::env_get(e2b, "e")

rlang::env_get(e2b, "e", inherit = T) # Find 
# [1] 5
```

## Special Environments

### Package Environments

Each package attached by `library()` or `require()` becomes one of the parents of the global environment.

If you follow all the parents back, you see the order in which every package has been attached. This is known as the **search path**.

![](https://i.imgur.com/yi7Ddhh.png)

`base` is the environment of base package.

```r
search()
#>  [1] ".GlobalEnv"        "package:rlang"     "package:stats"    
#>  [4] "package:graphics"  "package:grDevices" "package:utils"    
#>  [7] "package:datasets"  "package:methods"   "Autoloads"        
#> [10] "package:base"

rlang::search_envs()
#>  [[1]] $ <env: global>
#>  [[2]] $ <env: package:rlang>
#>  [[3]] $ <env: package:stats>
#>  [[4]] $ <env: package:graphics>
#>  [[5]] $ <env: package:grDevices>
#>  [[6]] $ <env: package:utils>
#>  [[7]] $ <env: package:datasets>
#>  [[8]] $ <env: package:methods>
#>  [[9]] $ <env: Autoloads>
#> [[10]] $ <env: package:base>
```

`package::function` to use functions in a package environment.

```r
rlang::env() # use env function in rlang package
```

## Function Environment

The **function environment** is the environment in which a function was **created or defined**. A function binds the **current environment** when it is created and is used for lexical scoping.

Across computer languages, functions that capture (or enclose) their environments are called **closures**, which is often used interchangeably with _**function**_ in R’s documentation.

```r
y <- 1
f <- function(x) x + y
fn_env(f)         #> <environment: R_GlobalEnv>

e <- rlang::env()
e$g <- function() 1
environment(e$g)  #> <environment: R_GlobalEnv>
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/5adf367e-0af4-4d30-8f2a-3330372efec4/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/4bc5bedc-8ca3-4fbe-9e0b-d7e31e76049d/image.png)

## Execution Environments

The **execution environment** is created each time a function is **called or executed**. It is temporary and exists only for the duration of the function call. Once the function has completed, the environment will be garbage collected.

```r
h <- function(x) {
  # 1.
  a <- 2 # 2.
  x + a
}
y <- h(1) # 3.
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/33471236-e6c6-4785-b385-e4f962bbc656/4d69a38b-3f63-42be-9104-9cbae3297c92.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/dded9223-11cb-43dc-8ba7-3041345a1d81/716ebdf2-aa6e-4d7c-bbc5-001a264e36a0.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/35bbb49e-65f5-47df-a9cc-69dec9194d83/e77dd8bf-2b59-4c4b-a22b-ce12cd62df5c.png)

## Namespace

The **namespace** environment is the internal interface to the package. **Namespaces** ensure that packages find the correct functions regardless of the order in which packages are loaded.

- The **package environment** controls how we find the function. (external interface)
- The **namespace** controls how the function finds its variables. (internal space)

e.g. `sd` find `var` function in `namespace:stats`

```r
sd
#> function (x, na.rm = FALSE) 
#> sqrt(var(if (is.vector(x) || is.factor(x)) x else as.double(x), 
#>     na.rm = na.rm))
#> <bytecode: 0x7fe6c495c900>
#> <environment: namespace:stats>
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/87f1b7d7-5e29-4ed8-8f6a-01b9d2c11149/image.png)

- Each namespace has an **imports** environment that contains bindings to all the functions used by the package. The imports environment is controlled by the package developer with the `NAMESPACE` file.
- Explicitly importing every base function would be tiresome, so the parent of the imports environment is the base **namespace**. The base namespace contains the same bindings as the base environment, but it has a different parent.
- The parent of the base namespace is the global environment. This means that if a binding isn’t defined in the imports environment the package will look for it in the usual way. This is usually a bad idea.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/17b55cbe-f014-468f-9922-9d4f3a63dad1/image.png)

# Call Stacks

---

`rlang::caller_env()` accesses **caller** environment. This provides the environment from which the function was called. (not defined)

(Stack: First In Last Out)

## Call Stack Example

```r
f <- function(x) g(x = 2)
g <- function(x) h(x = 3)
h <- function(x) stop()

f(x = 1)
#> Error:
traceback()
#> 4: stop() at #1
#> 3: h(x = 3) at #1
#> 2: g(x = 2) at #1
#> 1: f(x = 1)
```

**C**all **S**tack **T**ree by `lobstr::cst()`

```r
h <- function(x) {
  lobstr::cst()
}
f(x = 1)
#>    ▆
#> 1. └─global f(x = 1)
#> 2.   └─global g(x = 2)
#> 3.     └─global h(x = 3)
#> 4.       └─lobstr::cst()
```

## Frames

Each element of the call stack is a **frame**. Three key components of frame:

- An expression (labelled with `expr`) giving the function call. This is what `traceback()` prints out.
- An environment (labelled with `env`), which is typically the **execution environment** of a function.
- A parent, the previous call in the call stack (shown by a grey arrow).

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fed57f19-841c-40cc-8bec-ed90e77ad1b8/7d54024c-5d2a-4657-b5ae-f9801f478132/image.png)

## Lazy Evaluation in Function Call

```r
f <- function() { print("f:in"); g();           print("f:out"); }
g <- function() { print("g:in"); h();           print("g:out"); }
h <- function() { print("h:in"); lobstr::cst(); print("h:out"); }

a <- function(x) { print("a:in"); b(x); print("a:out"); }
b <- function(x) { print("b:in"); c(x); print("b:out"); }
c <- function(x) { print("c:in"); x;    print("c:out"); }

a(f())
```

`x` is lazily evaluated so this tree gets two branches. In the first branch `a()` calls `b()`, then `b()` calls `c()`. The second branch starts when `c()` evaluates its argument `x`. This argument is evaluated in a new branch because the environment in which it is evaluated is the global environment, not the environment of `c()`.

```r
[1] "a:in"
[1] "b:in"
[1] "c:in"
[1] "f:in"
[1] "g:in"
[1] "h:in"
    ▆
 1. ├─global a(f())
 2. │ └─global b(x)
 3. │   └─global c(x)
 4. └─global f()
 5.   └─global g()
 6.     └─global h()
 7.       └─lobstr::cst()
[1] "h:out"
[1] "g:out"
[1] "f:out"
[1] "c:out"
[1] "b:out"
[1] "a:out"
```

# Use Environments

- **Avoiding copies of large data**. (Better use [R R6](https://www.notion.so/R-R6-1528695f3ace8071b572fa2c68691c9c?pvs=21) )
- **Managing state within a package**. (maintain state across function calls)
```r
my_env <- new.env(parent = emptyenv())
my_env$a <- 1

get_a <- function() {
  my_env$a
}
set_a <- function(value) {
  old <- my_env$a
  my_env$a <- value
  invisible(old)
}
```
- **As a hash map**. (takes constant)

# References

- [https://adv-r.hadley.nz/environments.html](https://adv-r.hadley.nz/environments.html)