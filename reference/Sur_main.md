# Main survival analysis

Run the survival analysis and calculates key survival metrics

## Usage

``` r
Sur_main(
  DataCore,
  DeathInformation,
  CalculateMetricsFrom = c("Raw", "Kaplan-Meier", "Model"),
  BirthType_sel = "All",
  Models = "GO",
  Shape = "simple",
  MinAge = 0,
  MaxAge = 120,
  OutlLev1 = NA,
  OutlLevmin = 95,
  UncertDeath = 365,
  MinDate = "1980-01-01",
  MinNSur = 50,
  MaxNSur = NULL,
  MinInstitution = 2,
  MinLx = 0.1,
  MinBirthKnown = 0.3,
  MinMLE = 0.1,
  MaxLE = 2,
  LastDead = FALSE,
  niter = 25000,
  burnin = 5001,
  thinning = 20,
  nchain = 3,
  ncpus = 2,
  PlotDir = NULL,
  PlotName = ""
)
```

## Arguments

- DataCore:

  `data.frame` including at least the following columns *AnimalAnonID*,
  *BirthDate*, *DepartDate*, *EntryDate*, *MaxBirthDate*,
  *MinBirthDate*, *EntryType*, *DepartType*, *FirstHoldingInstitution*,
  *LastHoldingInstitution*, *SexType*, and *BirthType*.

- DeathInformation:

  `data.frame` including at least the following columns *AnimalAnonID*
  and *RelevantDeathInformationType*.

- CalculateMetricsFrom:

  `vector of characters` indicates which metrics should be calculated
  from which data: "Raw", "Kaplan-Meier", "Model".

- BirthType_sel:

  `character` The birth type to be selected: Captive, Wild, or All.

- Models:

  `vector of characters` names of the BaSTA Models to run: "GO", "EX",
  "LO" and/or "WE". see ?basta for more information.

- Shape:

  `character` Shape of the BaSTA model: "simple", "Makeham", "bathtub".
  see ?basta for more information.

- MinAge:

  `numeric` Ages at which the survival analysis should start in years.
  see ?basta for more information.

- MaxAge:

  `numeric` Maximum possible age in years. Only used for model
  predictions. This argument is not used to select data.

- OutlLev1:

  `numeric` Start threshold to select individuals based on the longevity
  distribution: 100%, 99.9, 99 or 95%. This number must decrease when
  many errors are expected in longevity data.

- OutlLevmin:

  `numeric` Minimum threshold to select individuals based on the
  longevity distribution: 100%, 99.9, 99 or 95%.

- UncertDeath:

  `numeric`: Maximum uncertainty accepted for death date, in days.

- MinDate:

  `character 'YYYY-MM-DD'` Earlier date to include data.

- MinNSur:

  `numeric` Minimum number of individual records needed to run the
  survival analysis.

- MaxNSur:

  `numeric` Maximum number of individual records allowed to run the
  survival analysis.

- MinInstitution:

  `numeric` Minimum number of institutions that should hold records to
  run the survival analysis.

- MinLx:

  `numeric` Value used for longevity threshold and for checks. between 0
  and 1. Minimum reached survivorship from the raw Kaplan-Meier
  analysis. This number avoids running survival analysis if there are
  too few dead individuals in the data. Lower is better.

- MinBirthKnown:

  `numeric` between 0 and 1. Minimum proportion of individuals with a
  known birth month in the data to run the survival analysis.

- MinMLE:

  `numeric` Value used for checks. Minimum survivorship allowed at mean
  life expectancy. Between 0 and 1.

- MaxLE:

  `numeric` Value used for checks. Maximum remaining life expectancy at
  last observed age. In years.

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

- PlotDir:

  `character` Directory to save the plots. If NULL, no plot is saved

- PlotName:

  `character` Name used to save the plots.

## Value

A named list with one element per value of `MinAge`, named
`from<MinAge>` (e.g. `from0`). Each element is itself a list containing
`summary`, `metrics`, `bastaRes`, `DICmods`, `KM_estimator`, `bastatab`,
`relex_from0`, `Sur1`, `Sur5`, `Sur1m`, and `check`. Some components may
be `NULL` depending on `CalculateMetricsFrom` and whether model fitting
was successful

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

  - analyzed: logical indicating whether the BaSTA survival model was
    fitted.

  - Nerr, error: error code and message when the model was not fitted or
    when a goodness-of-fit check failed.

- the basta fit of the best model

- the DIC table comparing the different fit of the Models

- Key survival metrics including Mean life expectancy (MLE & Ex), median
  life expectancy (L50) and age at which 90% of the individual died
  (Longevity = L90) estimated from raw data, the Kaplan-Meier estimator,
  and the survival model. Estimates include ages from birth or from age
  at sexual maturity (if given). First year survival, First month
  survival, Entropy (H and Epx = -log(H)), coefficient of variation (CV)
  and Gini coefficient (G) are also estimated from the Kaplan-Meier
  estimator and the survival model.

- Checks of the fit for the selected survival model:

  - Gof_KM_coeff1: Percentage of age points where the KM estimator is
    outside of the 95% CI of the survivorship estimated from the
    survival model.

  - Gof_KM_coeff2: Maximum sum of same sign residuals between KM
    estimator and survival model

  - Gof_Martingale: Goodness of fit using Martingale residuals

  - LxatMLE: Estimated survivorship at mean life expectancy

  - LEmaxOage: Life expectancy at max observed age

  - KMMinLx: Minimum survivorship reached by the Kaplan-Meier estimator

- The remaining life expectancy per age (relex_from0)

- The probability to live 5 years more (Sur5)

- Age-specific yearly survival (Sur1)

- Monthly survival (Sur1m)

- If PlotDir is filled, 2 plots are produced:

  - One showing the convergence of chains, together with estimated
    survivorship and survival and mortality rates.

  - The second showing remaining life-expectancy, and the age-specific
    probabilities to live 5 years and 1 year more.

## Examples

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_main(toy_core, DeathInformation = toy_deathinformation, BirthType_sel = "All",
                Models = "GO", Shape = "simple",
                niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 10.35 secs.
#> 
#> Multiple simulations started...
#> 
#> Warning: Unknown option on commandline: --file
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> Library BaSTA loaded.
#> Library BaSTA loaded in cluster.
#> 
#> Stopping cluster
#> Simulations finished.
#> Total MCMC computing time: 4.12 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.
#> Warning: Unknown option on commandline: --file
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> 
#> Stopping cluster
#> Joining with `by = join_by(Ages)`
```
