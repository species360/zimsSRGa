
<!-- README.md is generated from README.Rmd. Please edit that file -->

# zimsSRGa

<!-- badges: start -->

[![R-CMD-check](https://github.com/species360/zimsSRGa/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/species360/zimsSRGa/actions/workflows/R-CMD-check.yaml)
[![Codecov test
coverage](https://codecov.io/gh/species360/zimsSRGa/graph/badge.svg)](https://app.codecov.io/gh/species360/zimsSRGa)
<!-- badges: end -->

zimsSRGa can be used to run the main demographic analyses: survival,
reproduction and growth analytics from zims data. The data need to be
cleaned before using this package.

## Installation

You can install the development version of zimsSRGa from
[GitHub](https://github.com/) with:

``` r
# install.packages("pak")
pak::pak("species360/zimsSRGa")
```

## Documentation

Full documentation website on: <https://species360.github.io/zimsSRGa>

# Tutorial for survival

``` r
library(zimsSRGa)

#Filters -----------------------------------------------------------------------
# Earliest date to include records
MinDate <- "1980-01-01"
# Birth Type of Animals: "Captive", "Wild" or "All"
BirthType = "Captive"
# Minimum number of individuals to run the Survival Analysis
MinN <- 50
# Maximum threshold in the longevity distribution to use
MaxOutl <- 99 
# Minimum number of Institutions that hold individuals from one species
MinInstitution = 2 
# Maximum uncertainty accepted for death dates, in days
UncertDeath = 365
# Maximum possible age
MaxAge = 120
# Age when to start the analysis
MinAge = 0


# Survival Models ------------------------------------------------------------
# Survival Models to run: "GO", "LO", "EX" or/and "WE"
ModelsSur <- c("GO", "LO")
# Shape of the survival model: "simple", "bathtub" or "Makeham"
Shape = "bathtub"

# Number of CPUS:
ncpus <- 4
# MCMC settings:
niter <- 10000
burnin <- 3001
thinning <- 20
nchain <- 3

# Conditions to run the survival analysis
MinNSur = 50 #Minimum number of individuals
MaxNSur = 1000 #Maximum number of individuals
MinLx = 0.1  #Minimum survivorship reach by life table (Kaplan-Meier estimator)
MinBirthKnown = 0.3 #Minimum proportions of known birth dates (within a month)
#Goodness of fit
MinMLE = 0.1 #Minimum survivorship at Mean life expectancy
MaxLE = 2     #Maximum remaining life expectancy at max age

data(core)
data(deathinformation)
out = list()
# Run survival Analysis---------------------------------------------------------
    ## Loop over sexes
    for (sx in c("Male", "Female")){
      print(sx)
      #Check for gaps in longevity distribution to define a threshold 
      #after which individuals will be deleted (possible outiers or errors in data)
      sexDat <- select_Longthreshold( core,  SexCats = sx, 
                                      #PlotDir= glue::glue("{PlotDir}/"),
                                      MinN = MinN ,
                                      PlotName = glue::glue("plot_{sx}") )
      OutlLev1 = min(sexDat$summar$GapThresh,MaxOutl, na.rm = T)
      
      
      if(nrow(sexDat$data)>0){ 
        out[[sx]] <-  Sur_main(DataCore = sexDat$data,  
                               DeathInformation = deathinformation,
                               BirthType = BirthType, 
                               # PlotDir = glue::glue("{PlotDir}/"),
                               MaxAge = MaxAge, MinAge = MinAge,
                               Models = ModelsSur, Shape= Shape, 
                               OutlLev1 =OutlLev1,
                               MinMLE = MinMLE, MaxLE =  MaxLE,
                               MinDate = MinDate, MinNSur = MinNSur, MaxNSur = MaxNSur, 
                               MinInstitution = MinInstitution,UncertDeath= UncertDeath,
                               MinLx = MinLx , MinBirthKnown = MinBirthKnown, 
                               niter = niter, burnin = burnin, 
                               thinning = thinning, nchain = nchain, 
                               ncpus = ncpus, 
                               # PlotName = glue("plot{sx}") 
                               )
      }
    }
```
