# Age-specific survival

Estimate age-specific survival, i.e. the probability of surviving at
least Nyear additional years, from the parameters outputs of a Basta
model.

## Usage

``` r
Sur_age(Lx, xv, Nyear = 1)
```

## Arguments

- Lx:

  `numeric` Matrix of posterior survivorship trajectories

- xv:

  `numeric` Age vector

- Nyear:

  `numeric` number of years to survive Default = 1#

## Value

a data frame including age, the mean and 95% credible interval of the
age specific probability to survive `Nyear` years.

## Examples

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_age(Lx, Nyear = 5, xv = c(0:10))
```
