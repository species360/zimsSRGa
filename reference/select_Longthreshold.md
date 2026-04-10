# Gap analysis in longevity

Run a gap analysis on the distribution of longevity and give the
threshold value to use to avoid having gaps in longevity.

## Usage

``` r
select_Longthreshold(
  Data,
  SexCats = "All",
  MinN = 50,
  PlotDir = NULL,
  PlotName = ""
)
```

## Arguments

- Data:

  `data.frame` including at least the following columns *BirthDate*
  (`date`), *DepartDate* (`date`), *EntryDate* (`date`), and *SexType*

- SexCats:

  `character` Male, Female or All.

- MinN:

  `numeric` Minimum number of individuals to run the gap analysis.

- PlotDir:

  `character` Directory to save the plots.

- PlotName:

  `character` name of the graph to be saved.

## Value

A list including

- the data with the selected sex and additional columns showing which
  individuals are above the percentiles 95%, 99% and 99.9%

- A summary list with:

  - Sex = the sex selected

  - Nselect: the number of individuals of this sex

  - Nlifespan : the number of individuals with estimated lifespan (i.e.
    estimated birth dates, censored individuals are also included)

  - GapThresh : The threshold value selected for the distribution of
    longevity

  - NThres : the number of individuals selected using this threshold

- Plot and save the longevity distribution with gaps if PlotDir is
  given.

## Examples

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
