# Census Data in R

This workshop provides an introduciton to working with census data in R using the `tidycensus` package. 

## Description

Since 1790, the US Census has been a critical source of data about Americans living in the United States, providing valuable insights to social scientists and humanists. Mapping these data by census geographies adds more value by allowing researchers to explore spatial trends and outliers. This workshop will introduce three key packages for streamlining census data workflows in R: `tigris`, `tidycensus` and `tmap`. Participants will learn how to download census tabular data for one or more geographic aggregation units or years, download the associated census geographic data and then join these data for analysis and mapping. 

Specifically, we will:

- Describe the primary Census data products
- Introduce the R `tidycensus` package for working with Census Data
- Use R packages to fetch decennial and ACS census data
- Use R packages to fetch census geograpic boundary files
- Create maps of census data, symbolizing the color of those maps by the data values

### Knowledge Requirements: 

To participate in this tutorial, participants are expected to have R experience equivalent to the [D-Lab R Fundamentals](https://github.com/dlab-berkeley/R-Fundamentals) workshop series. Basic knowledge of census data and geospatial data will be helpful. 

### Tech Requirements:

Please bring a laptop with the following: 

* [R version 3.5](https://cloud.r-project.org/) or greater with the [RStudio integrated development environment (IDE)](https://www.rstudio.com/products/rstudio/download/#download).
* The following R packages installed: `sp`, `sf`, `rgdal`, `rgeos`, `raster`, `ggplot2`, `tmap`, `tigris`, `tidycensus` and `mapview`.
* Obtain a [Census API key](https://api.census.gov/data/key_signup.html)

You can download the repo and work through the tutorial by following along with the slides:

 - https://dlab-berkeley.github.io/Census-Data-in-R/Rcensus_data_maps-slides.html#1

## Requesting a Census API key

The `tidycensus` package, and any R package that accesses the Census APIs, requires you to first get a Census API key

Get one now if you don’t have one yet here (just takes a minute): https://api.census.gov/data/key_signup.html

---
<div style="display:inline-block;vertical-align:middle;">
<a href="https://dlab.berkeley.edu/" target="_blank">
<img src ="https://dlab.berkeley.edu/sites/default/files/logo.png" width="60" align="left" border=0 style="border:0; text-decoration:none; outline:none">
</a>
</div>
<div style="display:inline-block;vertical-align:middle;align:left">
    <div style="font-size:larger">D-Lab @ University of California - Berkeley
    </br>
    <a href="https://dlab.berkeley.edu" target="_blank">https://dlab.berkeley.edu</a>
    </br>
    &nbsp;
    </div>
</div>
