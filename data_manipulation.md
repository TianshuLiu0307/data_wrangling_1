data_manipulation with ‘dplyr’
================
2022-09-22

``` r
litters_data = read_csv("data/FAS_litters.csv", col_types = "ccddiiii")
litters_data = janitor::clean_names(litters_data)
pups_data = read_csv("./data/FAS_pups.csv", col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
```

## Select Cols

``` r
select(litters_data, group, litter_number)
## # A tibble: 49 × 2
##    group litter_number  
##    <chr> <chr>          
##  1 Con7  #85            
##  2 Con7  #1/2/95/2      
##  3 Con7  #5/5/3/83/3-3  
##  4 Con7  #5/4/2/95/2    
##  5 Con7  #4/2/95/3-3    
##  6 Con7  #2/2/95/3-2    
##  7 Con7  #1/5/3/83/3-3/2
##  8 Con8  #3/83/3-3      
##  9 Con8  #2/95/3        
## 10 Con8  #3/5/2/2/95    
## # … with 39 more rows
select(litters_data, group:gd_of_birth)
## # A tibble: 49 × 5
##    group litter_number   gd0_weight gd18_weight gd_of_birth
##    <chr> <chr>                <dbl>       <dbl>       <int>
##  1 Con7  #85                   19.7        34.7          20
##  2 Con7  #1/2/95/2             27          42            19
##  3 Con7  #5/5/3/83/3-3         26          41.4          19
##  4 Con7  #5/4/2/95/2           28.5        44.1          19
##  5 Con7  #4/2/95/3-3           NA          NA            20
##  6 Con7  #2/2/95/3-2           NA          NA            20
##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
##  8 Con8  #3/83/3-3             NA          NA            20
##  9 Con8  #2/95/3               NA          NA            20
## 10 Con8  #3/5/2/2/95           28.5        NA            20
## # … with 39 more rows
select(litters_data, -pups_survive)
## # A tibble: 49 × 7
##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_…¹ pups_…²
##    <chr> <chr>                <dbl>       <dbl>       <int>        <int>   <int>
##  1 Con7  #85                   19.7        34.7          20            3       4
##  2 Con7  #1/2/95/2             27          42            19            8       0
##  3 Con7  #5/5/3/83/3-3         26          41.4          19            6       0
##  4 Con7  #5/4/2/95/2           28.5        44.1          19            5       1
##  5 Con7  #4/2/95/3-3           NA          NA            20            6       0
##  6 Con7  #2/2/95/3-2           NA          NA            20            6       0
##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20            9       0
##  8 Con8  #3/83/3-3             NA          NA            20            9       1
##  9 Con8  #2/95/3               NA          NA            20            8       0
## 10 Con8  #3/5/2/2/95           28.5        NA            20            8       0
## # … with 39 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth
select(litters_data, New_Group = group, LiTtEr_NuMbEr = litter_number)
## # A tibble: 49 × 2
##    New_Group LiTtEr_NuMbEr  
##    <chr>     <chr>          
##  1 Con7      #85            
##  2 Con7      #1/2/95/2      
##  3 Con7      #5/5/3/83/3-3  
##  4 Con7      #5/4/2/95/2    
##  5 Con7      #4/2/95/3-3    
##  6 Con7      #2/2/95/3-2    
##  7 Con7      #1/5/3/83/3-3/2
##  8 Con8      #3/83/3-3      
##  9 Con8      #2/95/3        
## 10 Con8      #3/5/2/2/95    
## # … with 39 more rows
select(litters_data, starts_with("gd"))
## # A tibble: 49 × 3
##    gd0_weight gd18_weight gd_of_birth
##         <dbl>       <dbl>       <int>
##  1       19.7        34.7          20
##  2       27          42            19
##  3       26          41.4          19
##  4       28.5        44.1          19
##  5       NA          NA            20
##  6       NA          NA            20
##  7       NA          NA            20
##  8       NA          NA            20
##  9       NA          NA            20
## 10       28.5        NA            20
## # … with 39 more rows
select(litters_data, ends_with("t"))
## # A tibble: 49 × 2
##    gd0_weight gd18_weight
##         <dbl>       <dbl>
##  1       19.7        34.7
##  2       27          42  
##  3       26          41.4
##  4       28.5        44.1
##  5       NA          NA  
##  6       NA          NA  
##  7       NA          NA  
##  8       NA          NA  
##  9       NA          NA  
## 10       28.5        NA  
## # … with 39 more rows
select(litters_data, -group, litter_number, gd_of_birth, everything())
## # A tibble: 49 × 8
##    litter_number   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴ group
##    <chr>                <dbl>       <dbl>    <int>   <int>   <int>   <int> <chr>
##  1 #85                   19.7        34.7       20       3       4       3 Con7 
##  2 #1/2/95/2             27          42         19       8       0       7 Con7 
##  3 #5/5/3/83/3-3         26          41.4       19       6       0       5 Con7 
##  4 #5/4/2/95/2           28.5        44.1       19       5       1       4 Con7 
##  5 #4/2/95/3-3           NA          NA         20       6       0       6 Con7 
##  6 #2/2/95/3-2           NA          NA         20       6       0       4 Con7 
##  7 #1/5/3/83/3-3/2       NA          NA         20       9       0       9 Con7 
##  8 #3/83/3-3             NA          NA         20       9       1       8 Con8 
##  9 #2/95/3               NA          NA         20       8       0       8 Con8 
## 10 #3/5/2/2/95           28.5        NA         20       8       0       8 Con8 
## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive
```

## Filter

``` r
filter(pups_data, sex == 1)
## # A tibble: 155 × 6
##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>         <int>   <int>   <int>    <int>   <int>
##  1 #85               1       4      13        7      11
##  2 #85               1       4      13        7      12
##  3 #1/2/95/2         1       5      13        7       9
##  4 #1/2/95/2         1       5      13        8      10
##  5 #5/5/3/83/3-3     1       5      13        8      10
##  6 #5/5/3/83/3-3     1       5      14        6       9
##  7 #5/4/2/95/2       1      NA      14        5       9
##  8 #4/2/95/3-3       1       4      13        6       8
##  9 #4/2/95/3-3       1       4      13        7       9
## 10 #2/2/95/3-2       1       4      NA        8      10
## # … with 145 more rows
filter(pups_data,  sex == 2, pd_walk < 11)
## # A tibble: 127 × 6
##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>         <int>   <int>   <int>    <int>   <int>
##  1 #1/2/95/2         2       4      13        7       9
##  2 #1/2/95/2         2       4      13        7      10
##  3 #1/2/95/2         2       5      13        8      10
##  4 #1/2/95/2         2       5      13        8      10
##  5 #1/2/95/2         2       5      13        6      10
##  6 #5/5/3/83/3-3     2       5      13        8      10
##  7 #5/5/3/83/3-3     2       5      14        7      10
##  8 #5/5/3/83/3-3     2       5      14        8      10
##  9 #5/4/2/95/2       2      NA      14        7      10
## 10 #5/4/2/95/2       2      NA      14        7      10
## # … with 117 more rows
filter(pups_data,  sex == 2, pd_walk < 11) %>% select(starts_with("pd"))
## # A tibble: 127 × 4
##    pd_ears pd_eyes pd_pivot pd_walk
##      <int>   <int>    <int>   <int>
##  1       4      13        7       9
##  2       4      13        7      10
##  3       5      13        8      10
##  4       5      13        8      10
##  5       5      13        6      10
##  6       5      13        8      10
##  7       5      14        7      10
##  8       5      14        8      10
##  9      NA      14        7      10
## 10      NA      14        7      10
## # … with 117 more rows
filter(pups_data,  sex != 1 & pd_ears > 4)
## # A tibble: 11 × 6
##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>         <int>   <int>   <int>    <int>   <int>
##  1 #1/2/95/2         2       5      13        8      10
##  2 #1/2/95/2         2       5      13        8      10
##  3 #1/2/95/2         2       5      13        6      10
##  4 #5/5/3/83/3-3     2       5      13        8      10
##  5 #5/5/3/83/3-3     2       5      14        7      10
##  6 #5/5/3/83/3-3     2       5      14        8      10
##  7 #1/82/3-2         2       5      13        6      10
##  8 #62               2       5      13       10      12
##  9 #5/93/2           2       5      14        7       9
## 10 #5/93/2           2       5      14        7       9
## 11 #5/93/2           2       5      13        7       9
filter(pups_data,  !(sex == 1 & pd_walk != 10))
## # A tibble: 190 × 6
##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>         <int>   <int>   <int>    <int>   <int>
##  1 #1/2/95/2         1       5      13        8      10
##  2 #5/5/3/83/3-3     1       5      13        8      10
##  3 #2/2/95/3-2       1       4      NA        8      10
##  4 #85               2       4      13        6      11
##  5 #1/2/95/2         2       4      13        7       9
##  6 #1/2/95/2         2       4      13        7      10
##  7 #1/2/95/2         2       5      13        8      10
##  8 #1/2/95/2         2       5      13        8      10
##  9 #1/2/95/2         2       5      13        6      10
## 10 #5/5/3/83/3-3     2       5      13        8      10
## # … with 180 more rows
filter(pups_data,is.na(pd_eyes))
## # A tibble: 13 × 6
##    litter_number     sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>           <int>   <int>   <int>    <int>   <int>
##  1 #2/2/95/3-2         1       4      NA        8      10
##  2 #1/5/3/83/3-3/2     1       4      NA       NA       9
##  3 #1/5/3/83/3-3/2     1       4      NA        7       9
##  4 #1/5/3/83/3-3/2     1       4      NA        7       9
##  5 #1/5/3/83/3-3/2     1       4      NA        7       9
##  6 #1/5/3/83/3-3/2     1       4      NA        7       9
##  7 #2/2/95/3-2         2       4      NA        7      10
##  8 #2/2/95/3-2         2       4      NA        8      10
##  9 #2/2/95/3-2         2       4      NA        8      11
## 10 #1/5/3/83/3-3/2     2       4      NA        7       9
## 11 #1/5/3/83/3-3/2     2       4      NA        7       9
## 12 #1/5/3/83/3-3/2     2       4      NA        7       9
## 13 #1/5/3/83/3-3/2     2       4      NA        7       9
filter(pups_data, pd_ears %in% c(2, 3, 4))
## # A tibble: 276 × 6
##    litter_number     sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>           <int>   <int>   <int>    <int>   <int>
##  1 #85                 1       4      13        7      11
##  2 #85                 1       4      13        7      12
##  3 #4/2/95/3-3         1       4      13        6       8
##  4 #4/2/95/3-3         1       4      13        7       9
##  5 #2/2/95/3-2         1       4      NA        8      10
##  6 #1/5/3/83/3-3/2     1       4      NA       NA       9
##  7 #1/5/3/83/3-3/2     1       4      NA        7       9
##  8 #1/5/3/83/3-3/2     1       4      NA        7       9
##  9 #1/5/3/83/3-3/2     1       4      NA        7       9
## 10 #1/5/3/83/3-3/2     1       4      NA        7       9
## # … with 266 more rows
drop_na(pups_data, pd_ears, pd_eyes)
## # A tibble: 282 × 6
##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>         <int>   <int>   <int>    <int>   <int>
##  1 #85               1       4      13        7      11
##  2 #85               1       4      13        7      12
##  3 #1/2/95/2         1       5      13        7       9
##  4 #1/2/95/2         1       5      13        8      10
##  5 #5/5/3/83/3-3     1       5      13        8      10
##  6 #5/5/3/83/3-3     1       5      14        6       9
##  7 #4/2/95/3-3       1       4      13        6       8
##  8 #4/2/95/3-3       1       4      13        7       9
##  9 #85               2       4      13        6      11
## 10 #1/2/95/2         2       4      13        7       9
## # … with 272 more rows
drop_na(pups_data, pd_ears, pd_pivot) %>% filter(is.na(pd_eyes))
## # A tibble: 12 × 6
##    litter_number     sex pd_ears pd_eyes pd_pivot pd_walk
##    <chr>           <int>   <int>   <int>    <int>   <int>
##  1 #2/2/95/3-2         1       4      NA        8      10
##  2 #1/5/3/83/3-3/2     1       4      NA        7       9
##  3 #1/5/3/83/3-3/2     1       4      NA        7       9
##  4 #1/5/3/83/3-3/2     1       4      NA        7       9
##  5 #1/5/3/83/3-3/2     1       4      NA        7       9
##  6 #2/2/95/3-2         2       4      NA        7      10
##  7 #2/2/95/3-2         2       4      NA        8      10
##  8 #2/2/95/3-2         2       4      NA        8      11
##  9 #1/5/3/83/3-3/2     2       4      NA        7       9
## 10 #1/5/3/83/3-3/2     2       4      NA        7       9
## 11 #1/5/3/83/3-3/2     2       4      NA        7       9
## 12 #1/5/3/83/3-3/2     2       4      NA        7       9
```

## Mutate: add or remove cols

``` r
mutate(litters_data,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
## # A tibble: 49 × 9
##    group litter_number   gd0_w…¹ gd18_…² gd_of…³ pups_…⁴ pups_…⁵ pups_…⁶ wt_gain
##    <chr> <chr>             <dbl>   <dbl>   <int>   <int>   <int>   <int>   <dbl>
##  1 con7  #85                19.7    34.7      20       3       4       3    15  
##  2 con7  #1/2/95/2          27      42        19       8       0       7    15  
##  3 con7  #5/5/3/83/3-3      26      41.4      19       6       0       5    15.4
##  4 con7  #5/4/2/95/2        28.5    44.1      19       5       1       4    15.6
##  5 con7  #4/2/95/3-3        NA      NA        20       6       0       6    NA  
##  6 con7  #2/2/95/3-2        NA      NA        20       6       0       4    NA  
##  7 con7  #1/5/3/83/3-3/2    NA      NA        20       9       0       9    NA  
##  8 con8  #3/83/3-3          NA      NA        20       9       1       8    NA  
##  9 con8  #2/95/3            NA      NA        20       8       0       8    NA  
## 10 con8  #3/5/2/2/95        28.5    NA        20       8       0       8    NA  
## # … with 39 more rows, and abbreviated variable names ¹​gd0_weight,
## #   ²​gd18_weight, ³​gd_of_birth, ⁴​pups_born_alive, ⁵​pups_dead_birth,
## #   ⁶​pups_survive
```

## Pips: %\>%
