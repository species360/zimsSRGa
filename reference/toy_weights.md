# Weights data

A simulated dataset including information about individual weights.

## Usage

``` r
data(toy_weights)
```

## Format

A data.frame object with 2273 rows and 14 variables:

- AnimalAnonID:

  Anonimized ID of the Animal

- RecordingInstitution:

  Anonimized ID of recording institution

- MeasurementType:

  Name of the measurement made

- RecordType:

  Type of record

- MeasurementValue:

  Estimated value of the measure

- EstimatedMeasurement:

  1 = The measurment was not measured but estimated/guessed

- ExcludedFromNorms:

- MeasurementDate:

  Estimate date of measurement

- MeasurementDateEstimateType:

  If measurement date is not known with certainty, it can be estimated
  using: "Undetermined" = Unkwnown / "Range" = any date between min and
  max measurement date / "ApproxAfter" = Birth date is later than the
  min measurement date / "AproxBefore" = Birth date is ealier than the
  max measurement date / "Approxvariance" = measurement date estimated
  at mean with an error of some days around the estimated date

- MeasurementDateEstimateStart:

  Measurement estimated birth date

- MeasurementDateEstimateEnd:

  Measurement estimated birth date

- CollectionScopeType:

  "Local" = Only private use or "Global" = Zims user use

- Age:

  Animal age in years at measurement date

- UnitOfMeasure:

  Unit of Measure = kilogram

## Source

Subset of a toy database created by the Species360 Development team in
08-2024
