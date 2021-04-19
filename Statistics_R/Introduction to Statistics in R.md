# Summary Statistics

## Data type

1. Quantitative
  - Continuous
  - Discrete

2. Qualitative
  - Nominal: no order
  - Ordinal: has order

## Measurement of center

- Mean: sum/count

```r
mean(df$column)
```

- Median: is the value where 50% of the data is lower than it, and 50% is higher

```r
# sorting and taking the middle value
sort(df$column)[middle_value]
# use the median function
median(df$column)
```

- Mode: The most frequent value. Can be used for categorical data, even if unordered.

```r
df %>% count(column, sort=TRUE)
```

## Measure of spread

Keywords/Topics of interest:

- Variance
- Standard deviation
- Mean absolute deviation (MAD)
- Quantiles/quartiles/percentiles
- Inter-quantile range (IQR)
- Box plot

