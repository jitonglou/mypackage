A tutorial for creating a R package and publish to GitHub
================

## Documentation

## Installing the ‘mypackage’ package

Install the development version using **devtools** package:

``` r
devtools::install_github("jitonglou/mypackage")
```

## Quick usage overview

Load the package:

``` r
library(mypackage)
```

### Call functions in the package

``` r
add_one(1.5)
```

    ## [1] 2.5

``` r
add_one_cpp(1.5)
```

    ## [1] 2.5

### Benchmark functions

There are at least five functions/packages for measuring the running
time of R code (`Sys.time` and `system.time`) or benchmarking R code
(`rbenchmark`, `microbenchmark`, and `tictoc`). In this document, I take
`microbenchmark` as an example, and additional examples can be found in
[this blog](https://www.alexejgossmann.com/benchmarking_r/)

``` r
library(microbenchmark)

check_for_equal_results <- function(values) {
  tol <- 1e-8
  max_error <- abs(values[[1]]-values[[2]])
  max_error < tol
}

mbm <- microbenchmark(
  "r" = {y = add_one(1)},
  "cpp" = {y = add_one_cpp(1)},
   check = check_for_equal_results
)

mbm
```

    ## Unit: nanoseconds
    ##  expr  min   lq    mean median   uq   max neval cld
    ##     r  300  401  416.97    401  401  2501   100  a 
    ##   cpp 1101 1201 1606.03   1301 1502 18302   100   b

I used the function argument `check` to check for equality (up to a
maximal error of `1e-8`) of the results returned by the two functions.
If the results weren’t equal, `microbenchmark` would return an error
message.</br> A great feature of `microbenchmark` is the integration
with `ggplot2` for plotting benchmark results.

``` r
library(ggplot2)
autoplot(mbm)
```

![](README_files/figure-gfm/visualize%20mbm-1.png)<!-- -->

In this case, the function coded in C++ is slower than the default
addition operation in R.
