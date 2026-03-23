# Survivorship quantile

Estimate the age at which survivorship reaches a specified level xx

## Usage

``` r
Sur_xx(Lx, xv, xx = 0.5)
```

## Arguments

- Lx:

  `numeric` Survivorship

- xv:

  `numeric` Age vector

- xx:

  `numeric` Proportion of the population still alive. Between 0 and 1.

## Value

a list including the mean and 95% credible interval age at which xx% of
the population is still alive

## Examples

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_xx(Lx,  xv = c(0:10), xx = 0.1)
```
