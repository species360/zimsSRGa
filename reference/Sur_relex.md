# Remaining life expectancy

Estimate remaining life expectancy over ages.

## Usage

``` r
Sur_relex(Lx, dx = 0.01, xv)
```

## Arguments

- Lx:

  `numeric` Matrix of posterior survivorship trajectories

- dx:

  `numeric` Precision for age

- xv:

  `numeric` Age vector (same length as Lx)

## Value

a data frame including age, the mean and 95% credible interval of the
remaining life expectancy

## Examples

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_relex(Lx, dx = 1, xv = c(0:10))
```
