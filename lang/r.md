# R

## read data
Read data from clipboard in OS X

```r
df = read.delim(pipe("pbpaste"), sep=",")
```

## dplyr
Summarize data, go from long to wide, replace `NA`s with 0

```r
df %>%
  group_by(var1, var2) %>%
  summarize(count = n()) %>%
  spread(var2, count) %>%    # tidyr
  replace(is.na(.), 0)
```

## view source
Returns source for S3/S4 functions. If using RStudio, place cursor over function and press the `F2` key to automatically pull up source.

```r
# s3
getAnywhere(power.prop.test)
# s4
getMethod("chol2inv", "diagonalMatrix")
```
