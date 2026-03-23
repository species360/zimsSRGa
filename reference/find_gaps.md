# Find gaps in an integer variable

Looks for gaps within a continuous variable. Values of the variable are
rounded and gaps are found if one integer is not represented. Gaps are
returned only if the length of the gaps is 20% higher than the starting
age of this gap.

## Usage

``` r
find_gaps(x, MaxAlive = NA, plot = FALSE, ...)
```

## Arguments

- x:

  `vector of numeric` variable to analyze

- MaxAlive:

  `numeric` Return gaps higher than this maximum, only.

- plot:

  `logical` whether to actually plot the histogram

- ...:

  arguments pass to hist()

## Value

A data frame where each line is a gap described by its initial/starting
age, its final/ending age and its length

## Examples

``` r
x = runif(10,0,40)
out<-find_gaps(x,MaxAlive = 5, plot = FALSE)
```
