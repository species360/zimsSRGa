# Create output and plots for the survival analysis

estimates key metrics from the selected survival model and returns
illustrating plots of the survival model.

## Usage

``` r
Sur_out(
  out,
  MinAge = 0,
  MinMLE = 0.1,
  MaxLE = 2,
  MinLx = 0.1,
  MaxAge = 120,
  ncpus = 2,
  PlotDir = NULL,
  PlotName = ""
)
```

## Arguments

- out:

  `list` output of the survival analysis, returns from
  [`Sur_ana()`](Sur_ana.md)

- MinAge:

  `numeric` Age at which the survival analysis should start, in years.
  see ?basta for more information.

- MinMLE:

  `numeric` Value used for checks. Minimum survivorship allowed at mean
  life expectancy. Between 0 and 1.

- MaxLE:

  `numeric` Value used for checks. Maximum remaining life expectancy at
  last observed age. In years.

- MinLx:

  `numeric` Value used for longevity threshold and for checks. between 0
  and 1. Minimum reached survivorship from the raw Kaplan-Meier
  analysis. This number avoids running survival analysis if there are
  too few dead individuals in the data. Lower is better.

- MaxAge:

  `numeric` Maximum possible age in years. Only used for model
  predictions. This argument is not used to select data.

- ncpus:

  `numeric` Number of CPU cores to use.

- PlotDir:

  `character` Directory to save the plots. If `NULL`, no plot is saved

- PlotName:

  `character` Name used to save the plots.

## Value

The output list of the survival analysis including, in addition:

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

  - Gof_Martingal_KM: Value of the test that must be compared to a
    kolmogorov Smirnov table and test telling if the model fits

  - LxatMLE: Estimated survivorship at mean life expectancy

  - LEmaxOage: Life expectancy at max observed age

  - KMMinLx: Minimum survivorship reached by the Kaplan-Meier estimator

- The remaining life expectancy per age (relex_from0)

- The probability to live 5 years more (Sur5)

- Age-specific survival (Sur1)

- Monthly survival (Sur1m)

- Plots if requested

  - One showing the convergence of chains, together with estimated
    survivorship and survival and mortality rates.

  - The second showing remaining life-expectancy, and the age-specific
    probability to live 1 year more.

## Examples

``` r
data(toy_core)
data(toy_deathinformation)
out <- Sur_ana(toy_core,  DeathInformation = toy_deathinformation, 
               Models = "GO", Shape = "simple",
               niter = 1000, burnin = 101, thinning = 10, nchain = 3, ncpus = 3)
#> [1] "GO"
#> 
#> Running sequence to find jump SDs... Done
#> Total jump SDs computing time: 9.73 secs.
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
#> Total MCMC computing time: 3.82 secs.
#> 
#> Calculating summary statistics... Done.
#> All parameters converged properly.
#> Constructing life table... done.

out <- Sur_out(out, ncpus = 3)
#> Warning: Unknown option on commandline: --file
#> snowfall 1.84-6.3 initialized (using snow 0.4-4): parallel execution on 3 CPUs.
#> 
#> Stopping cluster
#> Joining with `by = join_by(Ages)`
```
