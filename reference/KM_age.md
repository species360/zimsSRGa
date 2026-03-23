# Age from Kaplan-Meier

Estimate Ages at which Lx equals given values from the Kaplan-Meier
table. Ages are linearly interpolation between adjacent KM.

## Usage

``` r
KM_age(KM_tab, Lx)
```

## Arguments

- KM_tab:

  `data.frame` The kaplan-Meier table including the columns: *Ages* and
  *ple*

- Lx:

  `numeric vector` Values of Lx for which age is requested

## Value

a data frame including Lx and Age

## Examples

``` r
KM_tab = data.frame( Ages = 1:10,
                     ple = sort(runif(10, 0, 1), decreasing = TRUE))
out <- KM_age(KM_tab, Lx = c(0.5,0.1))
```
