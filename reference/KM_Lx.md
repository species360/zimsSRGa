# Lx from Kaplan-Meier

Estimate Survivorship at given ages from the Kaplan-Meier table.

## Usage

``` r
KM_Lx(KM_tab, Age)
```

## Arguments

- KM_tab:

  `data.frame` The kaplan-Meier table including the columns: *Ages* and
  *ple*

- Age:

  `vector of numeric` Ages for which Lx is requested

## Value

a data frame including age, and Lx, giving the interpolated Kaplan-Meier
survivorship at the requested ages.

## Examples

``` r
KM_tab = data.frame( Ages = 1:10,
                     ple = sort(runif(10, 0, 1), decreasing = TRUE))
out <- KM_Lx(KM_tab, Age = c(3,6.5))
```
