# R

Read file from clipboard in OS X
```r
df = read.delim(pipe("pbpaste"), sep=",")
```

## dplyr
summarize data, go from long to wide, replace NAs with 0
```
df %>%
  group_by(var1, var2) %>%
  summarize(count = n()) %>%
  spread(var2, count) %>%    # tidyr
  replace(is.na(.), 0)
```