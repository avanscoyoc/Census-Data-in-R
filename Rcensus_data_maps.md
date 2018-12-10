---
title: "Census Data Wrangling and Mapping in R"
author: "Patty Frontiera"
date: "12/4/2018"
output: 
  html_document:
    keep_md: true
---




## Setup

Welcome! While we're waiting:

* **Clone or download** the workshop files from: [https://github.com/dlab-geo/](https://github.com/dlab-geo/ )
    - If you downloaded the zipfile, **unzip it**.
    - Make a note of the folder in which the files reside. 
    
    
* Open **RStudio**

* Open a new **R script** file

## Introduction

- About me

- About you
    - Your familiarity with US Census data
    - with geospatial data
    - with geospatial data in R

## Outline

- Describe primary Census data products

- Introduce R packages for working with Census Data

- Use those packages to fetch census data

- Use those packages to fetch census data plus census geograpic boundary files

- Make maps of census data

## US Census Data

The "nation's leading provider of quality data about its people and economy."

<img src="data/census_page.png" width="700px"></img>

Available at [www.census.gov](www.census.gov)

## Primary Census Products

- Decennial Census

- American Community Survey (ACS)

##  Decennial Census

Complete count of the population every 10 years since `1790` 

Includes data on 

- population, by age & race/ethnicity 

- housing, by occupancy & tenure (owned, rented)

## American Community Survey (ACS)

- Annual survey of a sample of about 3 million household

- Provides estimates of demographic, social, economic & housing characteristics

- Includes margin of error values for the estimates.

 
## Decennial Census* vs ACS Data
| Demographic*    | Social             | Economic          | Housing           |
|-----------------|--------------------|-------------------|-------------------|
| Sex             | Families           | Income            | Tenure*           |
| Age             | Education          | Benefits          | Occupancy*        |
| Race            | Marital Status     | Employment Status | Structure Type    |
| Hispanic Origin | Fertility          | Occupation        | Housing Value     |
|                 | Grandparents       | Industry          | Taxes & Insurance |
|                 | Veterans           | Commuting         | Utilities         |
|                 | Disability Status  | Place of Work     | Mortgage          |
|                 | Language at Home   | Health Insurance  | Monthly Rent      |
|                 | Citizenship        |                   |                   |
|                 | Mobility           |                   |                   |



## Census Geographies

Census data are publicly available at one or more levels of geographic aggregation.
<img src="data/census_geo_hierarchy.png" height="500px"></img>

## Census Data & Census Geographies

<img src="data/census_data_by_prod_geo.png" width="800px"></img>

## ACS 5 Year Dataset RECOMMENDED

ACS 1 year and 5 year products are currently available 

- ACS 3 year no longer available

ACS 5 year data provdes much better estimates, lower margins of error

More data available for ACS 5 Year product


## Census Data Workflow

Identify your 
- topic of interest
- year(s)
- geographic level of detail
- for what locations?

Then determine what specific tables and variables
are available - ACS or Decennial?

## CAUTION

"If you want to measure change you can't change the measures!"

**Census tables, variables, geographies, and geographic boundaries change over time!**

Measuring change over time with census data is *it's own thing*, complex and not covered by this workshop!


## Packages for Working with Census Data

These are the ones we recommend and will use today.

- [tidycensus](https://walkerke.github.io/tidycensus) & [tigris](https://github.com/walkerke/tigris)

- [tidyverse](https://www.tidyverse.org/)
   
- [sf](https://r-spatial.github.io/sf/)


# tidycensus & tigris

## [tidycensus](https://walkerke.github.io/tidycensus)

Functions for accessing census decennial and ACS 5 year datasets via Census APIs

- only a subset of datasets / years available
- requires a `Census API key` 

## [tidycensus](https://walkerke.github.io/tidycensus)

Limited set of years available via `tidycensus`

- decennial census: 1990, 2000, and 2010
- ACS 5 yr: 2005-2010 through 2012-2016 are available. 
- Note: tidycensus referes to ACS 5year datasets by the endyear.

Most recent ACS available is 2012-2016

- 2013 - 2017 released [Dec 6, 2018](https://www.census.gov/programs-surveys/acs/news/data-releases/2017/release-schedule.html) by the Census. Need to check when available in `tidycensus` & update package.

##  [tigris](https://github.com/walkerke/tigris)

Provides access to census geographic data files

    - detailed TIGER/Line boundary files (e.g., shapefiles), or
    - simplified Cartographic boundary files
    
Also provides access to census `feature data`,
  - eg, rivers, roads, coastlands, landmarks, and more
    

Used by `tidycensus` to access state, county, tract, block group, block, and ZCTA boundaries.

- Use `tigris` directly to access other census geographic data.
    
## tidycensus & tigris

Packages developed by [Kyle Walker](https://walkerke.github.io/) to make it easier to fetch data from Census websites and APIs in **R** and get that data in a useable format to analyze, plot, and map.

Check out his website to keep abreast of his great packages, blog posts and tutorials.

- http://personal.tcu.edu/kylewalker/

- https://walkerke.github.io/

Walker also develped a new [DataCamp](https://www.datacamp.com) course [Analyzing US Census Data in R!](https://www.datacamp.com/courses/analyzing-us-census-data-in-r)

- Highly recommended! First chapter free!

## Alternatives

You can write code to access the [Census APIs](https://www.census.gov/data/developers/data-sets.html) directly.

You can download Census data directly from:

- [American Factfinder](https://factfinder.census.gov/faces/nav/jsf/pages/index.xhtml) or 
- [NHGIS.org](https://www.nhgis.org/)
- [Social Explorer](https://www.socialexplorer.com/)
    - Subscription service but FREE for UCB community

You can download Census `geographic data` directly on the [census website](https://www.census.gov/geo/maps-data/)




## [tidyverse](https://www.tidyverse.org/)

A collection of R Packages for data science
- developed primarily by [Hadley Wickham](http://hadley.nz/), Chief Scientist at [RStudio](https://www.rstudio.com/).

 - `dplyr` and `tidyr` for reshaping data

 - `ggplot2` for plotting

 - `purr`, `readr` and `tibble` for improved performance 

These packages are used by `tidyverse` under the hood.
## [sf](https://r-spatial.github.io/sf/)

Simple features for geospatial data objects and methods.

- Next generation R package for working with vector geospatial data
    - will soon supercede the `sp` package
    
`sf` includes the functionality of the `sp`, `rgdal`, `rgeos` and `proj4` packages.

- but with improved performance, simplified command syntax and easier workflows.


# Tutorial Time!

## Part 1.

We will work through several exercises using `tidycensus` to fetch, wrangle and map census data.

## Loading packages

Load the packages we will use today


```r
library(tidycensus)
library(tidyverse) 
library(tigris)
library(sf)
library(dplyr)
```

## Install any packages that you do not have on your computer

Also install any dependancies.


```r
# install.packages("tidyverse")
# install.packages("tidycensus")
# install.packages("sf")
```


## Census API Key

You need a census API key to programmatically fetch census data.

* (https://api.census.gov/data/key_signup.html)

For more info see:

* https://www.census.gov/data/developers/data-sets.html


```r
# Install your census api key. 
# If you set install=TRUE you need only do this once.

#census_api_key("YOUR KEY GOES HERE", install = TRUE)

my_census_api_key <- "f2d6f4f743545d3a42a67412b05935dc7712c432"
census_api_key(my_census_api_key)
```

```
## To install your API key for use in future sessions, run this function with `install = TRUE`.
```

## Set working directory

to the folder in which you unzipped the workshop files, e.g.,

`setwd("~/Documents/Dlab/workshops/2018/rCensus_workshop")`

<img src="./data/swd.png" width="600px"></img>

# Fetching Data from the Decennial Census

## Population Data

Let's start by fetching population data for each state from the 2010 Census

## Identify Tables & Variables

First step is to identify the names of the census variables that contain the data of interest.

The tidycensus `load_variables` function can help with that.

First, take a look at the function documentation.

```r
?load_variables
```

## load_variables

Use `load_variables` to fetch all variables used in the 2010 census into a dataframe.

```r
vars2010 <- load_variables(year=2010,        # Year or end year for ACS
                           dataset = 'sf1',  # 'sf1' for decennial or 'acs5'
                           cache = TRUE)     # Whether to save fetched data locally
```

## Decennial Census Variables

Let's take a look at and discuss the resultant dataframe.

- How many 2010 census variables are in the table?

```r
View(vars2010)
```

## 2010 Decennial Census Tables

- Topics: Population, housing

- Tables: currenty `333`
    - 177 population tables (identified with a ‘‘P’’) available to the block level 
    - 58 housing tables (identified with an ‘‘H’’) available to the block level
    - 82 population tables (identified with a ‘‘PCT’’) available to the census tract level
    - 4 housing tables (identified with an “HCT”) available to the census tract level
    - 10 population tables (identified with a “PCO”) available to the county level 
    - plus 2 additoinal PCT tables
    
## get_decennial

`get_decenial` is the tidycensus function for fetching data from the decennial census API.

First, check the documentation for the function.

```r
?get_decennial
```

## get_decennial

Let's fetch `total population by state` (**P001001**) from the 2010 census using `get_decennial`.


```r
pop2010 <- get_decennial(geography = "state",   # census tabulation unit
                         variables = "P001001", # variable(s) of interest
                         year = 2010)           # census year
```

```
## Getting data from the 2010 decennial Census
```

## View the Data

- How many rows and columns? 

- What column has the pop counts?

```r
pop2010
```

```
## # A tibble: 52 x 4
##    GEOID NAME                 variable     value
##    <chr> <chr>                <chr>        <dbl>
##  1 01    Alabama              P001001   4779736.
##  2 02    Alaska               P001001    710231.
##  3 04    Arizona              P001001   6392017.
##  4 05    Arkansas             P001001   2915918.
##  5 06    California           P001001  37253956.
##  6 08    Colorado             P001001   5029196.
##  7 09    Connecticut          P001001   3574097.
##  8 10    Delaware             P001001    897934.
##  9 11    District of Columbia P001001    601723.
## 10 12    Florida              P001001  18801310.
## # ... with 42 more rows
```

## Visualize results

`ggplot2` is the most commonly used R package for data visualization. 

It is loaded when you load the `tidyverse` package.

- Knowlege of `ggplot` very useful for both exploratory analysis and communicating results.


```r
pop_plot<- ggplot(data=pop2010, aes(x=reorder(NAME,value), y=value/1000000)) + 
  geom_bar(stat="identity") + coord_flip() +
  theme_minimal() + 
  labs(title = "2010 US Population by State") +
  xlab("State") +
  ylab("in millions")
```

## Display the plot

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

## Challenge

Fetch population data by state for 2000.

*Don't assume variable names are the same across years.* Check first!

## Total Population in 2000


```r
# What is the variable name in 2000?
vars2000 <- load_variables(year=2000, dataset = 'sf1', cache = T)

# Take a look and search in the dataframe
View(vars2000)

# Fetch the 2000 pop data
pop2000 <- get_decennial(geography = "state", variables = "P001001", year = 2000)

# Take a look
pop2000
```

## Limiting by Area of Interest

In the previous example we retrieved population data for all states.

- This is the default behavior if you don't specify a subset.

- You can limit the retrieved data by state or by county, if applicable.

## Limit Areas of Interest

Let's fetch data for just 3 states.


```r
state_pop2010 <- get_decennial(geography = "state", # census tabulation unit
                         variables = "P001001",     # variables of interest
                         year = 2010,               # census year
                         state=c("CA","OR","WA"))   # Filter by states of interest
```

```
## Getting data from the 2010 decennial Census
```

## Review Results

```r
state_pop2010
```

```
## # A tibble: 3 x 4
##   GEOID NAME       variable     value
##   <chr> <chr>      <chr>        <dbl>
## 1 06    California P001001  37253956.
## 2 41    Oregon     P001001   3831074.
## 3 53    Washington P001001   6724540.
```

## Changing Census Tabulation unit

`get_decennial` accepts a number of different values for tabulation unit.

- Options include: `state`, `county`, `tract`, `block group`, `block`, and `ZCTA`.

Let's change the tabulation unit to `county`.

```r
co_pop2010 <- get_decennial(geography = "county",   # census tabulation unit
                            variables = "P001001",  # variables of interest
                            year = 2010)
```

```
## Getting data from the 2010 decennial Census
```

## Changing Census Tabulation unit

View the Data to see what was retrieved.

```r
str(co_pop2010)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	3221 obs. of  4 variables:
##  $ GEOID   : chr  "01001" "01007" "01017" "01019" ...
##  $ NAME    : chr  "Autauga County, Alabama" "Bibb County, Alabama" "Chambers County, Alabama" "Cherokee County, Alabama" ...
##  $ variable: chr  "P001001" "P001001" "P001001" "P001001" ...
##  $ value   : num  54571 22915 34215 25989 25833 ...
```

```r
head(co_pop2010, 2)
```

```
## # A tibble: 2 x 4
##   GEOID NAME                    variable  value
##   <chr> <chr>                   <chr>     <dbl>
## 1 01001 Autauga County, Alabama P001001  54571.
## 2 01007 Bibb County, Alabama    P001001  22915.
```

```r
tail(co_pop2010, 2)
```

```
## # A tibble: 2 x 4
##   GEOID NAME                           variable  value
##   <chr> <chr>                          <chr>     <dbl>
## 1 72151 Yabucoa Municipio, Puerto Rico P001001  37941.
## 2 72153 Yauco Municipio, Puerto Rico   P001001  42043.
```

## Challenge

1. Fetch population by **county** for just California

2. Fetch population by **county** for Oregon & California


## Challenge

1. Fetch population by **tract** for all states.

2. Fetch population by **tract** for California.

## Fetching Census Tract Data

If you want census data at the tract level or below you need to specifiy the state & county or counties.

```r
tract_pop2010 <- get_decennial(geography = "tract",   # census tabulation unit
                         variables = "P001001",       # variable of interest
                         year = 2010,                 # census year
                         state="CA",                  # limit to state of California
                         county=c("Alameda","Contra Costa"))  # and only these counties
```

```
## Getting data from the 2010 decennial Census
## Getting data from the 2010 decennial Census
## Getting data from the 2010 decennial Census
```

## Fetching Census Tract Data

View the results! How many census tracts are in these 3 counties?


```r
str(tract_pop2010)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	569 obs. of  4 variables:
##  $ GEOID   : chr  "06001400100" "06001400200" "06001400300" "06001400400" ...
##  $ NAME    : chr  "Census Tract 4001, Alameda County, California" "Census Tract 4002, Alameda County, California" "Census Tract 4003, Alameda County, California" "Census Tract 4004, Alameda County, California" ...
##  $ variable: chr  "P001001" "P001001" "P001001" "P001001" ...
##  $ value   : num  2937 1974 4865 3703 3517 ...
```

```r
head(tract_pop2010, 2)
```

```
## # A tibble: 2 x 4
##   GEOID       NAME                                          variable value
##   <chr>       <chr>                                         <chr>    <dbl>
## 1 06001400100 Census Tract 4001, Alameda County, California P001001  2937.
## 2 06001400200 Census Tract 4002, Alameda County, California P001001  1974.
```

```r
tail(tract_pop2010, 2)
```

```
## # A tibble: 2 x 4
##   GEOID       NAME                                          variable value
##   <chr>       <chr>                                         <chr>    <dbl>
## 1 06013392300 Census Tract 3923, Contra Costa County, Cali… P001001  3102.
## 2 06013990000 Census Tract 9900, Contra Costa County, Cali… P001001     0.
```

## Challenge

1. Fetch population by **county** for Alameda County, California

2. Fetch population by **tract** for the nine county Bay Area:
- Alameda, SF, Contra Costa, Marin County, Napa, 
- San Mateo, Santa Clara,  Solano,  Sonoma, Santa Cruz

## Challenge hint
You can use names or FIPs codes as your `state` and `county` function arguments.

```r
# County FIPs Codes for
# Alameda, SF, Contra Costa, Marin County, Napa, 
# San Mateo, Santa Clara,  Solano,  Sonoma, santa cruz
nine_counties <- c("001", "075", "013", "041", "055", "081", "085", "095", "097", "087")

# Your code below...
bayarea_pop10 <-...
```

## Fetching data for more than one census variable

What `three` things are new here?

```r
ur_pop10 <- get_decennial(geography = "county",  # census tabulation unit
                           variables = c(urban="P002002",rural="P002005"),
                           year = 2010, 
                           summary_var = "P002001",  # Total Urban - the denominator
                           state='CA',
                           county=c("Napa","Sonoma","Mendocino"))
```

```
## Getting data from the 2010 decennial Census
```

## Take a look at the results

```r
ur_pop10
```

```
## # A tibble: 6 x 5
##   GEOID NAME                         variable   value summary_value
##   <chr> <chr>                        <chr>      <dbl>         <dbl>
## 1 06045 Mendocino County, California urban     48110.        87841.
## 2 06055 Napa County, California      urban    118194.       136484.
## 3 06097 Sonoma County, California    urban    424102.       483878.
## 4 06045 Mendocino County, California rural     39731.        87841.
## 5 06055 Napa County, California      rural     18290.       136484.
## 6 06097 Sonoma County, California    rural     59776.       483878.
```

## Calculating Percents

The `summary_value` column comes in handy when you want to compute percent of total.

Here's one way to do it.

```r
ur_pop10 <- ur_pop10 %>%
            mutate(pct = 100 * (value / summary_value))
```

```
## Warning: package 'bindrcpp' was built under R version 3.4.4
```

## Calculating Percents

Let's take a look at the output

```r
ur_pop10 # Take a look
```

```
## # A tibble: 6 x 6
##   GEOID NAME                         variable   value summary_value   pct
##   <chr> <chr>                        <chr>      <dbl>         <dbl> <dbl>
## 1 06045 Mendocino County, California urban     48110.        87841.  54.8
## 2 06055 Napa County, California      urban    118194.       136484.  86.6
## 3 06097 Sonoma County, California    urban    424102.       483878.  87.6
## 4 06045 Mendocino County, California rural     39731.        87841.  45.2
## 5 06055 Napa County, California      rural     18290.       136484.  13.4
## 6 06097 Sonoma County, California    rural     59776.       483878.  12.4
```

## Plot it

Plots give us compact visual summaries of the data

```r
myplot <- ggplot(data = ur_pop10, 
          mapping = aes(x = NAME, fill = variable, 
                     y = ifelse(test = variable == "urban", 
                                yes = -pct, no = pct))) +
          geom_bar(stat = "identity") +
          scale_y_continuous(labels = abs, limits=c(-100,100)) +
          labs(title="Urban & Rural Population in Wine Country", 
               x="County", y = " Percent of Population", fill="") +
          coord_flip()
```

## Plot it

```r
myplot
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

## Fetch all the data in one table

This is often helpful but you need to keep tract of the meaning of each variable.

```r
alco_pop10 <- get_decennial(geography = "tract", # Census tabulation unit
                           table =  "P002",      # Table of urban & rural population counts
                           year = 2010,          # Decennial census year
                           state='CA',           # Filter state
                           county="Alameda")     # Filter county
```

```
## Getting data from the 2010 decennial Census
```

## Take a look

```r
unique(alco_pop10$variable)
```

```
## [1] "P002001" "P002002" "P002003" "P002004" "P002005" "P002006"
```

```r
alco_pop10
```

```
## # A tibble: 2,166 x 4
##    GEOID       NAME                                         variable value
##    <chr>       <chr>                                        <chr>    <dbl>
##  1 06001400100 Census Tract 4001, Alameda County, Californ… P002001  2937.
##  2 06001400200 Census Tract 4002, Alameda County, Californ… P002001  1974.
##  3 06001400300 Census Tract 4003, Alameda County, Californ… P002001  4865.
##  4 06001400400 Census Tract 4004, Alameda County, Californ… P002001  3703.
##  5 06001400500 Census Tract 4005, Alameda County, Californ… P002001  3517.
##  6 06001400600 Census Tract 4006, Alameda County, Californ… P002001  1571.
##  7 06001400700 Census Tract 4007, Alameda County, Californ… P002001  4206.
##  8 06001400800 Census Tract 4008, Alameda County, Californ… P002001  3594.
##  9 06001400900 Census Tract 4009, Alameda County, Californ… P002001  2302.
## 10 06001401000 Census Tract 4010, Alameda County, Californ… P002001  5678.
## # ... with 2,156 more rows
```

## Fetching Data for multiple years

You cannot do this with one `tidycensus` function call unless you wrap it in a function.

- See **Extra** section at the end of tutorial.


```r
pop2000 <- get_decennial(geography = "state", variables = "P001001", year = 2000)
pop2010 <- get_decennial(geography = "state", variables = "P001001", year = 2010)
```

## Output options

Let's try all three of these commands and then look at the ouput to see what's different?

```r
pop2010a <- get_decennial(geography = "state", variables = "P001001", year = 2010)
pop2010b <- get_decennial(geography = "state", variables = c(pop10="P001001"), year = 2010)
pop2010c <- get_decennial(geography = "state", variables = c(pop00="P001001"), year = 2010, output="wide")
```


## Data Wrangling

Your R skills can help you reformat the data and make it more useable.

First, fetch population data for 2010 & 2000 by state with `output=wide`.

Label the variables `pop00` and `pop10`.

## Data Wrangling

```r
pop2000 <- get_decennial(geography = "state", variables = c(pop00="P001001"), year = 2000, output="wide")
```

```
## Getting data from the 2000 decennial Census
```

```r
pop2010 <- get_decennial(geography = "state", variables = c(pop10="P001001"), year = 2010, output="wide")
```

```
## Getting data from the 2010 decennial Census
```

## Merge population by state from both censuses


```r
pop2000_2010 <- pop2000 %>% merge(pop2010, by="NAME") %>%
                             select(NAME, pop00, pop10)
```

## Data Wrangling

View results

```r
pop2000_2010
```

```
##                    NAME    pop00    pop10
## 1               Alabama  4447100  4779736
## 2                Alaska   626932   710231
## 3               Arizona  5130632  6392017
## 4              Arkansas  2673400  2915918
## 5            California 33871648 37253956
## 6              Colorado  4301261  5029196
## 7           Connecticut  3405565  3574097
## 8              Delaware   783600   897934
## 9  District of Columbia   572059   601723
## 10              Florida 15982378 18801310
## 11              Georgia  8186453  9687653
## 12               Hawaii  1211537  1360301
## 13                Idaho  1293953  1567582
## 14             Illinois 12419293 12830632
## 15              Indiana  6080485  6483802
## 16                 Iowa  2926324  3046355
## 17               Kansas  2688418  2853118
## 18             Kentucky  4041769  4339367
## 19            Louisiana  4468976  4533372
## 20                Maine  1274923  1328361
## 21             Maryland  5296486  5773552
## 22        Massachusetts  6349097  6547629
## 23             Michigan  9938444  9883640
## 24            Minnesota  4919479  5303925
## 25          Mississippi  2844658  2967297
## 26             Missouri  5595211  5988927
## 27              Montana   902195   989415
## 28             Nebraska  1711263  1826341
## 29               Nevada  1998257  2700551
## 30        New Hampshire  1235786  1316470
## 31           New Jersey  8414350  8791894
## 32           New Mexico  1819046  2059179
## 33             New York 18976457 19378102
## 34       North Carolina  8049313  9535483
## 35         North Dakota   642200   672591
## 36                 Ohio 11353140 11536504
## 37             Oklahoma  3450654  3751351
## 38               Oregon  3421399  3831074
## 39         Pennsylvania 12281054 12702379
## 40         Rhode Island  1048319  1052567
## 41       South Carolina  4012012  4625364
## 42         South Dakota   754844   814180
## 43            Tennessee  5689283  6346105
## 44                Texas 20851820 25145561
## 45                 Utah  2233169  2763885
## 46              Vermont   608827   625741
## 47             Virginia  7078515  8001024
## 48           Washington  5894121  6724540
## 49        West Virginia  1808344  1852994
## 50            Wisconsin  5363675  5686986
## 51              Wyoming   493782   563626
```

## Quick Combo Plot

To check my data values

```r
ggplot() +
  geom_bar(data=pop2000_2010, aes(x=reorder(NAME,pop10), y=pop10/1000000), stat="identity") + coord_flip() +
  geom_point(data=pop2000_2010, aes(x=reorder(NAME,pop10), y=pop00/1000000, col="red")) + coord_flip()
```

```
## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-33-1.png)<!-- -->

## Save the data

Use `write.csv` to save a data frame to a `CSV` file.


```r
write.csv(pop2000_2010, file="pop2000_2010.csv", row.names = FALSE)
```

# QUESTIONS?


# Part 2. Mapping

## map topics

- how to fetch
- cb vs tl
- ggplot vs tmap vs plot
- components of an sf
- saving to shapefile
- crs issues esp. with census data
- shift_geo option
- other available data

## Mapping Census Data with `tidycensus`

We can fetch geographic data with tidycensus by adding `geometry=TRUE` to our function calls.

Under the hood, tidycensus calls the `tigris` package to fetch data from the Census Geographic Data APIs.

Only a subset of data available via `tigris` can be accessed via `tidycensus`.

## Geometry Options

Before fetching geometry, we need to specify a few `tigris` options

- Set the `class` of returned data to be `sf` objects (not `sp`, the default)
- Set `tigris_use_cache` to TRUE
- Set `cache` directory (optional)


```r
# Tigris options - used by tidycensus
options(tigris_class = "sf")      # SP is the default format returned by tigris
options(tigris_use_cache = TRUE)  # Save retrieved data locally
#tigris_cache_dir("~/Documents/gis_data/census")  # Folder for local data
```

## Fetch geographic boundary data with tidycensus

We fetch the geospatial data by setting `geometry=TRUE`.


```r
pop2010geo <- get_decennial(geography = "state", 
                          variables = c(pop10="P001001"), 
                          year = 2010, 
                          output="wide", 
                          geometry=TRUE) # Fetch geometry with the data for mapping
```

```
## Getting data from the 2010 decennial Census
```

## Take a look


```r
pop2010geo
```

```
## Simple feature collection with 52 features and 3 fields
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -179.1473 ymin: 17.88481 xmax: 179.7785 ymax: 71.35256
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## # A tibble: 52 x 4
##    GEOID NAME                     pop10                           geometry
##    <chr> <chr>                    <dbl>                 <MULTIPOLYGON [°]>
##  1 01    Alabama               4779736. (((-85.00237 31.00068, -85.02411 …
##  2 02    Alaska                 710231. (((-164.9762 54.13459, -164.9378 …
##  3 04    Arizona               6392017. (((-109.0452 36.99908, -109.0452 …
##  4 05    Arkansas              2915918. (((-94.55929 36.4995, -94.51948 3…
##  5 06    California           37253956. (((-122.4463 37.86105, -122.4386 …
##  6 08    Colorado              5029196. (((-102.0422 36.99308, -102.0545 …
##  7 09    Connecticut           3574097. (((-71.85957 41.3224, -71.86823 4…
##  8 10    Delaware               897934. (((-75.55945 39.62981, -75.5591 3…
##  9 11    District of Columbia   601723. (((-77.0386 38.79151, -77.0389 38…
## 10 12    Florida              18801310. (((-85.15641 29.67963, -85.1374 2…
## # ... with 42 more rows
```

## Take a look at the file on our local computer

```r
Sys.getenv('TIGRIS_CACHE_DIR') # check cache dir
```

```
## [1] "/Users/pattyf/Documents/gis_data/census"
```

## Geospatial Data in R

R `sf` objects include

- `geometry` of type (MULTI) POINT, LINE, POLYGON
    - in the dataframe column named `geometry`

-  a dataframe of `attributes`

- a `CRS` (coordinate reference system), specified by
    - epsg(SRID) code
    - proj4string

## Map it

We can use `plot` to make a quick map the geometry...


```r
plot(pop2010geo$geometry)
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-39-1.png)<!-- -->

## Question

What do you get if you plot the `sf` object without specifying "$geometry"


## The Challenge of US maps

Almost all of the USA is between -66 and -180 degrees latitude.

But Alaska includes a few islands that are between 180-172 degrees latitude.

This makes it difficult to map the US centered at 0,0 lat/lon.


```r
plot(pop2010geo$geometry)
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-40-1.png)<!-- -->

## Fetch geographic data with tidycensus, SHIFTED

tidycensus includes a `shift_geo` parameter to shift AK & HI to below Texas.

```r
pop2010geo_shifted <- get_decennial(geography = "state", 
                                    variables = c(pop10="P001001"), 
                                    output="wide",
                                    year = 2010, 
                                    geometry=TRUE, 
                                    shift_geo=TRUE)
```

```
## Getting data from the 2010 decennial Census
```

```
## Using feature geometry obtained from the albersusa package
```

```
## Please note: Alaska and Hawaii are being shifted and are not to scale.
```

## Shift Happens!

```r
plot(pop2010geo_shifted$geometry)
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-42-1.png)<!-- -->

## Save it

You can save `sf` data to a shapefile using `st_write`

```r
st_write(pop2010geo_shifted,"usa_2010_shifted.shp")
```

## Mapping Data Values


```r
plot(pop2010geo_shifted['pop10'])
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-44-1.png)<!-- -->

## ggplot2 Maps


```r
ggplot(pop2010geo_shifted, aes(fill = pop10)) + 
  geom_sf()
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-45-1.png)<!-- -->

## Fancier


```r
ggplot(pop2010geo_shifted, aes(fill = pop10/1000000, color = pop10/1000000)) + 
  geom_sf() +
  scale_fill_viridis_c() + scale_color_viridis_c(guide = FALSE) +
  theme_minimal() +
  labs(title = "Population by State, 2010", fill="Population (millions)")
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-46-1.png)<!-- -->

## Challenge 

Create a `map` of CA Population in 2010 by county


## 2010 pop Data for California Counties

```r
cal_pop10 <- get_decennial(geography = "county", 
                           variables = "P001001",
                           year = 2010, 
                           state='CA',
                           geometry=TRUE)
```

```
## Getting data from the 2010 decennial Census
```


## Map it


```r
plot(cal_pop10['value'])
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-48-1.png)<!-- -->

## Fetch County data for more than one state

We can fetch both the census data and geometry for more than one state.

```r
west_pop10 <- get_decennial(geography = "county", 
                           variables =  "P001001",
                           year = 2010, 
                           state=c('CA','OR','NV',"AZ"),
                           geometry=T)
```

```
## Getting data from the 2010 decennial Census
```


## Census Tract Data

Fetching the data for all `tracts` in one state.

*Need to specify county!*.

```r
# Optional state & county argments to get more limited geo subset
alco_pop10 <- get_decennial(geography = "tract", 
                           variables = "P001001", 
                           year = 2010, 
                           state='CA',
                           county='Alameda',
                           geometry=T)
```

```
## Getting data from the 2010 decennial Census
```

## Challenge

Fetch and map the 2010 population by census tract for Alameda and Countra Costa counties.


## Mapping Tract data


```r
alcc_pop10 <- get_decennial(geography = "tract", 
                      variables = "P001001", 
                      year = 2010, 
                      state='CA',
                      county=c("Alameda","Contra Costa"),
                      geometry=T) 
```

```
## Getting data from the 2010 decennial Census
## Getting data from the 2010 decennial Census
## Getting data from the 2010 decennial Census
```

```r
plot(alcc_pop10['value'])
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-51-1.png)<!-- -->


## Challenge

Fetch and map the percent of San Francicso properties by census tract that were coded as rented in the 2010 Census.

Step 1 - indentify the variables for the

- total number of hounsing units 

- number of renter occupied units

## SF Rented Units, 2010 

```r
sf_rented <- get_decennial(geography = "tract",  # census tabulation unit
                           variables =  "H004004",
                           year = 2010, 
                           summary_var = "H004001",  # Total Urban - the denominator
                           state='CA',
                           county='San Francisco',
                           geometry=T)

sf_pct_rented <- sf_rented[sf_rented$value > 0,] %>%
                 mutate(pct = 100 * (value / summary_value))

plot(sf_pct_rented['pct'])
```

# Questions?

# Part 3. ACS 5 year data

## ACS 5 year

You can use the tidycensus `get_acs` function to retrieve data for the ACS 5 year products, beginning with the 2005 - 2010 dataset. 

The default end year is the most recent one in `tidycensus` which is **2016** as of Dec 6, 2018.

`get_acs` syntax and options are very similar to `get_decennial`. 

But there are many more data variables!
  
## Fetch List of ACS 5 year Variables
  
Let's start by fetching ACS 5-year 2016 data on poverty. 

We want to explore the number of folks living below the poverty level by census tract.

First we need to find the variable name(s)!

## Load ACS Table Vars

Load the ACS 2012-2016 5 year data variables into a dataframe.

- ACS 5 year datasets are referenced by `end year` in tidycensus!

Then search for the variable names.

How many variables refer to the concept of poverty?


```r
acs2016vars <- load_variables(year=2016, dataset = 'acs5', cache = T)
#View(acs2016vars)
```

## ACS Tables and variables

Many hundreds (thousands?) more than for decennial census!

See the documentation on the [census website](https://www.census.gov/programs-surveys/acs/guidance/which-data-tool/table-ids-explained.html)

Types of tables:

- `B` prefix = base tables
- `C` = collapsed tables
- `DP` = data profiles
- `S` = Subject tables

## Census Reporter

ACS variables can  be confusing. 

The Census Reporter website (https://censusreporter.org) provides another tool for navigating topics, tables, and variable names.

Let's check it out to see what tables/variables we should use.

## get_acs

Use the tidycensus `get_acs` function to fetch the poverty data for census tracts in San Francisco

```r
?get_acs
```

## get_acs in action


```r
sf_poor <- get_acs(geography = "tract",  
                   table = "C17002",     # Table: ratio income to poverty
                   year = 2016,          
                   state="CA",
                   summary_var = "C17002_001", # Est of num people - denom
                   county="San Francisco",
                   geometry=T)               
```

```
## Getting data from the 2012-2016 5-year ACS
```

## View output

Let's take a look at the output of `get_acs` and discuss how it differs from `get_decennial`.

```r
 sf_poor
```

```
## Simple feature collection with 1576 features and 7 fields (with 8 geometries empty)
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -123.0139 ymin: 37.69274 xmax: -122.3276 ymax: 37.86334
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## First 10 features:
##          GEOID                                               NAME
## 1  06075010100 Census Tract 101, San Francisco County, California
## 2  06075010100 Census Tract 101, San Francisco County, California
## 3  06075010100 Census Tract 101, San Francisco County, California
## 4  06075010100 Census Tract 101, San Francisco County, California
## 5  06075010100 Census Tract 101, San Francisco County, California
## 6  06075010100 Census Tract 101, San Francisco County, California
## 7  06075010100 Census Tract 101, San Francisco County, California
## 8  06075010100 Census Tract 101, San Francisco County, California
## 9  06075010200 Census Tract 102, San Francisco County, California
## 10 06075010200 Census Tract 102, San Francisco County, California
##      variable estimate moe summary_est summary_moe
## 1  C17002_001     3972 291        3972         291
## 2  C17002_002      314 196        3972         291
## 3  C17002_003      397 185        3972         291
## 4  C17002_004      120 130        3972         291
## 5  C17002_005      236 129        3972         291
## 6  C17002_006      123 112        3972         291
## 7  C17002_007      444 304        3972         291
## 8  C17002_008     2338 387        3972         291
## 9  C17002_001     4300 442        4300         442
## 10 C17002_002      160 123        4300         442
##                          geometry
## 1  MULTIPOLYGON (((-122.4206 3...
## 2  MULTIPOLYGON (((-122.4206 3...
## 3  MULTIPOLYGON (((-122.4206 3...
## 4  MULTIPOLYGON (((-122.4206 3...
## 5  MULTIPOLYGON (((-122.4206 3...
## 6  MULTIPOLYGON (((-122.4206 3...
## 7  MULTIPOLYGON (((-122.4206 3...
## 8  MULTIPOLYGON (((-122.4206 3...
## 9  MULTIPOLYGON (((-122.4267 3...
## 10 MULTIPOLYGON (((-122.4267 3...
```

## map it

First remove census tracts that have no people!

```r
sf_poor2 <- subset(sf_poor, summary_est > 0)
plot(sf_poor2['estimate'])
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-57-1.png)<!-- -->

## Calcuting percents

Let's calculate the percent below poverty by tract.


```r
sf_poor3 <- sf_poor2 %>%
  mutate(pct = 100 * (estimate / summary_est))

sf_poor3
```

```
## Simple feature collection with 1560 features and 8 fields
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -122.5145 ymin: 37.70813 xmax: -122.3276 ymax: 37.86334
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## First 10 features:
##          GEOID                                               NAME
## 1  06075010100 Census Tract 101, San Francisco County, California
## 2  06075010100 Census Tract 101, San Francisco County, California
## 3  06075010100 Census Tract 101, San Francisco County, California
## 4  06075010100 Census Tract 101, San Francisco County, California
## 5  06075010100 Census Tract 101, San Francisco County, California
## 6  06075010100 Census Tract 101, San Francisco County, California
## 7  06075010100 Census Tract 101, San Francisco County, California
## 8  06075010100 Census Tract 101, San Francisco County, California
## 9  06075010200 Census Tract 102, San Francisco County, California
## 10 06075010200 Census Tract 102, San Francisco County, California
##      variable estimate moe summary_est summary_moe        pct
## 1  C17002_001     3972 291        3972         291 100.000000
## 2  C17002_002      314 196        3972         291   7.905337
## 3  C17002_003      397 185        3972         291   9.994965
## 4  C17002_004      120 130        3972         291   3.021148
## 5  C17002_005      236 129        3972         291   5.941591
## 6  C17002_006      123 112        3972         291   3.096677
## 7  C17002_007      444 304        3972         291  11.178248
## 8  C17002_008     2338 387        3972         291  58.862034
## 9  C17002_001     4300 442        4300         442 100.000000
## 10 C17002_002      160 123        4300         442   3.720930
##                          geometry
## 1  MULTIPOLYGON (((-122.4206 3...
## 2  MULTIPOLYGON (((-122.4206 3...
## 3  MULTIPOLYGON (((-122.4206 3...
## 4  MULTIPOLYGON (((-122.4206 3...
## 5  MULTIPOLYGON (((-122.4206 3...
## 6  MULTIPOLYGON (((-122.4206 3...
## 7  MULTIPOLYGON (((-122.4206 3...
## 8  MULTIPOLYGON (((-122.4206 3...
## 9  MULTIPOLYGON (((-122.4267 3...
## 10 MULTIPOLYGON (((-122.4267 3...
```

## subset to isolate vars of interest

We want to subset on and then combine the percents of people in each tract who are below 50% and 99% of the poverty line.

```r
sf_poor4 <- subset(sf_poor3, (variable %in% c("C17002_002","C17002_003")))
head(sf_poor4)
```

```
## Simple feature collection with 6 features and 8 fields
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -122.4267 ymin: 37.79847 xmax: -122.3996 ymax: 37.81144
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
##          GEOID                                               NAME
## 2  06075010100 Census Tract 101, San Francisco County, California
## 3  06075010100 Census Tract 101, San Francisco County, California
## 10 06075010200 Census Tract 102, San Francisco County, California
## 11 06075010200 Census Tract 102, San Francisco County, California
## 18 06075010300 Census Tract 103, San Francisco County, California
## 19 06075010300 Census Tract 103, San Francisco County, California
##      variable estimate moe summary_est summary_moe      pct
## 2  C17002_002      314 196        3972         291 7.905337
## 3  C17002_003      397 185        3972         291 9.994965
## 10 C17002_002      160 123        4300         442 3.720930
## 11 C17002_003       75  55        4300         442 1.744186
## 18 C17002_002      194 102        4341         426 4.469016
## 19 C17002_003      431 204        4341         426 9.928588
##                          geometry
## 2  MULTIPOLYGON (((-122.4206 3...
## 3  MULTIPOLYGON (((-122.4206 3...
## 10 MULTIPOLYGON (((-122.4267 3...
## 11 MULTIPOLYGON (((-122.4267 3...
## 18 MULTIPOLYGON (((-122.4187 3...
## 19 MULTIPOLYGON (((-122.4187 3...
```

## Group by and sum

```r
sf_poor5 <- sf_poor4 %>%
  select(GEOID, pct, geometry) %>%
  group_by(GEOID) %>% 
  summarise(pct_below_pov = sum(pct))

head(sf_poor5)
```

```
## Simple feature collection with 6 features and 2 fields
## geometry type:  POLYGON
## dimension:      XY
## bbox:           xmin: -122.4267 ymin: 37.79323 xmax: -122.3897 ymax: 37.81144
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## # A tibble: 6 x 3
##   GEOID       pct_below_pov                                       geometry
##   <chr>               <dbl>                                  <POLYGON [°]>
## 1 06075010100         17.9  ((-122.4206 37.81111, -122.4075 37.81144, -12…
## 2 06075010200          5.47 ((-122.4267 37.80964, -122.4249 37.8108, -122…
## 3 06075010300         14.4  ((-122.4187 37.80593, -122.4169 37.80521, -12…
## 4 06075010400          7.93 ((-122.4149 37.80354, -122.4116 37.80396, -12…
## 5 06075010500          8.53 ((-122.4052 37.80476, -122.4035 37.80509, -12…
## 6 06075010600         24.7  ((-122.411 37.80117, -122.4078 37.80157, -122…
```

## Map it
Where are SF's poorest areas?

```r
plot(sf_poor5['pct_below_pov'])
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-61-1.png)<!-- -->

# Interactive Maps with `tmap`

## tmap

The `tmap` package is great for making both static and interactive maps. It turns R into a `GIS`.

Let's check it out with our last dataframe.

## tmap


```r
library(tmap)
```

```
## Warning: package 'tmap' was built under R version 3.4.4
```

```r
tmap_mode("view") # set mode to interactive
```

```
## tmap mode set to interactive viewing
```

```r
poverty_map <- tm_shape(sf_poor5) +
                  tm_polygons(col="pct_below_pov")
```

## tmap

View the map - click on tracts


```r
poverty_map
```

<!--html_preserve--><div id="htmlwidget-ec0d5bb91aa66b49baa7" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-ec0d5bb91aa66b49baa7">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addProviderTiles","args":["Esri.WorldGrayCanvas",null,"Esri.WorldGrayCanvas",{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"pane":"tilePane"}]},{"method":"addProviderTiles","args":["OpenStreetMap",null,"OpenStreetMap",{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"pane":"tilePane"}]},{"method":"addProviderTiles","args":["Esri.WorldTopoMap",null,"Esri.WorldTopoMap",{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"pane":"tilePane"}]},{"method":"createMapPane","args":["overlayPane01",401]},{"method":"addPolygons","args":[[[[{"lng":[-122.420612315115,-122.407452,-122.399638840473,-122.401787,-122.403495,-122.405174,-122.406625,-122.408332,-122.411581,-122.414877,-122.416888,-122.418718,-122.419224,-122.420612315115],"lat":[37.8111121700604,37.811441,37.8065658502084,37.805314,37.805089,37.804763,37.804535,37.804367,37.803963,37.803542,37.805212,37.805932,37.808453,37.8111121700604]}]],[[{"lng":[-122.426671,-122.424876,-122.425041162157,-122.420612315115,-122.419224,-122.418718,-122.418281,-122.417799,-122.417587,-122.420769,-122.420883,-122.424275,-122.424464,-122.424835,-122.425209,-122.425628,-122.426257,-122.426613,-122.426671],"lat":[37.809639,37.810799,37.811001508766,37.8111121700604,37.808453,37.805932,37.804023,37.801267,37.800341,37.799939,37.799902,37.799472,37.800402,37.802268,37.804133,37.806456,37.807185,37.808152,37.809639]}]],[[{"lng":[-122.418718,-122.416888,-122.414877,-122.412908,-122.412849,-122.412471,-122.412283,-122.417218,-122.417385,-122.417587,-122.417799,-122.418281,-122.418718],"lat":[37.805932,37.805212,37.803542,37.802184,37.801893,37.800032,37.799096,37.798472,37.799414,37.800341,37.801267,37.804023,37.805932]}]],[[{"lng":[-122.414877,-122.411581,-122.408332,-122.406625,-122.405174,-122.404798,-122.40316,-122.40279,-122.402421,-122.405645,-122.406024,-122.406199,-122.40777,-122.411019,-122.411376,-122.412908,-122.414877],"lat":[37.803542,37.803963,37.804367,37.804535,37.804763,37.802901,37.803098,37.801242,37.799382,37.798969,37.800835,37.80172,37.801571,37.80117,37.801125,37.802184,37.803542]}]],[[{"lng":[-122.405174,-122.403495,-122.401787,-122.399638840473,-122.398139,-122.394407919349,-122.389708268207,-122.391487,-122.393181,-122.394748,-122.396315,-122.396586,-122.400149,-122.400681,-122.402041,-122.402421,-122.40279,-122.40316,-122.404798,-122.405174],"lat":[37.804763,37.805089,37.805314,37.8065658502084,37.80563,37.8012904644047,37.7958244063587,37.793859,37.79323,37.79448,37.793337,37.794588,37.794134,37.796777,37.797505,37.799382,37.801242,37.803098,37.802901,37.804763]}]],[[{"lng":[-122.411019,-122.40777,-122.406199,-122.406024,-122.405645,-122.402421,-122.402041,-122.405513,-122.406668,-122.407083,-122.407869,-122.409014,-122.411376,-122.411019],"lat":[37.80117,37.801571,37.80172,37.800835,37.798969,37.799382,37.797505,37.797065,37.797865,37.798152,37.798697,37.79949,37.801125,37.80117]}]],[[{"lng":[-122.412908,-122.411376,-122.409014,-122.407869,-122.407083,-122.406668,-122.405513,-122.408431,-122.411719,-122.411911,-122.412283,-122.412471,-122.412849,-122.412908],"lat":[37.802184,37.801125,37.79949,37.798697,37.798152,37.797865,37.797065,37.796704,37.796291,37.797245,37.799096,37.800032,37.801893,37.802184]}]],[[{"lng":[-122.417218,-122.412283,-122.411911,-122.411719,-122.411364,-122.415218,-122.416292,-122.416473,-122.416653,-122.416839,-122.417218],"lat":[37.798472,37.799096,37.797245,37.796291,37.794527,37.794029,37.793892,37.794777,37.79567,37.796596,37.798472]}]],[[{"lng":[-122.424275,-122.420883,-122.420769,-122.417587,-122.417385,-122.417218,-122.416839,-122.421768,-122.423481,-122.423705,-122.424275],"lat":[37.799472,37.799902,37.799939,37.800341,37.799414,37.798472,37.796596,37.795945,37.795739,37.796678,37.799472]}]],[[{"lng":[-122.423481,-122.421768,-122.416839,-122.416653,-122.416473,-122.416292,-122.416113,-122.422801,-122.423143,-122.423283,-122.423481],"lat":[37.795739,37.795945,37.796596,37.79567,37.794777,37.793892,37.793014,37.792161,37.793923,37.79481,37.795739]}]],[[{"lng":[-122.422801,-122.416113,-122.415936,-122.41575,-122.415559,-122.415372,-122.422077,-122.422237,-122.42242,-122.422611,-122.422801],"lat":[37.792161,37.793014,37.792133,37.791206,37.790254,37.78932,37.788474,37.789399,37.790358,37.791277,37.792161]}]],[[{"lng":[-122.416292,-122.415218,-122.411364,-122.409722,-122.409541,-122.409171,-122.412461,-122.41575,-122.415936,-122.416113,-122.416292],"lat":[37.793892,37.794029,37.794527,37.794737,37.793852,37.792043,37.791627,37.791206,37.792133,37.793014,37.793892]}]],[[{"lng":[-122.411719,-122.408431,-122.408253,-122.407903,-122.407536,-122.409171,-122.409541,-122.409722,-122.411364,-122.411719],"lat":[37.796291,37.796704,37.795824,37.79406,37.792249,37.792043,37.793852,37.794737,37.794527,37.796291]}]],[[{"lng":[-122.408786,-122.407149,-122.40734,-122.407536,-122.404418,-122.404613,-122.400149,-122.396586,-122.396315,-122.399195,-122.406062,-122.407451,-122.407853,-122.408227,-122.408402,-122.408786],"lat":[37.79016,37.790366,37.791296,37.792249,37.792639,37.793565,37.794134,37.794588,37.793337,37.791058,37.785757,37.784604,37.785492,37.787359,37.788293,37.79016]}]],[[{"lng":[-122.407903,-122.404796,-122.404613,-122.404418,-122.407536,-122.407903],"lat":[37.79406,37.794453,37.793565,37.792639,37.792249,37.79406]}]],[[{"lng":[-122.41575,-122.412461,-122.412266,-122.412076,-122.415372,-122.415559,-122.41575],"lat":[37.791206,37.791627,37.790673,37.78974,37.78932,37.790254,37.791206]}]],[[{"lng":[-122.412461,-122.409171,-122.407536,-122.40734,-122.407149,-122.408786,-122.412076,-122.412266,-122.412461],"lat":[37.791627,37.792043,37.792249,37.791296,37.790366,37.79016,37.78974,37.790673,37.791627]}]],[[{"lng":[-122.422077,-122.415372,-122.415185,-122.414995,-122.418271,-122.421461,-122.421858,-122.422077],"lat":[37.788474,37.78932,37.788388,37.787454,37.787038,37.786632,37.787535,37.788474]}]],[[{"lng":[-122.415372,-122.412076,-122.408786,-122.408402,-122.411699,-122.413354,-122.414995,-122.415185,-122.415372],"lat":[37.78932,37.78974,37.79016,37.788293,37.787872,37.787664,37.787454,37.788388,37.78932]}]],[[{"lng":[-122.418271,-122.414995,-122.414617,-122.41443,-122.416072,-122.417707,-122.417901,-122.418271],"lat":[37.787038,37.787454,37.785582,37.784657,37.784449,37.784236,37.785167,37.787038]}]],[[{"lng":[-122.421461,-122.418271,-122.417901,-122.417707,-122.421069,-122.421256,-122.421461],"lat":[37.786632,37.787038,37.785167,37.784236,37.783816,37.784745,37.786632]}]],[[{"lng":[-122.414617,-122.41297,-122.411326,-122.411516,-122.408227,-122.407853,-122.41114,-122.412782,-122.41443,-122.414617],"lat":[37.785582,37.78579,37.785996,37.78694,37.787359,37.785492,37.785075,37.784866,37.784657,37.785582]}]],[[{"lng":[-122.414995,-122.413354,-122.411699,-122.408402,-122.408227,-122.411516,-122.411326,-122.41297,-122.414617,-122.414995],"lat":[37.787454,37.787664,37.787872,37.788293,37.787359,37.78694,37.785996,37.78579,37.785582,37.787454]}]],[[{"lng":[-122.417707,-122.416072,-122.41443,-122.414242,-122.414054,-122.413866,-122.415505,-122.417146,-122.417339,-122.417707],"lat":[37.784236,37.784449,37.784657,37.783724,37.782794,37.781863,37.781654,37.781447,37.782379,37.784236]}]],[[{"lng":[-122.421069,-122.417707,-122.417339,-122.417146,-122.415505,-122.413866,-122.413365,-122.416292,-122.417615,-122.418704,-122.419219,-122.419334,-122.419359,-122.419765,-122.420358,-122.420689,-122.421069],"lat":[37.783816,37.784236,37.782379,37.781447,37.781654,37.781863,37.779862,37.777494,37.7766,37.775645,37.775316,37.77521,37.775432,37.777291,37.780072,37.781955,37.783816]}]],[[{"lng":[-122.414054,-122.410765,-122.410952,-122.41114,-122.407853,-122.407451,-122.408104,-122.408952,-122.410336,-122.412548,-122.413365,-122.413866,-122.414054],"lat":[37.782794,37.783214,37.784141,37.785075,37.785492,37.784604,37.78402,37.783288,37.782245,37.780508,37.779862,37.781863,37.782794]}]],[[{"lng":[-122.41443,-122.412782,-122.41114,-122.410952,-122.410765,-122.414054,-122.414242,-122.41443],"lat":[37.784657,37.784866,37.785075,37.784141,37.783214,37.782794,37.783724,37.784657]}]],[[{"lng":[-122.441905,-122.437138,-122.436799,-122.436274,-122.441177,-122.441368,-122.441754,-122.441905],"lat":[37.803744,37.804352,37.802715,37.800814,37.80019,37.801126,37.802985,37.803744]}]],[[{"lng":[-122.441909,-122.439690935158,-122.425942,-122.425041162157,-122.424876,-122.426671,-122.426613,-122.426257,-122.425628,-122.425209,-122.430045,-122.431679,-122.436799,-122.437138,-122.441905,-122.441909],"lat":[37.807316,37.8086811541357,37.810979,37.811001508766,37.810799,37.809639,37.808152,37.807185,37.806456,37.804133,37.803518,37.803267,37.802715,37.804352,37.803744,37.807316]}]],[[{"lng":[-122.448241,-122.448241130218,-122.439690935158,-122.441909,-122.441905,-122.441754,-122.441368,-122.441177,-122.447466,-122.44782,-122.44946,-122.448241],"lat":[37.804478,37.8072521685356,37.8086811541357,37.807316,37.803744,37.802985,37.801126,37.80019,37.79939,37.801633,37.802454,37.804478]}]],[[{"lng":[-122.447466,-122.441177,-122.436274,-122.435678,-122.437326,-122.43695,-122.438591,-122.440234,-122.446728,-122.447015,-122.447303,-122.447466],"lat":[37.79939,37.80019,37.800814,37.798021,37.797811,37.795949,37.795741,37.795533,37.794706,37.796579,37.798459,37.79939]}]],[[{"lng":[-122.430045,-122.425209,-122.424835,-122.424464,-122.42929,-122.429667,-122.430045],"lat":[37.803518,37.804133,37.802268,37.800402,37.799788,37.801654,37.803518]}]],[[{"lng":[-122.436799,-122.431679,-122.430045,-122.429667,-122.42929,-122.424464,-122.424275,-122.427457,-122.430747,-122.432393,-122.434036,-122.435678,-122.436274,-122.436799],"lat":[37.802715,37.803267,37.803518,37.801654,37.799788,37.800402,37.799472,37.799067,37.798648,37.798439,37.79823,37.798021,37.800814,37.802715]}]],[[{"lng":[-122.437326,-122.435678,-122.434036,-122.432393,-122.430747,-122.427457,-122.424275,-122.423705,-122.426893,-122.430183,-122.433469,-122.435113,-122.43695,-122.437326],"lat":[37.797811,37.798021,37.79823,37.798439,37.798648,37.799067,37.799472,37.796678,37.796272,37.795854,37.795435,37.795226,37.795949,37.797811]}]],[[{"lng":[-122.426893,-122.423705,-122.423481,-122.423283,-122.423143,-122.422801,-122.422611,-122.425802,-122.426335,-122.426893],"lat":[37.796272,37.796678,37.795739,37.79481,37.793923,37.792161,37.791277,37.790874,37.793517,37.796272]}]],[[{"lng":[-122.430183,-122.426893,-122.426335,-122.425802,-122.422611,-122.42242,-122.425616,-122.428905,-122.42927,-122.429625,-122.430183],"lat":[37.795854,37.796272,37.793517,37.790874,37.791277,37.790358,37.789952,37.789534,37.791339,37.793098,37.795854]}]],[[{"lng":[-122.446728,-122.440234,-122.438591,-122.43695,-122.435113,-122.433469,-122.430183,-122.429625,-122.434558,-122.436204,-122.437846,-122.439488,-122.446117,-122.4463,-122.446728],"lat":[37.794706,37.795533,37.795741,37.795949,37.795226,37.795435,37.795854,37.793098,37.792472,37.792263,37.792054,37.791843,37.790999,37.791879,37.794706]}]],[[{"lng":[-122.459476,-122.45274,-122.4463,-122.446117,-122.445934,-122.445758,-122.445589,-122.445402,-122.450118,-122.454774,-122.45505,-122.459179,-122.459388,-122.459476],"lat":[37.789711,37.790974,37.791879,37.790999,37.790119,37.789248,37.788364,37.787438,37.786838,37.786244,37.786209,37.785682,37.788405,37.789711]}]],[[{"lng":[-122.446117,-122.439488,-122.437846,-122.436204,-122.435849,-122.435486,-122.438773,-122.440488,-122.443753,-122.445402,-122.445589,-122.445758,-122.445934,-122.446117],"lat":[37.790999,37.791843,37.792054,37.792263,37.790502,37.788697,37.788278,37.788061,37.787647,37.787438,37.788364,37.789248,37.790119,37.790999]}]],[[{"lng":[-122.436204,-122.434558,-122.429625,-122.42927,-122.428905,-122.43384,-122.435486,-122.435849,-122.436204],"lat":[37.792263,37.792472,37.793098,37.791339,37.789534,37.788906,37.788697,37.790502,37.792263]}]],[[{"lng":[-122.425616,-122.42242,-122.422237,-122.422077,-122.421858,-122.421461,-122.421256,-122.422838,-122.424678,-122.424704,-122.425047,-122.425616],"lat":[37.789952,37.790358,37.789399,37.788474,37.787535,37.786632,37.784745,37.784547,37.785291,37.785418,37.78713,37.789952]}]],[[{"lng":[-122.435486,-122.43384,-122.428905,-122.425616,-122.425047,-122.428336,-122.431635,-122.434918,-122.435486],"lat":[37.788697,37.788906,37.789534,37.789952,37.78713,37.786713,37.786293,37.785876,37.788697]}]],[[{"lng":[-122.443753,-122.440488,-122.438773,-122.435486,-122.434918,-122.436557,-122.438206,-122.443183,-122.44337,-122.443753],"lat":[37.787647,37.788061,37.788278,37.788697,37.785876,37.785668,37.785459,37.784827,37.785759,37.787647]}]],[[{"lng":[-122.459179,-122.45505,-122.454774,-122.450118,-122.445402,-122.443753,-122.44337,-122.443183,-122.44281,-122.442793,-122.445826,-122.447302,-122.454186,-122.456304,-122.458878,-122.458998,-122.459179],"lat":[37.785682,37.786209,37.786244,37.786838,37.787438,37.787647,37.785759,37.784827,37.782971,37.782885,37.782512,37.782392,37.781651,37.781449,37.781326,37.783189,37.785682]}]],[[{"lng":[-122.443183,-122.438206,-122.436557,-122.434918,-122.431635,-122.428336,-122.425047,-122.424704,-122.431291,-122.434562,-122.437863,-122.438901,-122.44281,-122.443183],"lat":[37.784827,37.785459,37.785668,37.785876,37.786293,37.786713,37.78713,37.785418,37.784621,37.784152,37.783791,37.78345,37.782971,37.784827]}]],[[{"lng":[-122.458878,-122.456304,-122.454186,-122.453385,-122.455217,-122.454683,-122.458371,-122.458505,-122.458878],"lat":[37.781326,37.781449,37.781651,37.777827,37.777599,37.774755,37.774309,37.77619,37.781326]}]],[[{"lng":[-122.455217,-122.453385,-122.454186,-122.447302,-122.445826,-122.442793,-122.441512,-122.440958,-122.442053,-122.441866,-122.441488,-122.44478,-122.446471,-122.448068,-122.451358,-122.454683,-122.455217],"lat":[37.777599,37.777827,37.781651,37.782392,37.782512,37.782885,37.782096,37.779364,37.779244,37.7783,37.776435,37.776017,37.775802,37.775599,37.775175,37.774755,37.777599]}]],[[{"lng":[-122.44281,-122.438901,-122.437863,-122.434562,-122.433973,-122.433596,-122.433407,-122.436697,-122.437072,-122.440958,-122.441512,-122.442793,-122.44281],"lat":[37.782971,37.78345,37.783791,37.784152,37.781219,37.779355,37.778422,37.778006,37.779867,37.779364,37.782096,37.782885,37.782971]}]],[[{"lng":[-122.442053,-122.440958,-122.437072,-122.436697,-122.433407,-122.433219,-122.438244,-122.441488,-122.441866,-122.442053],"lat":[37.779244,37.779364,37.779867,37.778006,37.778422,37.77749,37.776848,37.776435,37.7783,37.779244]}]],[[{"lng":[-122.434562,-122.431291,-122.424704,-122.424678,-122.424108,-122.427396,-122.430696,-122.433973,-122.434562],"lat":[37.784152,37.784621,37.785418,37.785291,37.782477,37.782057,37.781635,37.781219,37.784152]}]],[[{"lng":[-122.424678,-122.422838,-122.421256,-122.421069,-122.420689,-122.420358,-122.423541,-122.424108,-122.424678],"lat":[37.785291,37.784547,37.784745,37.783816,37.781955,37.780072,37.779674,37.782477,37.785291]}]],[[{"lng":[-122.433973,-122.430696,-122.427396,-122.424108,-122.423541,-122.423354,-122.424998,-122.426641,-122.429929,-122.433219,-122.433407,-122.433596,-122.433973],"lat":[37.781219,37.781635,37.782057,37.782477,37.779674,37.778747,37.778538,37.778329,37.777909,37.77749,37.778422,37.779355,37.781219]}]],[[{"lng":[-122.426641,-122.424998,-122.423354,-122.423541,-122.420358,-122.419765,-122.419359,-122.424246,-122.425888,-122.426263,-122.426641],"lat":[37.778329,37.778538,37.778747,37.779674,37.780072,37.777291,37.775432,37.774811,37.774599,37.776464,37.778329]}]],[[{"lng":[-122.433219,-122.429929,-122.426641,-122.426263,-122.425888,-122.429178,-122.430825,-122.432467,-122.433219],"lat":[37.77749,37.777909,37.778329,37.776464,37.774599,37.774181,37.773973,37.773757,37.77749]}]],[[{"lng":[-122.441488,-122.438244,-122.433219,-122.432467,-122.435751,-122.437469,-122.440735,-122.440922,-122.441488],"lat":[37.776435,37.776848,37.77749,37.773757,37.773346,37.773127,37.772713,37.773635,37.776435]}]],[[{"lng":[-122.454683,-122.451358,-122.448068,-122.446471,-122.44478,-122.441488,-122.440922,-122.444216,-122.447497,-122.450787,-122.454083,-122.454683],"lat":[37.774755,37.775175,37.775599,37.775802,37.776017,37.776435,37.773635,37.773212,37.77279,37.772368,37.77193,37.774755]}]],[[{"lng":[-122.453902,-122.4506,-122.445717,-122.440735,-122.440358,-122.443312,-122.442765,-122.44839,-122.450522,-122.453352,-122.453902],"lat":[37.771054,37.77146,37.77208,37.772713,37.770847,37.77047,37.769661,37.768865,37.768593,37.768246,37.771054]}]],[[{"lng":[-122.443866,-122.443356,-122.442765,-122.443312,-122.440358,-122.440735,-122.437469,-122.435751,-122.432467,-122.431904,-122.43157,-122.433572,-122.435794,-122.436588,-122.438338,-122.439301,-122.442009,-122.442575,-122.443533,-122.443866],"lat":[37.767536,37.768944,37.769661,37.77047,37.770847,37.772713,37.773127,37.773346,37.773757,37.770964,37.769307,37.769178,37.769058,37.769001,37.769007,37.768166,37.765907,37.765928,37.766691,37.767536]}]],[[{"lng":[-122.432467,-122.430825,-122.42899,-122.428802,-122.425512,-122.424929,-122.426402,-122.42822,-122.43157,-122.431904,-122.432467],"lat":[37.773757,37.773973,37.773248,37.772316,37.772734,37.770778,37.769596,37.769441,37.769307,37.770964,37.773757]}]],[[{"lng":[-122.429178,-122.425888,-122.424246,-122.419359,-122.419334,-122.422186,-122.42262,-122.423641,-122.424929,-122.425512,-122.428802,-122.42899,-122.430825,-122.429178],"lat":[37.774181,37.774599,37.774811,37.775432,37.77521,37.772909,37.772503,37.771828,37.770778,37.772734,37.772316,37.773248,37.773973,37.774181]}]],[[{"lng":[-122.435794,-122.433572,-122.43157,-122.42822,-122.426402,-122.426948,-122.428635,-122.428949,-122.430824,-122.435193,-122.435472,-122.435794],"lat":[37.769058,37.769178,37.769307,37.769441,37.769596,37.769175,37.767846,37.767504,37.766014,37.762727,37.765727,37.769058]}]],[[{"lng":[-122.446247,-122.443538,-122.443347,-122.442575,-122.442009,-122.439301,-122.438338,-122.436588,-122.435794,-122.435472,-122.435193,-122.435837,-122.439491,-122.440168,-122.444196,-122.446384,-122.446247],"lat":[37.762247,37.765323,37.765333,37.765928,37.765907,37.768166,37.769007,37.769001,37.769058,37.765727,37.762727,37.762485,37.762227,37.762187,37.761947,37.761816,37.762247]}]],[[{"lng":[-122.449433,-122.448274,-122.447635,-122.448013,-122.44839,-122.442765,-122.443356,-122.443866,-122.443533,-122.442575,-122.443347,-122.443538,-122.446247,-122.446384,-122.446783,-122.44912,-122.449433],"lat":[37.7632,37.764196,37.765139,37.767007,37.768865,37.769661,37.768944,37.767536,37.766691,37.765928,37.765333,37.765323,37.762247,37.761816,37.761781,37.761648,37.7632]}]],[[{"lng":[-122.453352,-122.450522,-122.44839,-122.448013,-122.447635,-122.448274,-122.449433,-122.44912,-122.451958,-122.4524,-122.452569,-122.452758,-122.452947,-122.453352],"lat":[37.768246,37.768593,37.768865,37.767007,37.765139,37.764196,37.7632,37.761648,37.761478,37.763669,37.764509,37.765443,37.766374,37.768246]}]],[[{"lng":[-122.417615,-122.416292,-122.413365,-122.412548,-122.410336,-122.408952,-122.408104,-122.407451,-122.406062,-122.402708,-122.404933,-122.405719,-122.407159,-122.409387,-122.411606,-122.413161,-122.415561,-122.418704,-122.417615],"lat":[37.7766,37.777494,37.779862,37.780508,37.782245,37.783288,37.78402,37.784604,37.785757,37.783259,37.7815,37.780878,37.779739,37.777977,37.776221,37.774992,37.773101,37.775645,37.7766]}]],[[{"lng":[-122.418704,-122.415561,-122.41248,-122.410931,-122.409736,-122.408007,-122.40499,-122.404497,-122.40509,-122.408424,-122.413003,-122.413588,-122.417332,-122.417487,-122.417802,-122.417794,-122.419219,-122.418704],"lat":[37.775645,37.773101,37.77063,37.769411,37.770349,37.769244,37.769715,37.764664,37.764628,37.764427,37.764091,37.763823,37.763572,37.765183,37.768405,37.770435,37.775316,37.775645]}]],[[{"lng":[-122.404933,-122.402708,-122.400468,-122.39893,-122.397386,-122.401843,-122.403387,-122.404933],"lat":[37.7815,37.783259,37.78503,37.783785,37.782554,37.779032,37.780265,37.7815]}]],[[{"lng":[-122.413161,-122.411606,-122.409387,-122.407159,-122.405719,-122.404933,-122.403387,-122.401843,-122.40407,-122.408516,-122.41248,-122.415561,-122.413161],"lat":[37.774992,37.776221,37.777977,37.779739,37.780878,37.7815,37.780265,37.779032,37.777273,37.77376,37.77063,37.773101,37.774992]}]],[[{"lng":[-122.376458,-122.375979,-122.374328,-122.373537,-122.372923,-122.371996,-122.375168,-122.374905,-122.374951,-122.375563,-122.376018,-122.376458],"lat":[37.738252,37.738477,37.73779,37.737176,37.737043,37.737043,37.736504,37.737092,37.737563,37.737987,37.737991,37.738252]}]],[[{"lng":[-122.332045780727,-122.327561619753,-122.33227,-122.332045780727],"lat":[37.7877603470098,37.7806441119025,37.781517,37.7877603470098]}]],[[{"lng":[-122.377679,-122.373453,-122.368752,-122.362911,-122.364289,-122.358779,-122.360775,-122.361107,-122.362294,-122.367138,-122.371649,-122.372836,-122.371269,-122.378962,-122.377679],"lat":[37.83046,37.832298,37.831286,37.822696,37.81887,37.814278,37.812831,37.808029,37.807016,37.807429,37.809379,37.811068,37.814669,37.826897,37.83046]}]],[[{"lng":[-122.420365736738,-122.419585,-122.41847,-122.419827349401,-122.420365736738],"lat":[37.8633422971737,37.863282,37.861764,37.8600830082779,37.8633422971737]}]],[[{"lng":[-122.41248,-122.408516,-122.40407,-122.401843,-122.397386,-122.39275,-122.397206,-122.398755,-122.400982,-122.399433,-122.401657,-122.404391,-122.40499,-122.408007,-122.409736,-122.410931,-122.41248],"lat":[37.77063,37.77376,37.777273,37.779032,37.782554,37.778852,37.775332,37.776568,37.774809,37.773573,37.771817,37.76975,37.769715,37.769244,37.770349,37.769411,37.77063]}]],[[{"lng":[-122.42262,-122.422186,-122.419334,-122.419219,-122.417794,-122.417802,-122.417487,-122.417332,-122.421732,-122.422044,-122.422308,-122.422365,-122.42262],"lat":[37.772503,37.772909,37.77521,37.775316,37.770435,37.768405,37.765183,37.763572,37.7633,37.76654,37.769278,37.769868,37.772503]}]],[[{"lng":[-122.426948,-122.426402,-122.424929,-122.423641,-122.42262,-122.422365,-122.422308,-122.422044,-122.421732,-122.426137,-122.426293,-122.426462,-122.426713,-122.426948],"lat":[37.769175,37.769596,37.770778,37.771828,37.772503,37.769868,37.769278,37.76654,37.7633,37.763036,37.764651,37.766272,37.768982,37.769175]}]],[[{"lng":[-122.435193,-122.430824,-122.428949,-122.428635,-122.426948,-122.426713,-122.426462,-122.426293,-122.426137,-122.430727,-122.432124,-122.435188,-122.435193],"lat":[37.762727,37.766014,37.767504,37.767846,37.769175,37.768982,37.766272,37.764651,37.763036,37.762753,37.762668,37.762671,37.762727]}]],[[{"lng":[-122.447915,-122.446395,-122.446394,-122.446783,-122.446384,-122.444196,-122.440168,-122.439491,-122.439337,-122.439167,-122.442175,-122.442258,-122.44222,-122.445106,-122.444485,-122.445283,-122.44695,-122.447915],"lat":[37.757589,37.758856,37.760907,37.761781,37.761816,37.761947,37.762187,37.762227,37.760627,37.759028,37.758939,37.757734,37.756639,37.756961,37.756354,37.756573,37.75655,37.757589]}]],[[{"lng":[-122.449447,-122.447794,-122.445697,-122.449423,-122.44709,-122.446734,-122.447943,-122.445348,-122.44695,-122.445283,-122.444485,-122.445106,-122.44222,-122.442258,-122.442175,-122.439167,-122.438864,-122.440345,-122.442525,-122.443255,-122.443421,-122.44462,-122.444556,-122.444783,-122.444857,-122.447399,-122.449363,-122.45084,-122.449447],"lat":[37.74666,37.749408,37.748855,37.750412,37.751505,37.753573,37.755532,37.755619,37.75655,37.756573,37.756354,37.756961,37.756639,37.757734,37.758939,37.759028,37.755434,37.755228,37.75238,37.750769,37.750099,37.747081,37.746687,37.746771,37.747025,37.746596,37.746129,37.745904,37.74666]}]],[[{"lng":[-122.439491,-122.435837,-122.435193,-122.435188,-122.435001,-122.434854,-122.434698,-122.434546,-122.437769,-122.438864,-122.439167,-122.439337,-122.439491],"lat":[37.762227,37.762485,37.762727,37.762671,37.760889,37.759289,37.757687,37.756093,37.755541,37.755434,37.759028,37.760627,37.762227]}]],[[{"lng":[-122.435188,-122.432124,-122.430727,-122.426137,-122.425836,-122.425685,-122.425532,-122.430115,-122.432331,-122.434546,-122.434698,-122.434854,-122.435001,-122.435188],"lat":[37.762671,37.762668,37.762753,37.763036,37.759835,37.758232,37.756636,37.75636,37.756227,37.756093,37.757687,37.759289,37.760889,37.762671]}]],[[{"lng":[-122.426137,-122.421732,-122.421578,-122.421425,-122.421118,-122.420964,-122.425384,-122.425532,-122.425685,-122.425836,-122.426137],"lat":[37.763036,37.7633,37.761701,37.760101,37.756902,37.755295,37.755035,37.756636,37.758232,37.759835,37.763036]}]],[[{"lng":[-122.421732,-122.417332,-122.417179,-122.417026,-122.416872,-122.41672,-122.416567,-122.418748,-122.420964,-122.421118,-122.421425,-122.421578,-122.421732],"lat":[37.7633,37.763572,37.761968,37.760367,37.758764,37.757167,37.755568,37.755437,37.755295,37.756902,37.760101,37.761701,37.7633]}]],[[{"lng":[-122.420964,-122.418748,-122.416567,-122.416413,-122.416108,-122.41587,-122.418206,-122.420282,-122.420507,-122.420665,-122.420812,-122.420964],"lat":[37.755295,37.755437,37.755568,37.753968,37.750771,37.748267,37.748203,37.748161,37.750506,37.752104,37.753703,37.755295]}]],[[{"lng":[-122.425384,-122.420964,-122.420812,-122.420665,-122.420507,-122.420282,-122.421393,-122.42485,-122.424777,-122.424934,-122.425097,-122.42524,-122.425384],"lat":[37.755035,37.755295,37.753703,37.752104,37.750506,37.748161,37.748124,37.747825,37.748644,37.750237,37.751845,37.753436,37.755035]}]],[[{"lng":[-122.434546,-122.432331,-122.430115,-122.425532,-122.425384,-122.42524,-122.425097,-122.429659,-122.43409,-122.43424,-122.434317,-122.434546],"lat":[37.756093,37.756227,37.75636,37.756636,37.755035,37.753436,37.751845,37.751571,37.751304,37.752891,37.75369,37.756093]}]],[[{"lng":[-122.442525,-122.440345,-122.438864,-122.437769,-122.434546,-122.434317,-122.43424,-122.43409,-122.438474,-122.443255,-122.442525],"lat":[37.75238,37.755228,37.755434,37.755541,37.756093,37.75369,37.752891,37.751304,37.751024,37.750769,37.75238]}]],[[{"lng":[-122.44462,-122.443421,-122.443255,-122.438474,-122.43409,-122.433769,-122.433697,-122.435916,-122.43809,-122.438246,-122.441917,-122.444556,-122.44462],"lat":[37.747081,37.750099,37.750769,37.751024,37.751304,37.748097,37.747297,37.747179,37.74705,37.748665,37.748474,37.746687,37.747081]}]],[[{"lng":[-122.43409,-122.429659,-122.425097,-122.424934,-122.424777,-122.42485,-122.427045,-122.431477,-122.433697,-122.433769,-122.43409],"lat":[37.751304,37.751571,37.751845,37.750237,37.748644,37.747825,37.747696,37.747434,37.747297,37.748097,37.751304]}]],[[{"lng":[-122.431477,-122.427045,-122.42485,-122.421393,-122.421704,-122.422276,-122.422709,-122.423361,-122.430888,-122.431022,-122.431256,-122.431477],"lat":[37.747434,37.747696,37.747825,37.748124,37.746412,37.745348,37.74394,37.74229,37.741833,37.742631,37.745032,37.747434]}]],[[{"lng":[-122.447399,-122.444857,-122.444783,-122.444556,-122.441917,-122.438246,-122.43809,-122.435916,-122.433697,-122.431477,-122.431256,-122.431022,-122.430888,-122.433259,-122.43332,-122.435559,-122.43556,-122.440049,-122.444148,-122.445679,-122.447399],"lat":[37.746596,37.747025,37.746771,37.746687,37.748474,37.748665,37.74705,37.747179,37.747297,37.747434,37.745032,37.742631,37.741833,37.741903,37.743296,37.743161,37.74155,37.745299,37.743297,37.744139,37.746596]}]],[[{"lng":[-122.45084,-122.449363,-122.447399,-122.445679,-122.444148,-122.440049,-122.43556,-122.435559,-122.43332,-122.433259,-122.430888,-122.430741,-122.431759,-122.428726,-122.431349,-122.432443,-122.434697,-122.434401,-122.435825,-122.438392,-122.439915,-122.443829,-122.443649,-122.442489,-122.442525,-122.445987,-122.446658,-122.449861,-122.451692,-122.45084],"lat":[37.745904,37.746129,37.746596,37.744139,37.743297,37.745299,37.74155,37.743161,37.743296,37.741903,37.741833,37.740424,37.739803,37.738326,37.736618,37.735879,37.737256,37.736199,37.733924,37.73442,37.734901,37.736134,37.736519,37.737289,37.739448,37.74118,37.742442,37.743265,37.745629,37.745904]}]],[[{"lng":[-122.434401,-122.434697,-122.432443,-122.431349,-122.428726,-122.431759,-122.430741,-122.430888,-122.423361,-122.424258,-122.42427,-122.424724,-122.425286,-122.426722,-122.428573,-122.429391,-122.432422,-122.433104,-122.435825,-122.434401],"lat":[37.736199,37.737256,37.735879,37.736618,37.738326,37.739803,37.740424,37.741833,37.74229,37.739935,37.739866,37.738431,37.73764,37.736372,37.735082,37.734636,37.733176,37.733419,37.733924,37.736199]}]],[[{"lng":[-122.393968,-122.388164,-122.387917,-122.385823,-122.380462169293,-122.379568,-122.380757,-122.378560351096,-122.380985,-122.381711,-122.390683,-122.391232,-122.392447,-122.392494,-122.392319,-122.39252,-122.392714,-122.393848,-122.393968],"lat":[37.766614,37.766958,37.764379,37.767255,37.7675163525902,37.76055,37.755288,37.7535517721854,37.753228,37.753186,37.752663,37.752657,37.756436,37.760245,37.76106,37.762819,37.764084,37.765307,37.766614]}]],[[{"lng":[-122.398703,-122.393848,-122.392714,-122.39252,-122.392319,-122.392494,-122.398216,-122.398338,-122.398461,-122.398703],"lat":[37.765014,37.765307,37.764084,37.762819,37.76106,37.760245,37.759909,37.761184,37.762463,37.765014]}]],[[{"lng":[-122.40648,-122.405105,-122.40509,-122.404497,-122.398703,-122.398461,-122.398338,-122.398216,-122.39797,-122.403689,-122.406389,-122.40648],"lat":[37.760731,37.763852,37.764628,37.764664,37.765014,37.762463,37.761184,37.759909,37.757359,37.757015,37.759804,37.760731]}]],[[{"lng":[-122.417332,-122.413588,-122.413003,-122.408424,-122.408064,-122.407754,-122.412348,-122.41672,-122.416872,-122.417026,-122.417179,-122.417332],"lat":[37.763572,37.763823,37.764091,37.764427,37.760601,37.757705,37.75743,37.757167,37.758764,37.760367,37.761968,37.763572]}]],[[{"lng":[-122.408424,-122.40509,-122.405105,-122.40648,-122.406389,-122.403689,-122.403396,-122.403127,-122.406453,-122.407449,-122.407602,-122.407754,-122.408064,-122.408424],"lat":[37.764427,37.764628,37.763852,37.760731,37.759804,37.757015,37.756471,37.754478,37.754277,37.754506,37.756109,37.757705,37.760601,37.764427]}]],[[{"lng":[-122.41672,-122.412348,-122.407754,-122.407602,-122.407449,-122.409253,-122.412047,-122.416413,-122.416567,-122.41672],"lat":[37.757167,37.75743,37.757705,37.756109,37.754506,37.754398,37.75423,37.753968,37.755568,37.757167]}]],[[{"lng":[-122.416413,-122.412047,-122.4119,-122.411746,-122.411488,-122.413673,-122.41587,-122.416108,-122.416413],"lat":[37.753968,37.75423,37.752629,37.751034,37.748344,37.748297,37.748267,37.750771,37.753968]}]],[[{"lng":[-122.412047,-122.409253,-122.408978,-122.40871,-122.410551,-122.411488,-122.411746,-122.4119,-122.412047],"lat":[37.75423,37.754398,37.751201,37.748397,37.748367,37.748344,37.751034,37.752629,37.75423]}]],[[{"lng":[-122.409253,-122.407449,-122.406453,-122.403127,-122.403007,-122.403784,-122.405239,-122.406788,-122.40871,-122.408978,-122.409253],"lat":[37.754398,37.754506,37.754277,37.754478,37.752446,37.749433,37.749125,37.748805,37.748397,37.751201,37.754398]}]],[[{"lng":[-122.406853,-122.399705,-122.395945,-122.396515,-122.398488,-122.400022,-122.399423,-122.399867,-122.402105,-122.402752,-122.404657,-122.40506,-122.406295,-122.406551,-122.406853],"lat":[37.73805,37.73992,37.737784,37.737156,37.733808,37.732311,37.731703,37.730292,37.72779,37.729357,37.732949,37.733652,37.735307,37.735721,37.73805]}]],[[{"lng":[-122.400022,-122.398488,-122.396515,-122.392776,-122.393904,-122.391617,-122.391891,-122.392699,-122.396409,-122.399867,-122.399423,-122.400022],"lat":[37.732311,37.733808,37.737156,37.735038,37.733788,37.732487,37.731684,37.729284,37.729801,37.730292,37.731703,37.732311]}]],[[{"lng":[-122.387399,-122.38717,-122.388529,-122.385282,-122.381558,-122.384047,-122.382919,-122.380186,-122.381284,-122.379932,-122.379779,-122.381439,-122.38209,-122.383431,-122.385287,-122.389035,-122.387399],"lat":[37.734738,37.735698,37.736475,37.74024,37.738123,37.737514,37.734511,37.73266,37.734034,37.733381,37.730799,37.730582,37.730635,37.729738,37.730791,37.732919,37.734738]}]],[[{"lng":[-122.384047,-122.381558,-122.380887,-122.37911,-122.376847,-122.373364367386,-122.36758,-122.369101,-122.373572,-122.373633,-122.370762,-122.367345,-122.367554,-122.369648,-122.371996,-122.372923,-122.373537,-122.374328,-122.375979,-122.376458,-122.376018,-122.375563,-122.374951,-122.374905,-122.375168,-122.374715,-122.374887291028,-122.374982,-122.374609,-122.373327,-122.372024,-122.370077,-122.369771405992,-122.37364,-122.377681,-122.378278,-122.379697,-122.383431,-122.38209,-122.381439,-122.379779,-122.379932,-122.381284,-122.380186,-122.382919,-122.384047],"lat":[37.737514,37.738123,37.738828,37.73768,37.740192,37.7457632793685,37.740214,37.739593,37.739415,37.738854,37.738181,37.738479,37.7381,37.737684,37.737043,37.737043,37.737176,37.73779,37.738477,37.738252,37.737991,37.737987,37.737563,37.737092,37.736504,37.735088,37.7342968808983,37.733862,37.73288,37.733186,37.734213,37.732788,37.7326989677807,37.728106,37.72863,37.728964,37.727618,37.729738,37.730635,37.730582,37.730799,37.733381,37.734034,37.73266,37.734511,37.737514]}]],[[{"lng":[-122.392699,-122.391891,-122.391617,-122.39106,-122.389035,-122.385287,-122.383431,-122.379697,-122.377833,-122.382176,-122.385024,-122.386899,-122.384517,-122.38521,-122.38793,-122.389801,-122.393284,-122.392699],"lat":[37.729284,37.731684,37.732487,37.734066,37.732919,37.730791,37.729738,37.727618,37.72656,37.722246,37.724098,37.724289,37.722862,37.723088,37.724729,37.725791,37.727764,37.729284]}]],[[{"lng":[-122.402105,-122.399867,-122.396409,-122.392699,-122.393284,-122.396682,-122.397025,-122.398515,-122.398863,-122.400785,-122.401183,-122.402105],"lat":[37.72779,37.730292,37.729801,37.729284,37.727764,37.720266,37.719515,37.717664,37.718683,37.723616,37.724588,37.72779]}]],[[{"lng":[-122.396682,-122.393284,-122.389801,-122.38793,-122.38521,-122.384517,-122.38282,-122.386265,-122.387912,-122.391141,-122.393545,-122.396682],"lat":[37.720266,37.727764,37.725791,37.724729,37.723088,37.722862,37.721872,37.717179,37.716328,37.71804,37.718487,37.720266]}]],[[{"lng":[-122.410662,-122.409378,-122.410441,-122.410619,-122.410551,-122.40871,-122.406788,-122.405239,-122.403784,-122.403569,-122.403561,-122.40459,-122.406952,-122.406853,-122.408217,-122.410377,-122.410507,-122.410365,-122.410662],"lat":[37.742424,37.742923,37.744918,37.746941,37.748367,37.748397,37.748805,37.749125,37.749433,37.749475,37.747762,37.744126,37.740495,37.73805,37.737612,37.736894,37.739779,37.741362,37.742424]}]],[[{"lng":[-122.418206,-122.41587,-122.413673,-122.411488,-122.410551,-122.410619,-122.410441,-122.409378,-122.410662,-122.410365,-122.410507,-122.411992,-122.413938,-122.415184,-122.415899,-122.418584,-122.41777,-122.418752,-122.416103,-122.41764,-122.417819,-122.419121,-122.418206],"lat":[37.748203,37.748267,37.748297,37.748344,37.748367,37.746941,37.744918,37.742923,37.742424,37.741362,37.739779,37.73971,37.73893,37.738933,37.739002,37.739297,37.740564,37.74118,37.745253,37.745882,37.745965,37.746775,37.748203]}]],[[{"lng":[-122.424258,-122.423361,-122.422709,-122.422276,-122.421704,-122.421393,-122.420282,-122.418206,-122.419121,-122.417819,-122.41764,-122.416103,-122.418752,-122.41777,-122.418584,-122.421742,-122.422841,-122.42427,-122.424258],"lat":[37.739935,37.74229,37.74394,37.745348,37.746412,37.748124,37.748161,37.748203,37.746775,37.745965,37.745882,37.745253,37.74118,37.740564,37.739297,37.740581,37.741027,37.739866,37.739935]}]],[[{"lng":[-122.425286,-122.424724,-122.42427,-122.422841,-122.421742,-122.418584,-122.415899,-122.415955,-122.41676,-122.420163,-122.420224,-122.422121,-122.422688,-122.426722,-122.425286],"lat":[37.73764,37.738431,37.739866,37.741027,37.740581,37.739297,37.739002,37.737211,37.735605,37.735777,37.735058,37.735153,37.735205,37.736372,37.73764]}]],[[{"lng":[-122.41676,-122.415955,-122.415899,-122.415184,-122.413938,-122.411992,-122.410507,-122.410377,-122.408217,-122.408779,-122.409798,-122.410513,-122.412447,-122.414313,-122.416824,-122.41676],"lat":[37.735605,37.737211,37.739002,37.738933,37.73893,37.73971,37.739779,37.736894,37.737612,37.735817,37.734828,37.734592,37.73467,37.734763,37.734888,37.735605]}]],[[{"lng":[-122.426722,-122.422688,-122.422121,-122.420224,-122.420163,-122.41676,-122.416824,-122.414313,-122.412447,-122.410513,-122.409798,-122.414388,-122.415939,-122.421703,-122.423982,-122.426104,-122.428038,-122.426158,-122.428573,-122.426722],"lat":[37.736372,37.735205,37.735153,37.735058,37.735777,37.735605,37.734888,37.734763,37.73467,37.734592,37.734828,37.732417,37.732072,37.731807,37.731552,37.731651,37.732016,37.733971,37.735082,37.736372]}]],[[{"lng":[-122.446308,-122.443664,-122.443075,-122.439483,-122.439558,-122.435892,-122.432422,-122.429391,-122.428573,-122.426158,-122.428038,-122.429304,-122.430165,-122.430199,-122.431294,-122.431801,-122.432447,-122.433982,-122.434798,-122.435385,-122.43702,-122.438787,-122.444486,-122.44473,-122.447878,-122.446308],"lat":[37.725978,37.728304,37.728634,37.730112,37.730191,37.731577,37.733176,37.734636,37.735082,37.733971,37.732016,37.730742,37.729851,37.729872,37.72873,37.728249,37.727637,37.725762,37.724683,37.723909,37.723723,37.723637,37.722974,37.722987,37.723005,37.725978]}]],[[{"lng":[-122.431294,-122.430199,-122.430165,-122.429304,-122.428038,-122.426104,-122.423982,-122.421703,-122.419514,-122.419486,-122.416637,-122.415193,-122.41446,-122.418404,-122.418888,-122.419875,-122.421847,-122.423336,-122.424199,-122.43039,-122.431294],"lat":[37.72873,37.729872,37.729851,37.730742,37.732016,37.731651,37.731552,37.731807,37.73149,37.729046,37.728809,37.729223,37.727457,37.726425,37.724966,37.72471,37.724199,37.727792,37.728736,37.728547,37.72873]}]],[[{"lng":[-122.421703,-122.415939,-122.414388,-122.409798,-122.408779,-122.408217,-122.406853,-122.406551,-122.406295,-122.405608,-122.408509,-122.40756,-122.410519,-122.41446,-122.415193,-122.416637,-122.419486,-122.419514,-122.421703],"lat":[37.731807,37.732072,37.732417,37.734828,37.735817,37.737612,37.73805,37.735721,37.735307,37.732439,37.731552,37.729261,37.728488,37.727457,37.729223,37.728809,37.729046,37.73149,37.731807]}]],[[{"lng":[-122.410519,-122.40756,-122.408509,-122.405608,-122.406295,-122.40506,-122.404657,-122.402752,-122.402105,-122.401183,-122.400785,-122.402061,-122.4026,-122.405572,-122.408529,-122.410519],"lat":[37.728488,37.729261,37.731552,37.732439,37.735307,37.733652,37.732949,37.729357,37.72779,37.724588,37.723616,37.72392,37.725235,37.724461,37.723688,37.728488]}]],[[{"lng":[-122.405572,-122.4026,-122.402061,-122.400785,-122.398863,-122.399615,-122.400435,-122.402103,-122.403078,-122.404059,-122.405572],"lat":[37.724461,37.725235,37.72392,37.723616,37.718683,37.716278,37.719162,37.718721,37.72113,37.720864,37.724461]}]],[[{"lng":[-122.418888,-122.418404,-122.41446,-122.410519,-122.408529,-122.405572,-122.404059,-122.403078,-122.402103,-122.403071,-122.4061,-122.406665,-122.407036,-122.41099,-122.412468,-122.412969,-122.416632,-122.419875,-122.418888],"lat":[37.724966,37.726425,37.727457,37.728488,37.723688,37.724461,37.720864,37.72113,37.718721,37.718465,37.719837,37.719215,37.720085,37.719074,37.722658,37.723858,37.722157,37.72471,37.724966]}]],[[{"lng":[-122.435385,-122.434798,-122.433982,-122.432447,-122.426912,-122.428074,-122.428837,-122.430026,-122.431205,-122.432385,-122.433985,-122.43715,-122.435385],"lat":[37.723909,37.724683,37.725762,37.727637,37.725151,37.723591,37.723955,37.722398,37.720856,37.719298,37.720063,37.721575,37.723909]}]],[[{"lng":[-122.431801,-122.431294,-122.43039,-122.424199,-122.423336,-122.421847,-122.423807,-122.424848,-122.428074,-122.426912,-122.432447,-122.431801],"lat":[37.728249,37.72873,37.728547,37.728736,37.727792,37.724199,37.723691,37.722053,37.723591,37.725151,37.727637,37.728249]}]],[[{"lng":[-122.432385,-122.431205,-122.430026,-122.428837,-122.428074,-122.424848,-122.424555,-122.42639,-122.42667,-122.427189,-122.431154,-122.433555,-122.432385],"lat":[37.719298,37.720856,37.722398,37.723955,37.723591,37.722053,37.721648,37.718561,37.717078,37.715748,37.716594,37.717744,37.719298]}]],[[{"lng":[-122.440141,-122.437502,-122.43715,-122.433985,-122.432385,-122.433555,-122.431154,-122.432777,-122.437046,-122.440418,-122.440141],"lat":[37.717692,37.721176,37.721575,37.720063,37.719298,37.717744,37.716594,37.714437,37.716008,37.717245,37.717692]}]],[[{"lng":[-122.448582,-122.447878,-122.44473,-122.444486,-122.438787,-122.43702,-122.435385,-122.43715,-122.437502,-122.440141,-122.440418,-122.440999,-122.441896,-122.443706,-122.446945,-122.44796,-122.448727,-122.448582],"lat":[37.718411,37.723005,37.722987,37.722974,37.723637,37.723723,37.723909,37.721575,37.721176,37.717692,37.717245,37.716488,37.715308,37.713592,37.716187,37.717,37.71745,37.718411]}]],[[{"lng":[-122.465994,-122.460858,-122.457312,-122.456376,-122.455619,-122.455028,-122.452884,-122.449627,-122.448727,-122.44796,-122.446945,-122.443706,-122.444571,-122.446091,-122.449309,-122.452665,-122.453983,-122.458509,-122.45905,-122.468939,-122.465994],"lat":[37.710154,37.710586,37.710648,37.710825,37.711066,37.711317,37.712829,37.715739,37.71745,37.717,37.716187,37.713592,37.712783,37.711592,37.709946,37.708806,37.708232,37.708231,37.708231,37.708232,37.710154]}]],[[{"lng":[-122.444571,-122.443706,-122.441896,-122.440999,-122.440418,-122.437046,-122.432777,-122.433682,-122.436144,-122.436203,-122.436642,-122.437811,-122.443335,-122.444097,-122.444571],"lat":[37.712783,37.713592,37.715308,37.716488,37.717245,37.716008,37.714437,37.713181,37.714254,37.714171,37.71355,37.71192,37.710555,37.711689,37.712783]}]],[[{"lng":[-122.437811,-122.436642,-122.436203,-122.436144,-122.433682,-122.432777,-122.431154,-122.427189,-122.425548,-122.426737,-122.424478,-122.423377,-122.42389,-122.428082,-122.430027,-122.431341,-122.433382,-122.433982,-122.435382,-122.436771,-122.436993,-122.43837,-122.437811],"lat":[37.71192,37.71355,37.714171,37.714254,37.713181,37.714437,37.716594,37.715748,37.714517,37.711225,37.71011,37.709203,37.708236,37.708431,37.708281,37.708231,37.708232,37.708132,37.708132,37.710156,37.710746,37.711133,37.71192]}]],[[{"lng":[-122.452665,-122.449309,-122.446091,-122.444571,-122.444097,-122.443335,-122.437811,-122.43837,-122.436993,-122.436771,-122.435382,-122.440781,-122.442079,-122.444482,-122.445875,-122.449751,-122.452183,-122.453983,-122.452665],"lat":[37.708806,37.709946,37.711592,37.712783,37.711689,37.710555,37.71192,37.711133,37.710746,37.710156,37.708132,37.708304,37.708231,37.708332,37.708233,37.708213,37.708132,37.708232,37.708806]}]],[[{"lng":[-122.415635,-122.415291,-122.415396,-122.411316,-122.410433,-122.406189,-122.407927,-122.410641,-122.411349,-122.412887,-122.416,-122.415635],"lat":[37.712733,37.713504,37.715474,37.714367,37.716414,37.715244,37.711448,37.712206,37.710605,37.711036,37.711915,37.712733]}]],[[{"lng":[-122.408171,-122.407272,-122.406665,-122.4061,-122.403071,-122.402103,-122.400435,-122.399615,-122.398863,-122.398515,-122.398328,-122.400626,-122.402546,-122.402154,-122.402188,-122.403617,-122.406189,-122.410433,-122.408171],"lat":[37.716157,37.71782,37.719215,37.719837,37.718465,37.718721,37.719162,37.716278,37.718683,37.717664,37.716259,37.712984,37.712266,37.712656,37.714731,37.714327,37.715244,37.716414,37.716157]}]],[[{"lng":[-122.411349,-122.410641,-122.407927,-122.406189,-122.403617,-122.402188,-122.402154,-122.402546,-122.40328,-122.404348,-122.404821,-122.405582,-122.406481,-122.41238,-122.411349],"lat":[37.710605,37.712206,37.711448,37.715244,37.714327,37.714731,37.712656,37.712266,37.711818,37.710443,37.709747,37.708231,37.708323,37.708303,37.710605]}]],[[{"lng":[-122.420083,-122.419348,-122.416648,-122.416,-122.412887,-122.411349,-122.41238,-122.415182,-122.416222,-122.420082,-122.420083],"lat":[37.708311,37.709947,37.710473,37.711915,37.711036,37.710605,37.708303,37.708231,37.708328,37.708231,37.708311]}]],[[{"lng":[-122.464297,-122.46109,-122.458405,-122.45779,-122.456913,-122.452947,-122.452758,-122.452569,-122.455466,-122.457536,-122.460843,-122.462972,-122.463226,-122.463782,-122.463912,-122.464042,-122.464172,-122.464297],"lat":[37.765963,37.766132,37.76616,37.766015,37.765874,37.766374,37.765443,37.764509,37.764161,37.763566,37.762628,37.762316,37.758562,37.758538,37.760405,37.762269,37.764131,37.765963]}]],[[{"lng":[-122.463782,-122.463226,-122.462972,-122.460843,-122.457536,-122.455466,-122.452569,-122.4524,-122.451958,-122.44912,-122.446783,-122.446394,-122.446395,-122.447915,-122.448535,-122.451367,-122.453872,-122.454078,-122.454172,-122.455537,-122.456091,-122.46147,-122.462108,-122.463361,-122.46377,-122.463658,-122.463782],"lat":[37.758538,37.758562,37.762316,37.762628,37.763566,37.764161,37.764509,37.763669,37.761478,37.761648,37.761781,37.760907,37.758856,37.757589,37.759045,37.758576,37.757378,37.757027,37.756507,37.753774,37.751804,37.751522,37.751972,37.753039,37.754142,37.756764,37.758538]}]],[[{"lng":[-122.477261,-122.47073,-122.470595,-122.470468,-122.470335,-122.476762,-122.476974,-122.477116,-122.477261],"lat":[37.765486,37.765779,37.763849,37.762027,37.760124,37.759837,37.761701,37.763562,37.765486]}]],[[{"lng":[-122.47073,-122.464297,-122.464172,-122.464042,-122.463912,-122.469263,-122.470335,-122.470468,-122.470595,-122.47073],"lat":[37.765779,37.765963,37.764131,37.762269,37.760405,37.760171,37.760124,37.762027,37.763849,37.765779]}]],[[{"lng":[-122.476762,-122.470335,-122.469263,-122.463912,-122.463782,-122.463658,-122.46377,-122.463361,-122.463433,-122.464462,-122.4666,-122.468335,-122.469782,-122.47068,-122.470908,-122.47121,-122.473683,-122.4766,-122.476699,-122.47687,-122.476762],"lat":[37.759837,37.760124,37.760171,37.760405,37.758538,37.756764,37.754142,37.753039,37.753028,37.752899,37.752803,37.753244,37.752675,37.752376,37.752616,37.755194,37.756237,37.756108,37.757968,37.759832,37.759837]}]],[[{"lng":[-122.4766,-122.473683,-122.47121,-122.470908,-122.47068,-122.469782,-122.468335,-122.4666,-122.464462,-122.463442,-122.464186,-122.465403,-122.466469,-122.466336,-122.469549,-122.470595,-122.476131,-122.476221,-122.476355,-122.4766],"lat":[37.756108,37.756237,37.755194,37.752616,37.752376,37.752675,37.753244,37.752803,37.752899,37.751195,37.750717,37.750986,37.750939,37.749075,37.748903,37.748869,37.748643,37.750509,37.752376,37.756108]}]],[[{"lng":[-122.476131,-122.470595,-122.469549,-122.466336,-122.466469,-122.465403,-122.464186,-122.463442,-122.464462,-122.463433,-122.463361,-122.462108,-122.46147,-122.458635,-122.459174,-122.461381,-122.463686,-122.465807,-122.466043,-122.470235,-122.475726,-122.475854,-122.476007,-122.476131],"lat":[37.748643,37.748869,37.748903,37.749075,37.750939,37.750986,37.750717,37.751195,37.752899,37.753028,37.753039,37.751972,37.751522,37.747769,37.747286,37.745569,37.743749,37.743574,37.743555,37.743289,37.743049,37.744915,37.746778,37.748643]}]],[[{"lng":[-122.46147,-122.456091,-122.455537,-122.454172,-122.454078,-122.453872,-122.451367,-122.448535,-122.447915,-122.44695,-122.445348,-122.447943,-122.446734,-122.44709,-122.449423,-122.445697,-122.447794,-122.449447,-122.45084,-122.451692,-122.453897,-122.453959,-122.454275,-122.454637,-122.458743,-122.459174,-122.458635,-122.46147],"lat":[37.751522,37.751804,37.753774,37.756507,37.757027,37.757378,37.758576,37.759045,37.757589,37.75655,37.755619,37.755532,37.753573,37.751505,37.750412,37.748855,37.749408,37.74666,37.745904,37.745629,37.745742,37.745765,37.745908,37.746142,37.746876,37.747286,37.747769,37.751522]}]],[[{"lng":[-122.461381,-122.459174,-122.458743,-122.454637,-122.454275,-122.453959,-122.453897,-122.451692,-122.449861,-122.452306,-122.453773,-122.454238,-122.45464,-122.454932,-122.458082,-122.458849,-122.459184,-122.459386,-122.45984,-122.461234,-122.463686,-122.461381],"lat":[37.745569,37.747286,37.746876,37.746142,37.745908,37.745765,37.745742,37.745629,37.743265,37.742819,37.743219,37.742835,37.742326,37.741777,37.739099,37.739158,37.739871,37.740571,37.741061,37.742584,37.743749,37.745569]}]],[[{"lng":[-122.459592,-122.458849,-122.458082,-122.454932,-122.45464,-122.454238,-122.453773,-122.452306,-122.449861,-122.446658,-122.445987,-122.442525,-122.442489,-122.443649,-122.443829,-122.439915,-122.438392,-122.438816,-122.439752,-122.448873,-122.453425,-122.456156,-122.459505,-122.460282,-122.459592],"lat":[37.738533,37.739158,37.739099,37.741777,37.742326,37.742835,37.743219,37.742819,37.743265,37.742442,37.74118,37.739448,37.737289,37.736519,37.736134,37.734901,37.73442,37.733087,37.733111,37.733069,37.73304,37.734212,37.734291,37.737855,37.738533]}]],[[{"lng":[-122.475726,-122.470235,-122.466043,-122.465807,-122.463686,-122.461234,-122.45984,-122.459386,-122.459184,-122.458849,-122.459592,-122.460282,-122.459505,-122.461242,-122.463136,-122.462879,-122.466248,-122.471569,-122.475173,-122.475189,-122.47539,-122.475485,-122.475621,-122.475726],"lat":[37.743049,37.743289,37.743555,37.743574,37.743749,37.742584,37.741061,37.740571,37.739871,37.739158,37.738533,37.737855,37.734291,37.735782,37.737023,37.736212,37.734937,37.734707,37.734562,37.734762,37.73745,37.739321,37.741183,37.743049]}]],[[{"lng":[-122.475347,-122.474745,-122.474634,-122.474972,-122.475173,-122.471569,-122.466248,-122.462879,-122.463136,-122.461242,-122.459505,-122.459064,-122.461748,-122.461727,-122.460816,-122.462888,-122.462786,-122.462199,-122.462265,-122.466278,-122.46974,-122.471983,-122.47244,-122.472547,-122.472672,-122.474528,-122.47537,-122.475347],"lat":[37.720914,37.726936,37.728992,37.731072,37.734562,37.734707,37.734937,37.736212,37.737023,37.735782,37.734291,37.733645,37.730064,37.729993,37.728681,37.728056,37.725549,37.725293,37.721748,37.721668,37.721644,37.721661,37.721603,37.71964,37.717248,37.719065,37.720275,37.720914]}]],[[{"lng":[-122.462888,-122.460816,-122.461727,-122.461748,-122.459064,-122.459505,-122.456156,-122.453425,-122.453411,-122.453394,-122.454002,-122.452382,-122.452335,-122.453105,-122.458191,-122.459213,-122.461202,-122.462199,-122.462786,-122.462888],"lat":[37.728056,37.728681,37.729993,37.730064,37.733645,37.734291,37.734212,37.73304,37.731568,37.729837,37.729166,37.727868,37.72318,37.723206,37.724344,37.724563,37.724974,37.725293,37.725549,37.728056]}]],[[{"lng":[-122.453394,-122.453411,-122.453425,-122.448873,-122.439752,-122.438816,-122.438392,-122.435825,-122.433104,-122.432422,-122.435892,-122.439558,-122.439483,-122.443075,-122.443664,-122.446308,-122.447878,-122.449546,-122.451596,-122.452335,-122.452382,-122.454002,-122.453394],"lat":[37.729837,37.731568,37.73304,37.733069,37.733111,37.733087,37.73442,37.733924,37.733419,37.733176,37.731577,37.730191,37.730112,37.728634,37.728304,37.725978,37.723005,37.723011,37.723033,37.72318,37.727868,37.729166,37.729837]}]],[[{"lng":[-122.462265,-122.462199,-122.461202,-122.459213,-122.458191,-122.453105,-122.452335,-122.451596,-122.449546,-122.451646,-122.452506,-122.459198,-122.459187,-122.462245,-122.462265],"lat":[37.721748,37.725293,37.724974,37.724563,37.724344,37.723206,37.72318,37.723033,37.723011,37.719686,37.72003,37.720024,37.71823,37.71822,37.721748]}]],[[{"lng":[-122.459198,-122.452506,-122.451646,-122.449546,-122.447878,-122.448582,-122.450378,-122.451895,-122.453378,-122.454189,-122.456108,-122.459187,-122.459198],"lat":[37.720024,37.72003,37.719686,37.723011,37.723005,37.718411,37.716169,37.716978,37.717768,37.718239,37.718235,37.71823,37.720024]}]],[[{"lng":[-122.472547,-122.47244,-122.471983,-122.46974,-122.466278,-122.462265,-122.462245,-122.46264,-122.466235,-122.468036,-122.468017,-122.471457,-122.472672,-122.472547],"lat":[37.71964,37.721603,37.721661,37.721644,37.721668,37.721748,37.71822,37.717936,37.717912,37.7179,37.716103,37.716085,37.717248,37.71964]}]],[[{"lng":[-122.472672,-122.471457,-122.468017,-122.468036,-122.466235,-122.46264,-122.462615,-122.462602,-122.462569,-122.462556,-122.460858,-122.465994,-122.468939,-122.469236,-122.471319,-122.471215,-122.471355,-122.471472,-122.472672],"lat":[37.717248,37.716085,37.716103,37.7179,37.717912,37.717936,37.715619,37.714308,37.71315,37.711254,37.710586,37.710154,37.708232,37.708232,37.708305,37.708939,37.712802,37.713427,37.717248]}]],[[{"lng":[-122.46264,-122.462245,-122.459187,-122.456108,-122.454189,-122.453378,-122.451895,-122.450378,-122.448582,-122.448727,-122.449627,-122.452884,-122.455028,-122.455619,-122.456376,-122.457312,-122.460858,-122.462556,-122.462569,-122.462602,-122.462615,-122.46264],"lat":[37.717936,37.71822,37.71823,37.718235,37.718239,37.717768,37.716978,37.716169,37.718411,37.71745,37.715739,37.712829,37.711317,37.711066,37.710825,37.710648,37.710586,37.711254,37.71315,37.714308,37.715619,37.717936]}]],[[{"lng":[-122.484763,-122.477261,-122.477116,-122.476974,-122.476762,-122.47687,-122.484365,-122.484496,-122.484627,-122.484763],"lat":[37.76516,37.765486,37.763562,37.761701,37.759837,37.759832,37.759501,37.76137,37.763231,37.76516]}]],[[{"lng":[-122.486906,-122.484763,-122.484627,-122.484496,-122.484365,-122.47687,-122.476699,-122.4766,-122.479817,-122.484104,-122.486248,-122.486382,-122.486512,-122.48664,-122.48677,-122.486906],"lat":[37.765054,37.76516,37.763231,37.76137,37.759501,37.759832,37.757968,37.756108,37.755966,37.755777,37.755683,37.757542,37.759407,37.761276,37.763137,37.765054]}]],[[{"lng":[-122.496085,-122.493336,-122.486906,-122.48677,-122.48664,-122.486512,-122.486382,-122.486248,-122.492675,-122.495433,-122.495555,-122.495692,-122.495822,-122.49595,-122.496085],"lat":[37.764675,37.764774,37.765054,37.763137,37.761276,37.759407,37.757542,37.755683,37.755398,37.755273,37.757137,37.759001,37.760871,37.762732,37.764675]}]],[[{"lng":[-122.485727,-122.479296,-122.479426,-122.476221,-122.476131,-122.476007,-122.475854,-122.475726,-122.485335,-122.485466,-122.485597,-122.485727],"lat":[37.74822,37.748504,37.750368,37.750509,37.748643,37.746778,37.744915,37.743049,37.742626,37.744493,37.746354,37.74822]}]],[[{"lng":[-122.486248,-122.484104,-122.479817,-122.4766,-122.476355,-122.476221,-122.479426,-122.479296,-122.485727,-122.485857,-122.485988,-122.486118,-122.486248],"lat":[37.755683,37.755777,37.755966,37.756108,37.752376,37.750509,37.750368,37.748504,37.74822,37.750085,37.751951,37.753815,37.755683]}]],[[{"lng":[-122.495046,-122.485857,-122.485727,-122.485597,-122.485466,-122.485335,-122.494504,-122.494646,-122.494774,-122.494911,-122.495046],"lat":[37.749681,37.750085,37.74822,37.746354,37.744493,37.742626,37.742222,37.744089,37.745949,37.747814,37.749681]}]],[[{"lng":[-122.495433,-122.492675,-122.486248,-122.486118,-122.485988,-122.485857,-122.495046,-122.495165,-122.495307,-122.495433],"lat":[37.755273,37.755398,37.755683,37.753815,37.751951,37.750085,37.749681,37.751543,37.75341,37.755273]}]],[[{"lng":[-122.494504,-122.485335,-122.475726,-122.475621,-122.475485,-122.47539,-122.475189,-122.479547,-122.480254,-122.485518,-122.485855,-122.491294,-122.493784,-122.494126,-122.494253,-122.494404,-122.494504],"lat":[37.742222,37.742626,37.743049,37.741183,37.739321,37.73745,37.734762,37.734619,37.73459,37.734356,37.734182,37.734096,37.733989,37.736631,37.738493,37.740352,37.742222]}]],[[{"lng":[-122.499059,-122.496564,-122.493784,-122.491294,-122.485855,-122.485518,-122.480254,-122.479547,-122.475189,-122.475173,-122.474972,-122.480015,-122.48193,-122.485302,-122.486179,-122.486606,-122.48889,-122.492161,-122.493529,-122.497017,-122.499059],"lat":[37.731576,37.733856,37.733989,37.734096,37.734182,37.734356,37.73459,37.734619,37.734762,37.734562,37.731072,37.731148,37.731172,37.730946,37.730879,37.729555,37.730257,37.728836,37.729594,37.729303,37.731576]}]],[[{"lng":[-122.486179,-122.485302,-122.48193,-122.480015,-122.474972,-122.474634,-122.474745,-122.475347,-122.478988,-122.479631,-122.484909,-122.483811,-122.486606,-122.486179],"lat":[37.730879,37.730946,37.731172,37.731148,37.731072,37.728992,37.726936,37.720914,37.720844,37.719663,37.724032,37.727029,37.729555,37.730879]}]],[[{"lng":[-122.485124,-122.484832,-122.483143,-122.483205,-122.479981,-122.478674,-122.47724,-122.475818,-122.474383,-122.474528,-122.472672,-122.471472,-122.471355,-122.472269,-122.477016,-122.485275,-122.485124],"lat":[37.718575,37.718571,37.718517,37.717585,37.716942,37.717903,37.717709,37.716891,37.717507,37.719065,37.717248,37.713427,37.712802,37.712838,37.714593,37.714823,37.718575]}]],[[{"lng":[-122.484909,-122.479631,-122.478988,-122.475347,-122.47537,-122.474528,-122.474383,-122.475818,-122.47724,-122.478674,-122.479981,-122.483205,-122.483143,-122.484832,-122.485124,-122.484909],"lat":[37.724032,37.719663,37.720844,37.720914,37.720275,37.719065,37.717507,37.716891,37.717709,37.717903,37.716942,37.717585,37.718517,37.718571,37.718575,37.724032]}]],[[{"lng":[-122.502979,-122.496085,-122.49595,-122.495822,-122.495692,-122.495555,-122.495433,-122.495307,-122.495165,-122.499914,-122.499784,-122.50193,-122.50206,-122.502191,-122.502318,-122.50245,-122.502582,-122.502713,-122.502844,-122.502979],"lat":[37.764344,37.764675,37.762732,37.760871,37.759001,37.757137,37.755273,37.75341,37.751543,37.751334,37.749472,37.749377,37.75124,37.753106,37.754953,37.756835,37.7587,37.760567,37.762427,37.764344]}]],[[{"lng":[-122.512070759187,-122.50987,-122.509622,-122.509382,-122.509111,-122.502582,-122.50245,-122.502318,-122.502191,-122.50206,-122.50193,-122.505148,-122.507942,-122.509993368973,-122.512070759187],"lat":[37.7636593024008,37.76409,37.762127,37.760272,37.758414,37.7587,37.756835,37.754953,37.753106,37.75124,37.749377,37.749235,37.749111,37.7488149503326,37.7636593024008]}]],[[{"lng":[-122.50987,-122.502979,-122.502844,-122.502713,-122.502582,-122.509111,-122.509382,-122.509622,-122.50987],"lat":[37.76409,37.764344,37.762427,37.760567,37.7587,37.758414,37.760272,37.762127,37.76409]}]],[[{"lng":[-122.50193,-122.499784,-122.499914,-122.495165,-122.495046,-122.494911,-122.494774,-122.494646,-122.494504,-122.494404,-122.494253,-122.494126,-122.493784,-122.496564,-122.497487,-122.499139,-122.501041,-122.501142,-122.501404,-122.501536,-122.50193],"lat":[37.749377,37.749472,37.751334,37.751543,37.749681,37.747814,37.745949,37.744089,37.742222,37.740352,37.738493,37.736631,37.733989,37.733856,37.733925,37.734404,37.7353,37.738192,37.74191,37.743785,37.749377]}]],[[{"lng":[-122.509993368973,-122.507942,-122.505148,-122.50193,-122.501536,-122.501404,-122.501142,-122.501041,-122.502031,-122.505262,-122.506796,-122.508121548602,-122.509993368973],"lat":[37.7488149503326,37.749111,37.749235,37.749377,37.743785,37.74191,37.738192,37.7353,37.735515,37.735565,37.735559,37.7354395332202,37.7488149503326]}]],[[{"lng":[-122.464781,-122.463581,-122.459476,-122.459388,-122.459179,-122.458998,-122.458878,-122.461068,-122.464284,-122.464551,-122.464781],"lat":[37.788564,37.788806,37.789711,37.788405,37.785682,37.783189,37.781326,37.781227,37.781089,37.784792,37.788564]}]],[[{"lng":[-122.472583,-122.472352,-122.470912,-122.466924,-122.464781,-122.464551,-122.464284,-122.466432,-122.467492,-122.472264,-122.472379,-122.472516,-122.472583],"lat":[37.786317,37.787234,37.787221,37.788132,37.788564,37.784792,37.781089,37.78099,37.780938,37.780747,37.782581,37.784453,37.786317]}]],[[{"lng":[-122.48408,-122.478714,-122.478579,-122.478444,-122.47831,-122.479388,-122.482598,-122.483667,-122.483804,-122.483936,-122.48408],"lat":[37.785795,37.786031,37.784174,37.782308,37.780449,37.780403,37.780265,37.780207,37.782077,37.783931,37.785795]}]],[[{"lng":[-122.478714,-122.472583,-122.472516,-122.472379,-122.472264,-122.47831,-122.478444,-122.478579,-122.478714],"lat":[37.786031,37.786317,37.784453,37.782581,37.780747,37.780449,37.782308,37.784174,37.786031]}]],[[{"lng":[-122.493491,-122.490364,-122.487151,-122.487286,-122.48408,-122.483936,-122.483804,-122.483667,-122.489019,-122.491172,-122.493305,-122.493446,-122.493491],"lat":[37.783498,37.78364,37.783785,37.785649,37.785795,37.783931,37.782077,37.780207,37.779977,37.779869,37.779752,37.781627,37.783498]}]],[[{"lng":[-122.493338,-122.493048716406,-122.492883,-122.485827333182,-122.483985,-122.483799,-122.479845,-122.472352,-122.472583,-122.478714,-122.48408,-122.487286,-122.487151,-122.490364,-122.493491,-122.493338],"lat":[37.787403,37.7879341073778,37.787929,37.7906121409026,37.789769,37.78774,37.786796,37.787234,37.786317,37.786031,37.785795,37.785649,37.783785,37.78364,37.783498,37.787403]}]],[[{"lng":[-122.464284,-122.461068,-122.458878,-122.458505,-122.458371,-122.460553,-122.463749,-122.464011,-122.464284],"lat":[37.781089,37.781227,37.781326,37.77619,37.774309,37.774031,37.773624,37.777235,37.781089]}]],[[{"lng":[-122.472264,-122.467492,-122.466432,-122.464284,-122.464011,-122.463749,-122.465332,-122.466952,-122.471706,-122.471842,-122.471967,-122.472103,-122.472264],"lat":[37.780747,37.780938,37.78099,37.781089,37.777235,37.773624,37.773423,37.773351,37.773141,37.775006,37.776871,37.778731,37.780747]}]],[[{"lng":[-122.479388,-122.47831,-122.472264,-122.472103,-122.471967,-122.471842,-122.471706,-122.478837,-122.478972,-122.479106,-122.479241,-122.479388],"lat":[37.780403,37.780449,37.780747,37.778731,37.776871,37.775006,37.773141,37.772815,37.774681,37.776546,37.778408,37.780403]}]],[[{"lng":[-122.489019,-122.483667,-122.482598,-122.479388,-122.479241,-122.479106,-122.482318,-122.488741,-122.488876,-122.489019],"lat":[37.779977,37.780207,37.780265,37.780403,37.778408,37.776546,37.7764,37.776107,37.777972,37.779977]}]],[[{"lng":[-122.488741,-122.482318,-122.479106,-122.478972,-122.478837,-122.482049,-122.488471,-122.488606,-122.488741],"lat":[37.776107,37.7764,37.776546,37.774681,37.772815,37.772667,37.772374,37.774241,37.776107]}]],[[{"lng":[-122.497455,-122.493168,-122.493305,-122.491172,-122.489019,-122.488876,-122.488741,-122.488606,-122.488471,-122.494903,-122.495039,-122.495175,-122.497319,-122.497455],"lat":[37.777579,37.777774,37.779752,37.779869,37.779977,37.777972,37.776107,37.774241,37.772374,37.772081,37.773947,37.775813,37.775715,37.777579]}]],[[{"lng":[-122.498801,-122.493446,-122.493305,-122.493168,-122.497455,-122.497319,-122.495175,-122.495039,-122.494903,-122.498114,-122.49825,-122.498386,-122.498521,-122.498669,-122.498801],"lat":[37.781377,37.781627,37.779752,37.777774,37.777579,37.775715,37.775813,37.773947,37.772081,37.771934,37.7738,37.775667,37.777531,37.779507,37.781377]}]],[[{"lng":[-122.514483,-122.509842514521,-122.507484,-122.510781,-122.509879,-122.509593,-122.509228,-122.506022,-122.501733,-122.498521,-122.498386,-122.49825,-122.498114,-122.505611,-122.511124,-122.513067688424,-122.513128,-122.514483],"lat":[37.780829,37.7846145393916,37.782185,37.782059,37.780627,37.780173,37.777044,37.77719,37.777385,37.777531,37.775667,37.7738,37.771934,37.771592,37.771323,37.7707830331714,37.771214,37.780829]}]],[[{"lng":[-122.509879,-122.506509,-122.505226,-122.502012,-122.498801,-122.498669,-122.498521,-122.501733,-122.506022,-122.509228,-122.509593,-122.509879],"lat":[37.780627,37.780857,37.781077,37.781227,37.781377,37.779507,37.777531,37.777385,37.77719,37.777044,37.780173,37.780627]}]],[[{"lng":[-122.485783,-122.480296,-122.478083,-122.470336,-122.463793,-122.448241130218,-122.448241,-122.44946,-122.44782,-122.447466,-122.447303,-122.447015,-122.446728,-122.4463,-122.45274,-122.459476,-122.463581,-122.464781,-122.466924,-122.470912,-122.472352,-122.479845,-122.483799,-122.483985,-122.485827333182,-122.485783],"lat":[37.790629,37.801293,37.810828,37.808671,37.804653,37.8072521685356,37.804478,37.802454,37.801633,37.79939,37.798459,37.796579,37.794706,37.791879,37.790974,37.789711,37.788806,37.788564,37.788132,37.787221,37.787234,37.786796,37.78774,37.789769,37.7906121409026,37.790629]}]],[[{"lng":[-122.506796,-122.505262,-122.502031,-122.501041,-122.499139,-122.497487,-122.496564,-122.499059,-122.497017,-122.493529,-122.492161,-122.48889,-122.486606,-122.483811,-122.484909,-122.485124,-122.485275,-122.477016,-122.472269,-122.471355,-122.471215,-122.471319,-122.486042,-122.502426880724,-122.506483,-122.508121548602,-122.506796],"lat":[37.735559,37.735565,37.735515,37.7353,37.734404,37.733925,37.733856,37.731576,37.729303,37.729594,37.728836,37.730257,37.729555,37.727029,37.724032,37.718575,37.714823,37.714593,37.712838,37.712802,37.708939,37.708305,37.708233,37.7081329861058,37.723731,37.7354395332202,37.735559]}]],[[{"lng":[-122.424478,-122.422811,-122.422241,-122.421475,-122.420908,-122.415635,-122.416,-122.416648,-122.419348,-122.420083,-122.420082,-122.42389,-122.423377,-122.424478],"lat":[37.71011,37.710511,37.711317,37.712201,37.713774,37.712733,37.711915,37.710473,37.709947,37.708311,37.708231,37.708236,37.709203,37.71011]}]],[[{"lng":[-122.40499,-122.404391,-122.401657,-122.399433,-122.400982,-122.398755,-122.397206,-122.39275,-122.390527,-122.389168,-122.388423,-122.387421,-122.383505768697,-122.381232,-122.380462169293,-122.385823,-122.387917,-122.388164,-122.393968,-122.393848,-122.398703,-122.404497,-122.40499],"lat":[37.769715,37.76975,37.771817,37.773573,37.774809,37.776568,37.775332,37.778852,37.780607,37.781649,37.781798,37.78279,37.7830792797042,37.773514,37.7675163525902,37.767255,37.764379,37.766958,37.766614,37.765307,37.765014,37.764664,37.769715]}]],[[{"lng":[-122.405582,-122.404821,-122.404348,-122.40328,-122.402546,-122.400626,-122.398328,-122.398515,-122.397025,-122.396682,-122.393545,-122.391141,-122.387912,-122.386265,-122.38282,-122.383251,-122.374980911262,-122.377251,-122.379568,-122.37529,-122.390674,-122.39137503156,-122.393635,-122.395107,-122.404682,-122.405292,-122.405582],"lat":[37.708231,37.709747,37.710443,37.711818,37.712266,37.712984,37.716259,37.717664,37.719515,37.720266,37.718487,37.71804,37.716328,37.717179,37.721872,37.720151,37.7155576312199,37.714557,37.711491,37.708482,37.70864,37.708331,37.708244,37.708339,37.708331,37.708262,37.708231]}]],[[{"lng":[-122.408431,-122.405513,-122.402041,-122.400681,-122.400149,-122.404613,-122.404796,-122.407903,-122.408253,-122.408431],"lat":[37.796704,37.797065,37.797505,37.796777,37.794134,37.793565,37.794453,37.79406,37.795824,37.796704]}]],[[{"lng":[-122.395945,-122.394093,-122.391268,-122.389425,-122.387705,-122.387143,-122.385282,-122.388529,-122.38717,-122.387399,-122.389035,-122.39106,-122.391617,-122.393904,-122.392776,-122.396515,-122.395945],"lat":[37.737784,37.736729,37.739853,37.738803,37.742697,37.741297,37.74024,37.736475,37.735698,37.734738,37.732919,37.734066,37.732487,37.733788,37.735038,37.737156,37.737784]}]],[[{"lng":[-122.403784,-122.403007,-122.403127,-122.403396,-122.403689,-122.39797,-122.398216,-122.392494,-122.392447,-122.391232,-122.390683,-122.394587,-122.396408,-122.402605,-122.403569,-122.403784],"lat":[37.749433,37.752446,37.754478,37.756471,37.757015,37.757359,37.759909,37.760245,37.756436,37.752657,37.752663,37.752427,37.751179,37.750558,37.749475,37.749433]}]],[[{"lng":[-122.406062,-122.399195,-122.396315,-122.394748,-122.393181,-122.391487,-122.389708268207,-122.388570312096,-122.385683559327,-122.385323,-122.383505768697,-122.387421,-122.388423,-122.389168,-122.390527,-122.39275,-122.397386,-122.39893,-122.400468,-122.402708,-122.406062],"lat":[37.785757,37.791058,37.793337,37.79448,37.79323,37.793859,37.7958244063587,37.7945008753206,37.7911433584052,37.790724,37.7830792797042,37.78279,37.781798,37.781649,37.780607,37.778852,37.782554,37.783785,37.78503,37.783259,37.785757]}]],[[{"lng":[-122.510781,-122.507484,-122.509842514521,-122.50531,-122.493048716406,-122.493338,-122.493491,-122.493446,-122.498801,-122.502012,-122.505226,-122.506509,-122.509879,-122.510781],"lat":[37.782059,37.782185,37.7846145393916,37.788312,37.7879341073778,37.787403,37.783498,37.781627,37.781377,37.781227,37.781077,37.780857,37.780627,37.782059]}]],[[{"lng":[-122.513067688424,-122.511124,-122.505611,-122.498114,-122.494903,-122.488471,-122.482049,-122.478837,-122.471706,-122.466952,-122.465332,-122.463749,-122.460553,-122.458371,-122.454683,-122.454083,-122.450787,-122.447497,-122.444216,-122.440922,-122.440735,-122.445717,-122.4506,-122.453902,-122.453352,-122.452947,-122.456913,-122.45779,-122.458405,-122.46109,-122.464297,-122.47073,-122.477261,-122.484763,-122.486906,-122.493336,-122.496085,-122.502979,-122.50987,-122.512070759187,-122.513067688424],"lat":[37.7707830331714,37.771323,37.771592,37.771934,37.772081,37.772374,37.772667,37.772815,37.773141,37.773351,37.773423,37.773624,37.774031,37.774309,37.774755,37.77193,37.772368,37.77279,37.773212,37.773635,37.772713,37.77208,37.77146,37.771054,37.768246,37.766374,37.765874,37.766015,37.76616,37.766132,37.765963,37.765779,37.765486,37.76516,37.765054,37.764774,37.764675,37.764344,37.76409,37.7636593024008,37.7707830331714]}]],[[{"lng":[-122.42667,-122.42639,-122.424555,-122.424848,-122.423807,-122.421847,-122.419875,-122.416632,-122.412969,-122.412468,-122.41099,-122.407036,-122.406665,-122.407272,-122.408171,-122.410433,-122.411316,-122.415396,-122.415291,-122.415635,-122.420908,-122.421475,-122.422241,-122.422811,-122.424478,-122.426737,-122.425548,-122.427189,-122.42667],"lat":[37.717078,37.718561,37.721648,37.722053,37.723691,37.724199,37.72471,37.722157,37.723858,37.722658,37.719074,37.720085,37.719215,37.71782,37.716157,37.716414,37.714367,37.715474,37.713504,37.712733,37.713774,37.712201,37.711317,37.710511,37.71011,37.711225,37.714517,37.715748,37.717078]}]],[[{"lng":[-122.385024,-122.382176,-122.377833,-122.379697,-122.378278,-122.377681,-122.37364,-122.369771405992,-122.367386,-122.366161,-122.364671,-122.356784,-122.360238,-122.364084,-122.370411,-122.374980911262,-122.383251,-122.38282,-122.384517,-122.386899,-122.385024],"lat":[37.724098,37.722246,37.72656,37.727618,37.728964,37.72863,37.728106,37.7326989677807,37.732004,37.732844,37.731859,37.729505,37.719421,37.715701,37.717572,37.7155576312199,37.720151,37.721872,37.722862,37.724289,37.724098]}]],[[{"lng":[-122.406952,-122.40459,-122.403561,-122.403569,-122.402605,-122.396408,-122.394587,-122.390683,-122.381711,-122.380985,-122.378560351096,-122.376716,-122.376241,-122.373364367386,-122.376847,-122.37911,-122.380887,-122.381558,-122.385282,-122.387143,-122.387705,-122.389425,-122.391268,-122.394093,-122.395945,-122.399705,-122.406853,-122.406952],"lat":[37.740495,37.744126,37.747762,37.749475,37.750558,37.751179,37.752427,37.752663,37.753186,37.753228,37.7535517721854,37.752094,37.748523,37.7457632793685,37.740192,37.73768,37.738828,37.738123,37.74024,37.741297,37.742697,37.738803,37.739853,37.736729,37.737784,37.73992,37.73805,37.740495]}]]],null,"sf_poor5",{"interactive":true,"className":"","pane":"overlayPane01","stroke":true,"color":"#666666","weight":1,"opacity":1,"fill":true,"fillColor":["#FEE697","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FEBE4A","#FEBE4A","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEBE4A","#FEE697","#F88B22","#FEE697","#FEE697","#FEBE4A","#FEE697","#FEBE4A","#F88B22","#DB5D0A","#FEBE4A","#FEBE4A","#FEBE4A","#FEBE4A","#DB5D0A","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FEBE4A","#FEE697","#FEE697","#FEE697","#FFFACE","#FEE697","#FEE697","#FEBE4A","#FFFACE","#FEE697","#FFFACE","#FEE697","#FEE697","#FFFACE","#FEE697","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEBE4A","#FEE697","#F88B22","#FEBE4A","#A33803","#A33803","#A33803","#A33803","#FEBE4A","#FEBE4A","#FEE697","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEBE4A","#FEE697","#FEBE4A","#FEE697","#FEE697","#FEE697","#FEE697","#F88B22","#F88B22","#FEBE4A","#FEE697","#FEBE4A","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEE697","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FEE697","#FEE697","#FFFACE","#FFFACE","#FEE697","#FEE697","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FEE697","#FEE697","#FEE697","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#A33803","#FEBE4A","#F88B22","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FEE697","#FFFACE","#FEE697","#FFFACE","#FEE697","#FEE697","#FFFACE","#FFFACE","#FEE697","#FFFACE","#FFFACE","#FEE697","#FEE697","#FFFACE","#FEE697","#F88B22","#FFFACE","#FFFACE","#F88B22","#FEE697","#FEE697","#FFFACE","#FEE697","#FFFACE","#F88B22","#F88B22","#FEBE4A"],"fillOpacity":[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],"smoothFactor":1,"noClip":false},["<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>17.900<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.465<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.398<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.933<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.532<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>24.727<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>24.796<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>18.166<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075010900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.568<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.179<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.527<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.544<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.020<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>19.675<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>33.547<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011901<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.775<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075011902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.485<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.947<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.077<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012201<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.453<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012202<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>31.642<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012301<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>41.023<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012302<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>27.007<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012401<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>27.724<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012402<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>28.733<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012501<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>29.246<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012502<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>45.182<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012601<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>0.706<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012602<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.138<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.249<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.377<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012901<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>2.943<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075012902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.551<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.812<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013101<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.009<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013102<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.862<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.184<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.226<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.247<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075013500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.138<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.903<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.925<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.597<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.803<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.285<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>15.211<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.540<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>19.498<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.167<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075015900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.474<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.287<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>29.083<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.908<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.813<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.587<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.906<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.415<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.368<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.836<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.788<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075016900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.426<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.001<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017101<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.667<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017102<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.281<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017601<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>28.338<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>15.131<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>36.055<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>24.479<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>53.414<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>53.414<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>53.414<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075017902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>53.414<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075018000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>23.253<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>22.435<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.791<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.535<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020401<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.581<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020402<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.976<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>3.743<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.729<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.518<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.001<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075020900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>17.301<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.844<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>3.876<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.666<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>2.174<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>2.971<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.545<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.856<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.676<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075021800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>3.186<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>3.218<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022702<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>3.771<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022704<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.544<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.526<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>22.138<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022803<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.092<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022901<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.190<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>15.597<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075022903<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.168<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023001<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.326<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023003<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.468<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023102<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>33.261<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023103<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>31.061<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>24.301<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.500<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075023400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>25.963<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.779<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.442<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.960<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025401<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.270<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025402<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.981<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025403<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.476<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.976<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.631<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025701<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.822<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025702<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.724<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.773<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075025900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.047<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026001<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.269<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026002<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.871<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026003<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.808<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026004<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.545<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.632<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.765<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026301<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.843<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026302<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.942<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026303<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.360<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026401<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.000<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026402<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.692<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026403<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.850<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075026404<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>15.895<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030101<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.794<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030102<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.798<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030201<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.224<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030202<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.330<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030301<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.043<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030302<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.713<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.167<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.697<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>2.398<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.211<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>1.762<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075030900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.993<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.748<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.190<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031201<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.755<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031202<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.468<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031301<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.347<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031302<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>19.792<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075031400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.491<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032601<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.683<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032602<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.689<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.879<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.049<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.446<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032901<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.758<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075032902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.738<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075033000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.918<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075033100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.655<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075033201<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>54.433<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075033203<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>29.440<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075033204<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>36.321<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075035100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.396<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075035201<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.117<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075035202<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.763<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075035300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.479<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075035400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.518<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075040100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.306<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075040200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>6.569<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075042601<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>19.808<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075042602<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.693<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075042700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.946<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075042800<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.945<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075045100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>12.154<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075045200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>17.127<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>5.702<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047701<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>7.651<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047702<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.313<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047801<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.648<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047802<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.876<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047901<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>10.663<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075047902<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>13.348<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075060100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>4.026<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075060400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>19.434<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075060502<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>37.793<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075060700<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.542<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075061000<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>9.323<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075061100<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>32.309<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075061200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>14.923<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075061400<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>16.056<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075061500<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>8.423<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075980200<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>11.304<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075980300<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>0.000<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075980501<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>32.708<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075980600<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>39.496<\/td><\/tr><\/table><\/div>","<div style=\"max-height:10em;overflow:auto;\"><table>\n\t\t\t   <thead><tr><th colspan=\"2\"><b>06075980900<\/b><\/th><\/thead><\/tr><tr><td style=\"color: #888888;\">pct_below_pov<\/td><td>20.000<\/td><\/tr><\/table><\/div>"],{"maxWidth":500,"minWidth":100,"autoPan":true,"keepInView":false,"closeButton":true,"className":""},["06075010100","06075010200","06075010300","06075010400","06075010500","06075010600","06075010700","06075010800","06075010900","06075011000","06075011100","06075011200","06075011300","06075011700","06075011800","06075011901","06075011902","06075012000","06075012100","06075012201","06075012202","06075012301","06075012302","06075012401","06075012402","06075012501","06075012502","06075012601","06075012602","06075012700","06075012800","06075012901","06075012902","06075013000","06075013101","06075013102","06075013200","06075013300","06075013400","06075013500","06075015100","06075015200","06075015300","06075015400","06075015500","06075015600","06075015700","06075015801","06075015802","06075015900","06075016000","06075016100","06075016200","06075016300","06075016400","06075016500","06075016600","06075016700","06075016801","06075016802","06075016900","06075017000","06075017101","06075017102","06075017601","06075017700","06075017801","06075017802","06075017902","06075017902","06075017902","06075017902","06075018000","06075020100","06075020200","06075020300","06075020401","06075020402","06075020500","06075020600","06075020700","06075020800","06075020900","06075021000","06075021100","06075021200","06075021300","06075021400","06075021500","06075021600","06075021700","06075021800","06075022600","06075022702","06075022704","06075022801","06075022802","06075022803","06075022901","06075022902","06075022903","06075023001","06075023003","06075023102","06075023103","06075023200","06075023300","06075023400","06075025100","06075025200","06075025300","06075025401","06075025402","06075025403","06075025500","06075025600","06075025701","06075025702","06075025800","06075025900","06075026001","06075026002","06075026003","06075026004","06075026100","06075026200","06075026301","06075026302","06075026303","06075026401","06075026402","06075026403","06075026404","06075030101","06075030102","06075030201","06075030202","06075030301","06075030302","06075030400","06075030500","06075030600","06075030700","06075030800","06075030900","06075031000","06075031100","06075031201","06075031202","06075031301","06075031302","06075031400","06075032601","06075032602","06075032700","06075032801","06075032802","06075032901","06075032902","06075033000","06075033100","06075033201","06075033203","06075033204","06075035100","06075035201","06075035202","06075035300","06075035400","06075040100","06075040200","06075042601","06075042602","06075042700","06075042800","06075045100","06075045200","06075047600","06075047701","06075047702","06075047801","06075047802","06075047901","06075047902","06075060100","06075060400","06075060502","06075060700","06075061000","06075061100","06075061200","06075061400","06075061500","06075980200","06075980300","06075980501","06075980600","06075980900"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addLegend","args":[{"colors":["#FFFACE","#FEE697","#FEBE4A","#F88B22","#DB5D0A","#A33803"],"labels":["0 to 10","10 to 20","20 to 30","30 to 40","40 to 50","50 to 60"],"na_color":null,"na_label":null,"opacity":1,"position":"topright","type":"factor","title":"pct_below_pov","extra":null,"layerId":null,"className":"info legend","group":"sf_poor5"}]},{"method":"addLayersControl","args":[["Esri.WorldGrayCanvas","OpenStreetMap","Esri.WorldTopoMap"],"sf_poor5",{"collapsed":true,"autoZIndex":true,"position":"topleft"}]}],"limits":{"lat":[37.708132,37.8633422971737],"lng":[-122.514483,-122.327561619753]},"fitBounds":[37.708132,-122.514483,37.8633422971737,-122.327561619753,[]]},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## tmap

There are a number of great tutorials online for working with `tmap`.

See the `References` at the end of this workshop document.


## Types of Census Geographic Data

`Cartographic Boundary` data vs `Detailed TIGER/Line` data

By default, `tidycensus` downloads census `cartographic boundary` data.

These are simplifed geometries, clipped to coastlines. 

In `get_acs` you can also request the more detailed census TIGER data.

Let's check it out.

## Fetch Cartographic Boundary Data

```r
sf_poor_cb <- get_acs(geography = "tract",   
                   table = "C17002", 
                   summary_var = "C17002_001",
                   year = 2016,           
                   state="CA",
                   county="San Francisco",
                   geometry=TRUE,
                   cb = TRUE)     # THIS IS THE DEFAULT!
```

```
## Getting data from the 2012-2016 5-year ACS
```

## Fetch Detailed TIGER/Line Geometry

```r
sf_poor_tl <- get_acs(geography = "tract",   
                   table = "C17002",        
                   summary_var = "C17002_001",
                   year = 2016,              
                   state="CA",
                   county="San Francisco",
                   geometry=TRUE,
                   cb = FALSE)  # Fetching the TIGER/Line data  
```

```
## Getting data from the 2012-2016 5-year ACS
```

## Map it


```r
par(mfrow=c(1,2))
plot(sf_poor_cb[sf_poor$summary_est>0,]$geometry)
plot(sf_poor_tl[sf_poor$summary_est>0,]$geometry)
```

![](Rcensus_data_maps_files/figure-html/unnamed-chunk-66-1.png)<!-- -->

```r
par(mfrow=c(1,1))
```

## Visualize differences with Tmap


```r
tm_shape(sf_poor_tl) +
    tm_borders() +
tm_shape(sf_poor_cb) +
    tm_borders(col="red")
```

<!--html_preserve--><div id="htmlwidget-75011ae02a1bbcc0889f" style="width:672px;height:480px;" class="leaflet html-widget"></div>


## Challenge

TO BE ADDED....

# Questions?

## Scaling Up Example

Read in census variables of interest from a file


```r
# Load cenvar lookup table of vars of interest
my_cenvar_df <-read.csv("data/cenvar_lookup.csv", strip.white = T, stringsAsFactors = F)

my_cenvar_df
```

```
##              my_cen_var_names my_cen_vars
## 1          citizenship_totpop B05001_001E
## 2     citizenship_non_citizen B05001_006E
## 3                entry_totpop B05005_001E
## 4                  entry_2010 B05005_002E
## 5             entry_2000_2009 B05005_007E
## 6           birthplace_totpop B05007_001E
## 7            birthplace_europ B05007_014E
## 8            birthplace_asian B05007_027E
## 9     birthplace_latinAmerica B05007_040E
## 10    birthplace_southAmerica B05007_081E
## 11    birthplace_other_nonUSA B05007_094E
## 12    birthplace_byage_totpop B06001_001E
## 13     birthplace_byage_fborn B06001_049E
## 14             poverty_totpop B06012_001E
## 15                  below_pov B06012_002E
## 16                 below_pov2 B06012_003E
## 17       poverty_fborn_totpop B06012_017E
## 18            below_pov_fborn B06012_018E
## 19           below_pov2_fborn B06012_019E
## 20       health_native_totpop B27020_002E
## 21  health_native_noinsurance B27020_006E
## 22    health_fborn_nat_totpop B27020_008E
## 23 fborn_nohealth_naturalized B27020_012E
## 24 health_fborn_noncit_totpop B27020_013E
## 25  fborn_nohealth_noncitizen B27020_017E
```

## Fetch the ACS data

Fetch the ACS data for these variables for the 9 county bay area


```r
nine_counties <- c("001", "075", "013", "041", "055", "081", "085", "095", "097", "087")
bay9_data <-get_acs(geography = "tract", 
                       variables = my_cenvar_df$my_cen_vars, 
                       year=2016,
                       state = "CA", 
                       county = nine_counties, 
                       geometry = T)
```

```
## Getting data from the 2012-2016 5-year ACS
```

```r
bay9_data
```

```
## Simple feature collection with 41025 features and 5 fields (with 175 geometries empty)
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -123.5335 ymin: 36.85065 xmax: -121.2082 ymax: 38.86424
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## First 10 features:
##          GEOID                                          NAME   variable
## 1  06001400100 Census Tract 4001, Alameda County, California B05001_001
## 2  06001400100 Census Tract 4001, Alameda County, California B05001_006
## 3  06001400100 Census Tract 4001, Alameda County, California B05005_001
## 4  06001400100 Census Tract 4001, Alameda County, California B05005_002
## 5  06001400100 Census Tract 4001, Alameda County, California B05005_007
## 6  06001400100 Census Tract 4001, Alameda County, California B05007_001
## 7  06001400100 Census Tract 4001, Alameda County, California B05007_014
## 8  06001400100 Census Tract 4001, Alameda County, California B05007_027
## 9  06001400100 Census Tract 4001, Alameda County, California B05007_040
## 10 06001400100 Census Tract 4001, Alameda County, California B05007_081
##    estimate moe                       geometry
## 1      3018 195 MULTIPOLYGON (((-122.2469 3...
## 2       218 106 MULTIPOLYGON (((-122.2469 3...
## 3       944 168 MULTIPOLYGON (((-122.2469 3...
## 4        61  50 MULTIPOLYGON (((-122.2469 3...
## 5       176  99 MULTIPOLYGON (((-122.2469 3...
## 6       843 156 MULTIPOLYGON (((-122.2469 3...
## 7       208  75 MULTIPOLYGON (((-122.2469 3...
## 8       546 141 MULTIPOLYGON (((-122.2469 3...
## 9        46  43 MULTIPOLYGON (((-122.2469 3...
## 10       11  17 MULTIPOLYGON (((-122.2469 3...
```

## Reformat Ouput
(1) we only keep the columns of interest - including GEOID and geometry
(2) each estimate variable is in its own column

```r
bay9_data2 <- bay9_data %>%
  select("GEOID", "variable", "estimate") %>%
  spread(key=variable, value=estimate)

bay9_data2
```

```
## Simple feature collection with 1641 features and 26 fields (with 7 geometries empty)
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -123.5335 ymin: 36.85065 xmax: -121.2082 ymax: 38.86424
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## First 10 features:
##          GEOID B05001_001 B05001_006 B05005_001 B05005_002 B05005_007
## 1  06001400100       3018        218        944         61        176
## 2  06001400200       1960         92        315         33         55
## 3  06001400300       5236        317        900        130        314
## 4  06001400400       4171        190        524         69        115
## 5  06001400500       3748        430        701        226        123
## 6  06001400600       1661         62        205         21          9
## 7  06001400700       4552        353        619        122        177
## 8  06001400800       3506        276        767        108        185
## 9  06001400900       2262        202        446         20         16
## 10 06001401000       6193        477        754         90        186
##    B05007_001 B05007_014 B05007_027 B05007_040 B05007_081 B05007_094
## 1         843        208        546         46         11         43
## 2         243         65        136         28         19         14
## 3         857        119         90        257         86        391
## 4         471        146        228         35         15         62
## 5         635        160         83        245         37        147
## 6         178         49         74         38          0         17
## 7         587         70        145        299         67         73
## 8         741        181        330        149          8         81
## 9         405         20        136        148         38        101
## 10        671         56         46        519         67         50
##    B06001_001 B06001_049 B06012_001 B06012_002 B06012_003 B06012_017
## 1        3018        843       3011        113         20        843
## 2        1960        243       1952        106         84        243
## 3        5236        857       5153        450        217        848
## 4        4171        471       4158        268        198        471
## 5        3748        635       3733        339        240        635
## 6        1661        178       1656        158        229        178
## 7        4552        587       4552        820        723        587
## 8        3506        741       3457        381        348        741
## 9        2262        405       2228        358        268        405
## 10       6193        671       6184       1466        768        671
##    B06012_018 B06012_019 B27020_002 B27020_006 B27020_008 B27020_012
## 1          31         12       2175         88        625         12
## 2          31         22       1717         38        151          0
## 3         124        183       4379        111        540         34
## 4          52         82       3694        152        281          0
## 5         151         58       3113        243        205          6
## 6          59         21       1483        143        116          8
## 7         107        103       3965        512        234         33
## 8          75        191       2759        184        465         99
## 9          66         39       1857        103        203         15
## 10        120         17       5513        463        194          0
##    B27020_013 B27020_017                       geometry
## 1         218         10 MULTIPOLYGON (((-122.2469 3...
## 2          92         35 MULTIPOLYGON (((-122.2574 3...
## 3         317         15 MULTIPOLYGON (((-122.2642 3...
## 4         190         21 MULTIPOLYGON (((-122.2618 3...
## 5         430        148 MULTIPOLYGON (((-122.2694 3...
## 6          62          0 MULTIPOLYGON (((-122.2681 3...
## 7         353        147 MULTIPOLYGON (((-122.2779 3...
## 8         276         79 MULTIPOLYGON (((-122.2887 3...
## 9         202         28 MULTIPOLYGON (((-122.2856 3...
## 10        477        111 MULTIPOLYGON (((-122.2787 3...
```

## Rename the columns

Use our dataframe of census variables to rename the columns so that they are self-describing.

```r
colnames(bay9_data2)<-c("GEOID", my_cenvar_df$my_cen_var_names, "geometry")

bay9_data2
```

```
## Simple feature collection with 1641 features and 26 fields (with 7 geometries empty)
## geometry type:  MULTIPOLYGON
## dimension:      XY
## bbox:           xmin: -123.5335 ymin: 36.85065 xmax: -121.2082 ymax: 38.86424
## epsg (SRID):    4269
## proj4string:    +proj=longlat +datum=NAD83 +no_defs
## First 10 features:
##          GEOID citizenship_totpop citizenship_non_citizen entry_totpop
## 1  06001400100               3018                     218          944
## 2  06001400200               1960                      92          315
## 3  06001400300               5236                     317          900
## 4  06001400400               4171                     190          524
## 5  06001400500               3748                     430          701
## 6  06001400600               1661                      62          205
## 7  06001400700               4552                     353          619
## 8  06001400800               3506                     276          767
## 9  06001400900               2262                     202          446
## 10 06001401000               6193                     477          754
##    entry_2010 entry_2000_2009 birthplace_totpop birthplace_europ
## 1          61             176               843              208
## 2          33              55               243               65
## 3         130             314               857              119
## 4          69             115               471              146
## 5         226             123               635              160
## 6          21               9               178               49
## 7         122             177               587               70
## 8         108             185               741              181
## 9          20              16               405               20
## 10         90             186               671               56
##    birthplace_asian birthplace_latinAmerica birthplace_southAmerica
## 1               546                      46                      11
## 2               136                      28                      19
## 3                90                     257                      86
## 4               228                      35                      15
## 5                83                     245                      37
## 6                74                      38                       0
## 7               145                     299                      67
## 8               330                     149                       8
## 9               136                     148                      38
## 10               46                     519                      67
##    birthplace_other_nonUSA birthplace_byage_totpop birthplace_byage_fborn
## 1                       43                    3018                    843
## 2                       14                    1960                    243
## 3                      391                    5236                    857
## 4                       62                    4171                    471
## 5                      147                    3748                    635
## 6                       17                    1661                    178
## 7                       73                    4552                    587
## 8                       81                    3506                    741
## 9                      101                    2262                    405
## 10                      50                    6193                    671
##    poverty_totpop below_pov below_pov2 poverty_fborn_totpop
## 1            3011       113         20                  843
## 2            1952       106         84                  243
## 3            5153       450        217                  848
## 4            4158       268        198                  471
## 5            3733       339        240                  635
## 6            1656       158        229                  178
## 7            4552       820        723                  587
## 8            3457       381        348                  741
## 9            2228       358        268                  405
## 10           6184      1466        768                  671
##    below_pov_fborn below_pov2_fborn health_native_totpop
## 1               31               12                 2175
## 2               31               22                 1717
## 3              124              183                 4379
## 4               52               82                 3694
## 5              151               58                 3113
## 6               59               21                 1483
## 7              107              103                 3965
## 8               75              191                 2759
## 9               66               39                 1857
## 10             120               17                 5513
##    health_native_noinsurance health_fborn_nat_totpop
## 1                         88                     625
## 2                         38                     151
## 3                        111                     540
## 4                        152                     281
## 5                        243                     205
## 6                        143                     116
## 7                        512                     234
## 8                        184                     465
## 9                        103                     203
## 10                       463                     194
##    fborn_nohealth_naturalized health_fborn_noncit_totpop
## 1                          12                        218
## 2                           0                         92
## 3                          34                        317
## 4                           0                        190
## 5                           6                        430
## 6                           8                         62
## 7                          33                        353
## 8                          99                        276
## 9                          15                        202
## 10                          0                        477
##    fborn_nohealth_noncitizen                       geometry
## 1                         10 MULTIPOLYGON (((-122.2469 3...
## 2                         35 MULTIPOLYGON (((-122.2574 3...
## 3                         15 MULTIPOLYGON (((-122.2642 3...
## 4                         21 MULTIPOLYGON (((-122.2618 3...
## 5                        148 MULTIPOLYGON (((-122.2694 3...
## 6                          0 MULTIPOLYGON (((-122.2681 3...
## 7                        147 MULTIPOLYGON (((-122.2779 3...
## 8                         79 MULTIPOLYGON (((-122.2887 3...
## 9                         28 MULTIPOLYGON (((-122.2856 3...
## 10                       111 MULTIPOLYGON (((-122.2787 3...
```

# Questions? 
 
# Next steps

## ACS 5 Year

Latest is 2012 - 2016

2013 - 2017 release date is dec 8, 2018

See: https://www.census.gov/programs-surveys/acs/news/data-releases/2017/release-schedule.html

See when `tidycensus` package is updated....!!!

## References

- [DataCamp](https://www.datacamp.com) course [Analyzing US Census Data in R!](https://www.datacamp.com/courses/analyzing-us-census-data-in-r)
- [Geocomputation in R](https://geocompr.robinlovelace.net/)
- [Creating beautiful demographic maps with tidycensus and tmap packages](https://www.zevross.com/blog/2018/10/02/creating-beautiful-demographic-maps-in-r-with-the-tidycensus-and-tmap-packages/)

## Related D-Lab Workshops

- Geospatial Data in R, parts 1, 2, & 3
- Web Maps in R with Leaflet
- Geocoding & Mapping in R


# Extras for Enthusiasts

## Fetching data for multiple years

Requires variable name to be the same!

```r
# use purr::map_df to get data for multiple years (must have same vars!)
pop90_10 <- map_df(c(1990, 2000, 2010), function(x) { 
  get_decennial(geography = "state",
  variables = c(totalpop = "P001001"),
  dataset = "sf1",
  year = x) %>%
  mutate(year = x) }
)

# View output
head(pop90_10)
tail(pop90_10)

# Plot it
pop90_10 %>% ggplot(aes(x=reorder(NAME,value), y=value/1000000, fill=factor(year))) + 
             geom_bar(stat="identity", position=position_dodge()) + coord_flip()
```


# Combining Census Data with Other Data

## Area Weighted Interpolation

One of the strenghts of the `sf` package is how relatively easy it is to reaggregate data from one geometry to another. This process is called areal interpolation.

Area weighted interpolation reaggregates the data based on the percent of area shared by input and output geometeries.

## Read in a Shapefile

```r
sfnhoods<- st_read("data/sfnhoods.shp")
head(sfnhoods)
plot(sfnhoods['nhood'])
```

##  Check the CRS

```r
st_crs(sfnhoods)
st_crs(sf_poor5)
```

## CRS transformation

```r
sf_poor5_4326 = st_transform(sf_poor5, st_crs(sfnhoods))
```

## Area Weighted Interpolation

Reaggregate percent of people below poverty from census tract to neighborhood polygons.


```r
sfhoods2 = st_interpolate_aw(sf_poor5_4326[, "pct_below_pov"], sfnhoods,
extensive = F) # True= aw sum; False= aw avg
```

## Map it

```r
par(mfrow=c(1,2))
plot(sf_poor5['pct_below_pov'])
plot(sfhoods2['pct_below_pov'])
par(mfrow=c(1,1))
```

## Map it with `tmap`

```r
tm_shape(sfhoods2) +
   tm_polygons(col="pct_below_pov")
```

## Combine the values

```r
head(sfhoods2)
sfnhoods$pct_below_pov <- sfhoods2$pct_below_pov

# map again - click on polygons and view data in popups
# to confirm the AWI output values
tm_shape(sfnhoods) +
  tm_polygons(col="pct_below_pov", 
    popup.vars = c("nhood", "pct_below_pov")
  )
```
