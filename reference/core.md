# Animal data

A simulated dataset including information about individual animals.

## Usage

``` r
data(core)
```

## Format

A data.frame object with 1000 rows and 10 variables:

- AnimalAnonID:

  Anonimized ID of the Animal

- Class:

  Taxonomy: Class

- Order:

  Taxonomy: Order

- Family:

  Taxonomy: Family

- SpeciesName:

  Taxonomy: SpeciesName

- CommonName:

  Point where observation conditions have been monitored

- FirstHoldingInstitution:

  Anonimized ID of the first institution holding the animal

- FirstCollectionScopeType:

  First collection can be "Local" = Only private use or "Global" = Zims
  user use

- LastHoldingInstitution:

  Anonimized ID of the last institution holding the animal

- LastCollectionScopeType:

  Last collection can be "Local" = Only private use or "Global" = Zims
  user use

- AnyLocalCollectionFlag:

  Whether the individual has ever been in a "local" (= Private use)
  collection in his life

- LatitudeZone:

- BirthDate:

  Estimated birth date

- BirthDateEstimateType:

  If birth date is not known with certainty, it can be estimated using:
  "Undetermined" = Unkwnown / "Range" = any date between min and max
  birth date / "ApproxAfter" = Birth date is later than the min birth
  date / "AproxBefore" = Birth date is ealier than the max birth date /
  "Approxvariance" = birth date estimated at mean with an error of some
  days around the estimated date

- BirthObserved:

  Whether the birth was observed

- FirstAcquisitionDate:

  Date of first acquisition of the animal

- SexType:

  Sex

- DeathDate:

  Estimated death date

- DeathDateEstimateType:

  If death date is not known with certainty, it can be estimated, see
  BirthDateEstimateType

- GlobalStatus:

  Current status of the animal

- LastCommentEntryDate:

  Last date a measure/observation/action included in ZIMS was made for a
  given animal

- LastTXDate:

  Last transaction date

- RelevantDeathInformationType:

  Information about cause of death

- MinBirthDate:

  Minimum estimated birth date

- MaxBirthDate:

  Maximum estimated birth date

- MinDeathDate:

  Minimum estimated death date

- MaxDeathDate:

  Maximum estimated death date

- EntryDate:

  Entry date in Zims

- EntryType:

  Whether the indivual entered in Zims at birth "b" or later "T"

- DepartDate:

  Depart Date from Zims

- DepartType:

  Whether the indivual exit Zims at death "D" or was censored/lost to
  follow up "C"

- above95:

  1 = the individual is within the oldest 5% of the distribution of
  individual lifespans

- above99:

  1 = the individual is within the oldest 1% of the distribution of
  individual lifespans

- above99.9:

  1 = the individual is within the oldest 0.1% of the distribution of
  individual lifespans

## Source

Subset of a toy database created by the Species360 Development team in
08-2024
