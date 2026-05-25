# Survival Analysis

Run BaSTA Models on the data after checking conditions for correct
convergence of the Models

## Usage

``` r
Sur_ana(
  Data,
  DeathInformation,
  CalculateMetricsFrom = c("Raw", "Kaplan-Meier", "Model"),
  Models = "GO",
  Shape = "simple",
  MinAge = 0,
  OutlLev1 = 100,
  OutlLevmin = 95,
  MinDate = "1980-01-01",
  MinNSur = 50,
  MaxNSur = NULL,
  UncertDeath = 365,
  MinLx = 0.1,
  MinBirthKnown = 0.3,
  MinInstitution = 1,
  LastDead = FALSE,
  niter = 25000,
  burnin = 5001,
  thinning = 20,
  nchain = 3,
  ncpus = 2
)
```

## Arguments

- Data:

  `data.frame` including at least the following columns *AnimalAnonID*,
  *BirthDate*, *DepartDate*, *EntryDate*, *MaxBirthDate*,
  *MinBirthDate*, *EntryType*, *DepartType*, *FirstHoldingInstitution*,
  *LastHoldingInstitution*, *SexType*, and *BirthType*.

- DeathInformation:

  `data.frame` including at least the following columns *AnimalAnonID*
  and *RelevantDeathInformationType*

- CalculateMetricsFrom:

  `vector of characters` indicates which metrics should be calculated
  from which data: "Raw", "Kaplan-Meier", "Model".

- Models:

  `vector of characters` names of the BaSTA models to run: "GO", "EX",
  "LO" and/or "WE". see ?basta for more information.

- Shape:

  `character` Shape of the BaSTA model: "simple", "Makeham", "bathtub".
  see ?basta for more information.

- MinAge:

  `numeric` Ages at which the survival analysis should start, in years.
  see ?basta for more information.

- OutlLev1:

  `numeric` Start threshold to select individuals based on the longevity
  distribution: 100%, 99.9, 99 or 95%. This number must decrease when
  many errors are expected in longevity data.

- OutlLevmin:

  `numeric` Minimum threshold to select individuals based on the
  longevity distribution: 100%, 99.9, 99 or 95%.

- MinDate:

  `character 'YYYY-MM-DD'` Earlier date to include data.

- MinNSur:

  `numeric` Minimum number of individual records needed to run the
  survival analysis.

- MaxNSur:

  `numeric` Maximum number of individual records to run the survival
  analysis.

- UncertDeath:

  `numeric`: Maximum uncertainty accepted for death date, in days.

- MinLx:

  `numeric` Value used for longevity threshold and for checks. between 0
  and 1. Minimum reached survivorship from the raw Kaplan-Meier
  analysis. This number avoids running survival analysis if there are
  too few dead individuals in the data. Lower is better.

- MinBirthKnown:

  `numeric` between 0 and 1. Minimum proportion of individuals with a
  known birth month in the data to run the survival analysis.

- MinInstitution:

  `numeric` Minimum number of institutions that should hold records to
  run the survival analysis.

- LastDead:

  `logical` Whether the longest lived individuals should be considered
  dead.

- niter:

  `numeric`. Parameter to run the model: number of MCMC iterations. see
  ?basta for more information.

- burnin:

  `numeric` Parameter to run the model: number of iterations removed so
  that the model has time to converge. see ?basta for more information.

- thinning:

  `numeric` Parameter to run the model: number of iterations to run
  before saving a set of parameters. see ?basta for more information.

- nchain:

  `numeric` Parameter to run the model: Number of chains to run.

- ncpus:

  `numeric` Number of computer cores to use.

## Value

A list with components `summary`, `metrics`, `bastaRes`, `DICmods`,
`KM_estimator`, and `bastatab`. Some components may be `NULL` depending
on `CalculateMetricsFrom` and whether model fitting succeeded.

- a summary of the data used:

  - NSelect: Number of individuals selected from filters

  - NUncertdeath: Number of individuals selected after filter
    uncertainty in death

  - NBasta: Number of data (individuals) selected for the BaSTA/survival
    analysis

  - Ndead:Number of selected individuals with known age of death used in
    the BaSTA/survival analysis

  - Nrc: Number of selected right-censored individuals

  - N8090: Number of selected individuals born between 1980 and 1990

  - N9000: Number of selected individuals born between 1990 and 2000

  - N0010: Number of selected individuals born between 2000 and 2010

  - N1020: Number of selected individuals born between 2010 and 2020

  - N2030: Number of selected individuals born between 2020 and 2030

  - QBD10: 10% Quantile of birth date distribution among selected
    individuals

  - QBD50: Median of birth date distribution among selected individuals

  - QBD90: 90% Quantile of birth date distribution among selected
    individuals

  - BDincert: average uncertainty in birth date: in days among selected
    individuals

  - maxAge: Maximum age of selected known age individuals

  - maxAlive:Maximum number of years spent ex situ among selected
    individuals

  - lxMin: Minimum survivorship reached with the raw Kaplan-Meier model

  - outLev: threshold selected for the distribution of longevity: 100%,
    99.9%, 99% or 95%

  - analyzed: a logical indicated if the basta survival model was
    performed

  - If the basta survival model was not performed, an error and its
    number (Nerr) are returned: The possibility for this function are:
    2/Nuncertdeath \< MinNSur; 3/ lxMin\> 0.75; 4/NBasta = 0; 5/ %known
    births \< MinBirthKnown; 6/Number of institutions \< MinInstitution;
    7/Nbasta \< MinNSur; 8/Nbasta \> MaxNSur; 9/no DIC from Basta;
    10/Gof martingale does not fit

- the Kaplan-Meier table

- Key survival metrics including Mean life expectancy (MLE & Ex), median
  life expectancy (L50) and age at which 90% of the individual died
  (Longevity = L90) estimated from raw data and from the Kaplan-Meier
  Estimator. Estimates include ages from birth or from age at sexual
  maturity (if given). First year survival, First month survival,
  Entropy (H and Epx = -log(H)), coefficient of variation (CV) and Gini
  coefficient (G) are also estimated from the Kaplan-Meier estimator.

- the basta fit of the best model

- the DIC table comparing the different fit of the Models

## Examples

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_ana(toy_core,  DeathInformation = toy_deathinformation, Models = "GO", Shape = "simple",
               niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 9.26 secs.
#> 
#> Multiple simulations started...
#> 
#> Warning: Unknown option on commandline: --file
#> R Version:  R version 4.6.0 (2026-04-24) 
#> 
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> Library BaSTA loaded.
#> Library BaSTA loaded in cluster.
#> 
#> Stopping cluster
#> Simulations finished.
#> Total MCMC computing time: 3.75 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.
```
