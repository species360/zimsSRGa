# Survival

``` r
library(zimsSRGa)
```

## Main survival Analysis

[`Sur_main()`](../reference/Sur_main.md) runs the survival analysis (see
next function : [`Sur_ana()`](../reference/Sur_ana.md)) and estimates
associated key survival metrics. In addition to the arguments of
[`Sur_ana()`](../reference/Sur_ana.md),
[`Sur_main()`](../reference/Sur_main.md) includes the main following
arguments:

- `DataCore` the core data of the species from ZIMS including at least
  the following columns *AnimalAnonID*, *BirthDate*, *DepartDate*,
  *EntryDate*, *MaxBirthDate*, *MinBirthDate*, *EntryType*,
  *DepartType*, *FirstHoldingInstitution*, *LastHoldingInstitution*,
  *SexType*, and *BirthType*.
- `DeathInformation` data.frame including at least the following columns
  *AnimalAnonID* and *RelevantDeathInformationType*
- `CalculateMetricsFrom` indicates which metrics should be calculated
  from which data: “Raw”, “Kaplan-Meier”, “Model”.
- `BirthType_sel`: The birth type to be selected: Captive, Wild, or All
- `PlotDir`: Directory to save the plots of the selected model

It returns a list including, depending on `CalculateMetricsFrom`: \* a
summary of the data selected \* The Kaplan-Meier estimator \* the BaSTA
fit of the selected model \* the DIC table comparing the fit of the
different models \* the remaining life expectancy per age \* key
survival metrics \* Statistics to check the fit of the selected model

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_main(toy_core, DeathInformation = toy_deathinformation, BirthType_sel = "All",
                Models = "GO", Shape = "simple",
                niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 10.43 secs.
#> 
#> Multiple simulations started...
#> 
#> R Version:  R version 4.5.3 (2026-03-11)
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> Library BaSTA loaded.
#> Library BaSTA loaded in cluster.
#> 
#> Stopping cluster
#> Simulations finished.
#> Total MCMC computing time: 6.53 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> 
#> 
#> Stopping cluster
#> Joining with `by = join_by(Ages)`
```

## Survival Analysis

This function runs BaSTA survival models on the data after checking
conditions for correct convergence of the models. It includes the
following arguments: \* `Data`including at least the following columns
*AnimalAnonID*, *BirthDate*, *DepartDate*, *EntryDate*, *MaxBirthDate*,
*MinBirthDate*, *EntryType*, *DepartType*, *FirstHoldingInstitution*,
*LastHoldingInstitution*, *SexType*, and *BirthType*. \*
`DeathInformation` data.frame including at least the following columns
*AnimalAnonID* and *RelevantDeathInformationType*. \*
`CalculateMetricsFrom` indicates which metrics should be calculated from
which data: “Raw”, “Kaplan-Meier”, “Model”. \* `Models` names of the
BaSTA Models to run: “GO”, “EX”, “LO” and/or “WE”. see ?basta for more
information. Default = “GO”. \* `Shape` shape of the BaSTA model:
“simple”, “Makeham”, “bathtub”. see ?basta for more information. Default
= “simple”.

The user should define different filters to select data and run the
survival analyses. The main ones are: \* `UncertDeath` Maximum
uncertainty accepted for death date, in days. \* `MinNSur` Minimum
number of individual records needed to run the survival analysis. \*
`MaxNSur` Maximum number of individual records to run the survival
analysis. \* `MinLx` Minimum reached survivorship from the raw
Kaplan-Meier analysis needed to run the survival analysis. \*
`MinBirthKnown` Minimum proportion of individuals with a known birth
month in the data. \* `MinInstitution` Minimum number of institutions
that should hold records to run the survival analysis.

It returns a list including and depending on `CalculateMetricsFrom`: \*
a summary of the data used \* The Kaplan-Meier estimator and associated
key metrics \* Key survival metrics from raw data \* the BaSTA fit of
the best model \* the DIC table comparing the different fit of the
Models

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_ana(toy_core,  DeathInformation = toy_deathinformation, Models = "GO", Shape = "simple",
               niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 10.30 secs.
#> 
#> Multiple simulations started...
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> Library BaSTA loaded.
#> Library BaSTA loaded in cluster.
#> 
#> Stopping cluster
#> Simulations finished.
#> Total MCMC computing time: 6.32 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.
```

## Key survival metrics

[`Sur_out()`](../reference/Sur_out.md) estimates key metrics from the
selected survival model and returns plots for the survival model. It
includes the main following arguments:

- `out`: the output of the survival analysis (returns from
  [`Sur_ana()`](../reference/Sur_ana.md))
- `PlotDir`: Directory to save the plots of the best model

In addition to the outputs of the survival analysis, it returns: \* Key
survival metrics estimated from either raw data, the Kaplan-Meier
estimator or the selected survival model. Estimates are from birth or
from age at sexual maturity. \* A check list \* the remaining life
expectancy per age (relex_from0) \* the probability to live 5 years more
(Sur5) \* Age-specific survival (Sur1) \* The monthly survival (Sur1m)

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_ana(toy_core,  DeathInformation = toy_deathinformation, 
               Models = "GO", Shape = "simple",
               niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 10.46 secs.
#> 
#> Multiple simulations started...
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> Library BaSTA loaded.
#> Library BaSTA loaded in cluster.
#> 
#> Stopping cluster
#> Simulations finished.
#> Total MCMC computing time: 6.52 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.

out <- Sur_out(out, ncpus = 3)
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> 
#> 
#> Stopping cluster
#> Joining with `by = join_by(Ages)`
```

## Create Basta data frame

This function formats the data prior to running BaSTA models. It checks
column names and the succession of dates for each individual. It
includes the following arguments:

- `coresubset` data frame including at least the following columns
  *AnimalAnonID*, *binSpecies*, *Class*, *Order*, *Family*,
  *CommonName*, *BirthDate*, *DepartDate*, *EntryDate*, *MaxBirthDate*,
  *MinBirthDate*, *EntryType*, and *DepartType*
- `DeathInformation` data.frame including at least the following columns
  *AnimalAnonID* and *RelevantDeathInformationType*
- `EarliestDate` Earlier date to be included.
- `LatestDate` Latest date to be included.
- `OtherCovars` Additional variables to include in the data
- `ExcludeStillBirth` Whether to exclude still births.

This function returns the subset dataset excluding individuals: \* with
NA in the columns BirthDate, MinBirthDate, MaxBirthDate, EntryDate, and
DepartDate. \* for which the dates from min birth date/entry date to
Depart date do not follow one another in a logical order. \* Still born
if required \* with depart date earlier than EarliestDate \* with entry
date later than LatestDate \* Depart dates later than LatestDate are
changed to latest date. These individuals are considered right-censored.

``` r
data(toy_core)
data(toy_deathinformation)
out<- surv_Bastab(toy_core, DeathInformation = toy_deathinformation,
                  EarliestDate = '1990-01-01', LatestDate = '2020-12-31', 
                  OtherCovars = "SexType", ExcludeStillBirth = TRUE)
```

## Estimate raw survivorship from life table

This function estimated the Kaplan-Meier table

``` r
entryAge = sample(c(1:10), 200, replace = TRUE)
data <- data.frame(
  entryAge = entryAge,
  deparAge =  entryAge + sample(c(0:10), 200, replace = TRUE),
  DepartType = sample(c('C', 'D'), 200, replace = TRUE))


out<-Sur_ple(data)
```

## Remaining life expectancy

[`Sur_relex()`](../reference/Sur_relex.md) estimates the remaining life
expectancy over ages. It includes the following arguments:

- `Lx` Survivorship
- `dx` precision for age (delta between successive age)
- `xv` Age vector

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_relex(Lx, dx = 1, xv = c(0:10))
```

## Age-specific survival

[`Sur_age()`](../reference/Sur_age.md) estimates age-specific survival.
It includes the following arguments:

- `Lx` Survivorship
- `xv` Age vector
- `Nyear` Number of year to survive. Default = 1

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_age(Lx, Nyear = 5, xv = c(0:10))
```

## Survivorship quantile

[`Sur_xx()`](../reference/Sur_xx.md) estimates the age at which xx% of
the population is still alive. It includes the following arguments:

- `Lx` Survivorship
- `xv` Age vector
- `xx` Proportion of the population still alive

``` r
Lx = matrix(c(seq(1,0,by = -0.1), 1,seq(0.5,0,length.out = 10)),nrow =2)
out <- Sur_xx(Lx,  xv = c(0:10), xx = 0.1)
```

## Survivorship

`Sur_Lx()` estimates the survivorship from parameter of the survival
model. It includes the following arguments:

- `sim` iteration number if the function is run in parallel
- `theMat` The posteriors estimates of the model parameter
- `model` Name of the basta model: “GO”, “EX”, “LO” and/or “WE”.
- `Shape` character Shape of the basta model: “simple”, “Makeham”,
  “bathtub”.
- `iseq` rows of themat to consider
- `xv` Age vector

## Age from Kaplan-Meier

[`KM_age()`](../reference/KM_age.md) estimates Ages at which Lx equals
given values from the Kaplan-Meier curve. It includes the following
arguments:

- `KM_tab`: The kaplan-Meier table including the columns: `Ages` and
  `ple`.
- `Lx`: Values of Lx for which age is requested

``` r
KM_tab = data.frame( Ages = 1:10,
                     ple = sort(runif(10, 0, 1), decreasing = TRUE))
out <- KM_age(KM_tab, Lx = c(0.5,0.1))
```

## Lx from Kaplan-Meier

[`KM_Lx()`](../reference/KM_Lx.md) estimates Lx at given ages from the
Kaplan-Meier table. It includes the following arguments:

- `KM_tab`: The kaplan-Meier table including the columns: `Ages` and
  `ple`.
- `Age`: Age for which Lx is requested

``` r
KM_tab = data.frame( Ages = 1:10,
                     ple = sort(runif(10, 0, 1), decreasing = TRUE))
out <- KM_Lx(KM_tab, Age = c(3,6.5))
```

## Gap analysis in longevity

[`select_Longthreshold()`](../reference/select_Longthreshold.md) runs a
gap analysis on the distribution of longevity and gives the threshold
value to use to avoid having gaps in longevity. It can focus on a given
sex using the following arguments:

- `Data` including at least the following columns *Birth.Date*,
  *Depart.Date*, *Entry.Date*, and *SexType*
- `SexCats` Male, Female or All
- `MinN` Minimum number of individuals to run the gap analysis
- `PlotDir` Directory to save the plots
- `PlotName` name of the graph to be saved

It returns the data with the selected sex and additional columns showing
which individuals are above the percentiles 95%, 99% and 99.9% and a
summary list including the threshold value selected for the distribution
of longevity. It plots the distribution of longevity with gaps if
`PlotDir` is given.

``` r
TempDir <- paste0(tempdir(check = TRUE),'/temp')
dir.create(TempDir)
data(toy_core) #### CHANGE DATASET WITH ONE EXCLUDING ABOVE95 99 99.9
out <- select_Longthreshold (Data = toy_core,  SexCats = "All", 
                             PlotDir = TempDir, PlotName = "Testudo_hermanni")
list.files(TempDir)
#> [1] "Testudo_hermanni_LongThres.pdf"
#reMove temporary folder
unlink(TempDir, recursive = TRUE)
```

### Find gaps in a continuous variable

`find_gaps` Looks for gaps within a continuous variable.Values of the
variable are rounded and gaps are found if one integer is not
represented. Gaps are returned only if the length of the gaps is 20%
higher than the starting age of this gap. It uses the following
arguments:

- `x` variable to analyze
- `MaxAlive` Return gaps higher than this maximum, only. Default = NA

It returns a data frame where each line is a gap described by its
initial/starting age, its final/ending age and its length

``` r
x = runif(10,0,40)
out<-find_gaps(x,MaxAlive = 5, plot = FALSE)
```
