# Checks required columns and the logical succession of dates, then formats the data for use in BaSTA.

Checks required columns and the logical succession of dates, then
formats the data for use in BaSTA.

## Usage

``` r
surv_Bastab(
  coresubset,
  DeathInformation,
  EarliestDate = NA,
  LatestDate = NA,
  OtherCovars = NA,
  ExcludeStillBirth = FALSE
)
```

## Arguments

- coresubset:

  `data.frame` including at least the following columns: *AnimalAnonID*,
  *binSpecies*, *Class*, *Order*, *Family*, *CommonName*, *BirthDate*,
  *DepartDate*, *EntryDate*, *MaxBirthDate*, *MinBirthDate*,
  *EntryType*, and *DepartType*.

- DeathInformation:

  `data.frame` including at least the following columns *AnimalAnonID*
  and *RelevantDeathInformationType*.

- EarliestDate:

  `character` Earliest date to include, in `"YYYY-MM-DD"` format, or
  `NA`.

- LatestDate:

  `character` Latest date to include, in `"YYYY-MM-DD"` format, or `NA`.

- OtherCovars:

  `character` vector of additional variables to include.

- ExcludeStillBirth:

  `logical` Whether stillborn and fetal death records should be
  excluded.

## Value

A `data.frame` formatted for BaSTA, with date columns renamed to
`Birth.Date`, `Min.Birth.Date`, `Max.Birth.Date`, `Entry.Date`,
`Depart.Date`, `Entry.Type`, and `Depart.Type`. Additional covariates
are kept if requested.

## Details

The function removes:

- individuals with missing values in `BirthDate`, `MinBirthDate`,
  `MaxBirthDate`, `EntryDate`, or `DepartDate`;

- individuals for which the sequence of dates is not logically
  consistent;

- stillborn or fetal death records, if requested;

- individuals with departure date earlier than `EarliestDate`;

- individuals with entry date later than `LatestDate`.

Departure dates later than `LatestDate` are truncated to `LatestDate`,
and the corresponding individuals are treated as right-censored.

## Examples

``` r
data(core)
data(deathinformation)
out<- surv_Bastab(core, DeathInformation = deathinformation,
                  EarliestDate = '1990-01-01', LatestDate = '2020-12-31', 
                  OtherCovars = "SexType", ExcludeStillBirth = TRUE)

```
