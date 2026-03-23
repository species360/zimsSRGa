# Kaplan-Meier estimator

Compute the Kaplan-Meier product-limit estimator from individual entry
and departure ages.

## Usage

``` r
Sur_ple(Data)
```

## Arguments

- Data:

  `data.frame` including at least the following columns *deparAge*,
  *entryAge* (`date`), and *DepartType*

## Value

A data frame with columns:

- Ages: event times

- ple: Kaplan-Meier survivorship estimate

- event : number of deaths at each age

- Snd: Greenwood-type cumulative variance term used downstream

## Examples

``` r
entryAge = sample(c(1:10), 200, replace = TRUE)
data <- data.frame(
  entryAge = entryAge,
  deparAge =  entryAge + sample(c(0:10), 200, replace = TRUE),
  DepartType = sample(c('C', 'D'), 200, replace = TRUE))


out<-Sur_ple(data)
```
