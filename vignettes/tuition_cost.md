Tuition Cost
================
[Jim Tyhurst, Ph.D.](https://www.jimtyhurst.com/)
2020-04-06

  - [TODO](#todo)
  - [Install the needed packages](#install-the-needed-packages)
  - [Look at the available datasets](#look-at-the-available-datasets)
  - [Load the dataset](#load-the-dataset)
  - [Questions to explore with this
    data](#questions-to-explore-with-this-data)
  - [Findings](#findings)
  - [Lessons learned](#lessons-learned)
  - [References](#references)

## TODO

  - Read `salary_potential` data.
  - Combine `salary_potential` and `tuition_cost` into a single `tbl`.
  - Can the `salary_potential` variables `mid_career_pay` and
    `make_world_better_percent` be predicted from `tuition_cost`
    variables, such as `type`, `in_state_total`, and
    `out_of_state_total`?
  - Is `salary_potential$stem_percent` correlated with public vs private
    `type` of school?

## Install the needed packages

``` r
#install.packages("remotes")
#remotes::install_github("thebioengineer/tidytuesdayR")

# Optional package for browsing:
#remotes::install_github("laderast/burro")
```

## Look at the available datasets

``` r
library(tidytuesdayR)
# This will open up in the help window:
#tidytuesdayR::tt_available()
```

## Load the dataset

Load your dataset in with the function below. The input is the date the
dataset was issued. You should be able to get this from the
`tt_available()` function.

``` r
# Warning: This takes a long time ...
# Incoming data comes in as a list
datasets <- tidytuesdayR::tt_load('2020-03-10')
```

    ## --- Downloading #TidyTuesday Information for 2020-03-10 ----

    ## --- Identified 11 files available for download ----

    ## --- Downloading files ---

    ## Warning: 1021 parsing failures.
    ## row col  expected     actual                                                                                file
    ##   2  -- 1 columns 7 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc30dfe583.htm'
    ##   3  -- 1 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc30dfe583.htm'
    ##   4  -- 1 columns 4 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc30dfe583.htm'
    ##   5  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc30dfe583.htm'
    ##   6  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc30dfe583.htm'
    ## ... ... ......... .......... ...................................................................................
    ## See problems(...) for more details.

    ## Warning: 1021 parsing failures.
    ## row col  expected     actual                                                                                file
    ##   2  -- 1 columns 7 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc3b551486.htm'
    ##   3  -- 1 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc3b551486.htm'
    ##   4  -- 1 columns 4 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc3b551486.htm'
    ##   5  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc3b551486.htm'
    ##   6  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc3b551486.htm'
    ## ... ... ......... .......... ...................................................................................
    ## See problems(...) for more details.

    ## Warning: 1012 parsing failures.
    ## row col  expected     actual                                                                                file
    ##   2  -- 1 columns 7 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4eb687d8.htm'
    ##   3  -- 1 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4eb687d8.htm'
    ##   4  -- 1 columns 4 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4eb687d8.htm'
    ##   5  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4eb687d8.htm'
    ##   6  -- 1 columns 50 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4eb687d8.htm'
    ## ... ... ......... .......... ...................................................................................
    ## See problems(...) for more details.

    ## Warning: Duplicated column names deduplicated: 'BLANK' =>
    ## 'BLANK_1' [257], 'BLANK' => 'BLANK_2' [258], 'OUTLIER CHECK' =>
    ## 'OUTLIER CHECK_1' [298], 'RATE 0708—0809' => 'RATE 0708—0809_1' [299],
    ## 'RATE 0809—0910' => 'RATE 0809—0910_1' [300], 'RATE 0910—1011'
    ## => 'RATE 0910—1011_1' [301], 'RATE 1011—1112' => 'RATE 1011—
    ## 1112_1' [302], 'RATE 1112—1213' => 'RATE 1112—1213_1' [303], 'RATE
    ## 1213—1314' => 'RATE 1213—1314_1' [304], 'RATE 1314—1415' => 'RATE 1314
    ## —1415_1' [305], 'RATE 1415—1516' => 'RATE 1415—1516_1' [306], 'RATE
    ## 1516—1617' => 'RATE 1516—1617_1' [307], 'RATE 1617—1718' => 'RATE 1617
    ## —1718_1' [308], 'RATE 1718—1819' => 'RATE 1718—1819_1' [309], 'AAGR'
    ## => 'AAGR_1' [310], 'Retention full-time total' => 'Retention full-
    ## time total_1' [384], 'BLANK' => 'BLANK_3' [385], 'Retention part-time
    ## total' => 'Retention part-time total_1' [386]

    ## Warning: 1 parsing failure.
    ##  row   col           expected      actual                                                                                file
    ## 5980 BLANK 1/0/T/F/TRUE/FALSE 0.250963763 '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc31909e47.csv'

    ## Warning in identify_delim(temp_file): Not able to detect delimiter for
    ## the file. Defaulting to ` `.

    ## Warning: 213419 parsing failures.
    ## row col  expected     actual                                                                                  file
    ##   1  -- 4 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4e94358f.mhtml'
    ##   2  -- 4 columns 14 columns '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4e94358f.mhtml'
    ##   3  -- 4 columns 7 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4e94358f.mhtml'
    ##   4  -- 4 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4e94358f.mhtml'
    ##   5  -- 4 columns 2 columns  '/var/folders/cf/h8l4xxzs0bvd7s6nz8s_xgy40000gn/T//RtmpG0o2UJ/file59dc4e94358f.mhtml'
    ## ... ... ......... .......... .....................................................................................
    ## See problems(...) for more details.

    ## Warning in identify_delim(temp_file): Detected multiple possible
    ## delimeters:`,`, ` `. Defaulting to `,`.

    ## --- Download complete ---

``` r
# Show the names of the individual datasets
names(datasets)
```

    ##  [1] "Student Diversity at More Than 4,600 Institutions - The Chronicle of Higher Education Percent"
    ##  [2] "Student Diversity at More Than 4,600 Institutions - The Chronicle of Higher Education"        
    ##  [3] "Tuition and Fees, 1998-99 Through 2018-19 - The Chronicle of Higher Education"                
    ##  [4] "all-schools"                                                                                  
    ##  [5] "diversity_school"                                                                             
    ##  [6] "historical_tuition"                                                                           
    ##  [7] "salary_potential"                                                                             
    ##  [8] "student_diversity"                                                                            
    ##  [9] "student_diversity"                                                                            
    ## [10] "tuition_cost"                                                                                 
    ## [11] "tuition_income"

``` r
df <- datasets$tuition_cost
```

To explore the data quickly, use
[burro](https://github.com/laderast/burro), which opens a web app for
browsing the data:

``` r
library(burro)
library(ggplot2)
#burro::explore_data(df,outcome_var = NULL)
```

``` r
dim(df)
```

    ## [1] 2973   10

``` r
colnames(df)
```

    ##  [1] "name"                 "state"               
    ##  [3] "state_code"           "type"                
    ##  [5] "degree_length"        "room_and_board"      
    ##  [7] "in_state_tuition"     "in_state_total"      
    ##  [9] "out_of_state_tuition" "out_of_state_total"

``` r
head(df$name)
```

    ## [1] "Aaniiih Nakoda College"              
    ## [2] "Abilene Christian University"        
    ## [3] "Abraham Baldwin Agricultural College"
    ## [4] "Academy College"                     
    ## [5] "Academy of Art University"           
    ## [6] "Adams State University"

``` r
library(dplyr)
df %>% 
  dplyr::select(state, state_code, type) %>% 
  head()
```

    ## # A tibble: 6 x 3
    ##   state      state_code type      
    ##   <chr>      <chr>      <chr>     
    ## 1 Montana    MT         Public    
    ## 2 Texas      TX         Private   
    ## 3 Georgia    GA         Public    
    ## 4 Minnesota  MN         For Profit
    ## 5 California CA         For Profit
    ## 6 Colorado   CO         Public

``` r
df %>% 
  dplyr::select(
    in_state_tuition, 
    in_state_total, 
    out_of_state_tuition, 
    out_of_state_total
  ) %>% 
  head()
```

    ## # A tibble: 6 x 4
    ##   in_state_tuition in_state_total out_of_state_tuit… out_of_state_tot…
    ##              <dbl>          <dbl>              <dbl>             <dbl>
    ## 1             2380           2380               2380              2380
    ## 2            34850          45200              34850             45200
    ## 3             4128          12602              12550             21024
    ## 4            17661          17661              17661             17661
    ## 5            27810          44458              27810             44458
    ## 6             9440          18222              20456             29238

## Questions to explore with this data

Given your inital exploration of the data, what was the question you
wanted to answer?

## Findings

Put your findings and your visualization code here.

## Lessons learned

Were there any lessons you learned? Any cool packages you want to talk
about?

## References

[Source code](./tuition_cost.Rmd).

The overall flow of this R markdown file is based on the
[template.Rmd](https://github.com/pdxrlang/tidy_tuesday_template/blob/master/template.Rmd)
file in the
[pdxrlang/tidy\_tuesday\_template](https://github.com/pdxrlang/tidy_tuesday_template/)
project.

Dataset: [College tuition, diversity, and
pay](https://github.com/rfordatascience/tidytuesday/tree/master/data/2020/2020-03-10).
