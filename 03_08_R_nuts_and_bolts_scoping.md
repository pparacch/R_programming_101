Scoping Rules
========================================================
author:
date:
autosize: true

A Diversion on Binding Values to Symbol
========================================================

How does R know which value to assign to which symbol? When I type

<font size = "6px">

```r
lm <- function(x) { x * x }

# Print the function
lm
function(x) { x * x }
```
</font>

- How does R know what value to assign to the symbol `lm`?
- Why doesn't it give it the value of `lm` that is in the *stats* package?

A Diversion on Binding Values to Symbol (Cont'd)
========================================================

When R tries to bind a value to a symbol, it searches through a series of `environments` to find the appropriate value. When you are working on the command line and need to retrieve the value of an R object, the order is roughly

1. Search the __global environment__ for a symbol name matching the one requested.
2. Search the __namespaces__ of each of the packages on the search list

The search list can be found by using the `search` function.

<font size = "6px">

```r
search()
 [1] ".GlobalEnv"        "package:knitr"     "package:stats"    
 [4] "package:graphics"  "package:grDevices" "package:utils"    
 [7] "package:datasets"  "package:methods"   "Autoloads"        
[10] "package:base"     
```
</font>

Binding Values to Symbol
========================================================

- The __global environment__ or the user’s workspace is always the first element of the search list and the *base* package is always the last.
- The order of the packages on the search list matters!
- User’s can configure which packages get loaded on startup so you cannot assume that there will be a set list of packages available.
- When a user loads a package with `library` the namespace of that package gets put in position 2 of the search list (by default) and everything else gets shifted down the list.
- Note that R has separate namespaces for functions and non-functions so it’s possible to have an object named c and a function named c.

Scoping Rules
========================================================

The scoping rules for R are the main feature that make it different from the original S language.

- The scoping rules determine how a value is associated with a free variable in a function
- R uses _lexical scoping_ or _static scoping_. A common alternative is _dynamic scoping_.
- Related to the scoping rules is how R uses the search _list_ to bind a value to a symbol
- Lexical scoping turns out to be particularly useful for simplifying statistical computations

Lexical Scoping
========================================================

Consider the following function. This function has 2 formal arguments `x` and `y`. In the body of the function there is another symbol `z`. In this case `z` is called a _free variable_.

<font size="6px">
```r
f <- function(x, y) {
        x^2 + y / z
}
```
</font>

The _scoping rules of a language_ determine how values are assigned to free variables. Free variables are not formal arguments and are not local variables (assigned insided the function body).

Lexical Scoping
========================================================

Lexical scoping in R means that

>the values of __free variables__ are searched for in the environment in which the function was defined.

What is an environment?

- An _environment_ is a collection of (symbol, value) pairs, i.e. `x` is a symbol and `3.14` might be its value.
- Every environment has a parent environment; it is possible for an environment to have multiple “children”
    - the only environment without a parent is the __empty environment__
- A function + an environment = a __closure__ or __function closure__.

Lexical Scoping
========================================================

Searching for the value for a free variable:

- If the value of a symbol is not found in the environment in which a function was defined, then the search is continued in the _parent environment_.
- The search continues down the sequence of parent environments until we hit the _top-level environment_; this usually the global environment (workspace) or the namespace of a package.
- After the top-level environment, the search continues down the search list until we hit the _empty environment_. If a value for a given symbol cannot be found once the empty environment is arrived at, then an error is thrown.

Lexical Scoping
========================================================

Why does all this matter?

- Typically, a function is defined in the global environment, so that the values of free variables are just found in the user’s workspace
    - This behavior is logical for most people and is usually the “right thing” to do
- However, in R you can have functions defined _inside other functions_
    - In this case the environment in which a function is defined is the body of another function!

---

<font size="6px">

```r
# Create a function that returns another function
make.power <- function(n) {
        pow <- function(x) {
                x^n
        }
        pow
}

square <- make.power(2)
cube <- make.power(3)

square(3)
[1] 9
cube(3)
[1] 27

# The global environment content
environment()
<environment: R_GlobalEnv>
ls(environment())
[1] "cube"       "lm"         "make.power" "square"    

# The environment connected with square function
environment(square)
<environment: 0x000000000a2777a8>
ls(environment(square))
[1] "n"   "pow"
get("n", environment(square))
[1] 2

# The environment connected with cube function
environment(cube)
<environment: 0x000000000a2128f0>
ls(environment(cube))
[1] "n"   "pow"
get("n", environment(cube))
[1] 3
```
</font>

Lexical vs. Dynamic Scoping
========================================================

<font size="6px">

```r
y <- 10

f <- function(x) {
        y <- 2
        y^2 + g(x)
}

g <- function(x) {
        x*y
}

# What is the value of f(3)?
f(3)
[1] 34
```
</font>

---

- With __lexical scoping__ the value of `y` in the function `g` is looked up in the environment in which the function was defined, in this case the global environment, so the value of `y` is 10.
- With __dynamic scoping__, the value of `y` is looked up in the environment from which the function was _called_ (sometimes referred to as the _calling environment_ or __parent frame__). So the value of `y` would be 2.


When a function is _defined_ in the global environment and is subsequently _called_ from the global environment, then __the defining environment and the calling environment are the same__ and this can sometimes give the appearance of dynamic scoping.

Scoping: Summary
========================================================

- Binding Values to Symbol: how?
- Scoping Rules
- Lexical Scoping
    - Lexical Scoping vs. Dynamic Scoping