# Spatial-heterogeneity-of-air-pollution-statistics
## Introduction
In this project, we propose a technique for analyzing the spatial heterogeneity of air pollution data in order to better understand the dynamics of air pollution over time. The method was applied to data from Europe, but it can be applied to data from anywhere in the world.

## How to use
1. Download data using the R code provided, utilizing the Saqgetr" package: "https://github.com/skgrange/saqgetr", saving them as csv files. It can be implemented as follow:
```
require(devtools)
install_github('davidcarslaw/worldmet')
library(worldmet)
library(openair)
library(tidyverse)
europe_meta <- importMeta(source = "Europe")
# Install saqgetr package
install.packages("saqgetr")
# Load packages
library(dplyr)
library(saqgetr)
# Import site information
data_sites <- get_saq_sites()
info <- getMeta()

europe_lat = data_sites["latitude"]
europe_lon = data_sites["longitude"]
europecode = data_sites["site"]

for (siteIndex in 1:10000){
  print(siteIndex)
  selected_site = europecode[siteIndex,]
  print(selected_site)
  #abstract pollution data
  thedata <- get_saq_observations(site = selected_site, start = 2017, end=2021) %>% 
    saq_clean_observations(summary = "hour", valid_only = TRUE)  
  if(nrow(thedata) == 0){next
  }else{
    selected_lat = europe_lat[siteIndex,]
    selected_lon = europe_lon[siteIndex,]
    if(is.na(selected_lat) || is.na(selected_lon)){next
      }else{
        selected_lat <- as.numeric(selected_lat)
        selected_lon <- as.numeric(selected_lon)
        #abstract closest meta data
        info_x <- getMeta(lat = selected_lat, lon = selected_lon, n = 1, plot = FALSE)
        match_lat = info_x['latitude']
        match_lon = info_x['longitude']
        match_lat <- as.numeric(match_lat)
        match_lon <- as.numeric(match_lon)
        if (round(match_lat, digits =1) == round(selected_lat, digits =1) && 
            round(match_lon, digits =1) == round(selected_lon, digits=1)){
          match_code <- as.character(info_x['code'])
          met_x <- importNOAA(code = match_code, year = 2017:2021)
          if (is.null(met_x) ){next
          } else{
            # merge the two data sets 
            thedata_met <- left_join(
              thedata,
              met_x,
              by = "date"
            )
            filename=paste(europecode[siteIndex,],".csv" , sep = "") 
            # name by site code
            write.csv(thedata_met, filename)}
        }else {next
        }
      }
  }
}
```

2. Open all ".ipynb" files in Jupyter Notebook
3. Use "dataframe.ipynb" to set up dataframes
4. Use "Europe basemap.ipynb" to create an overview of the data sets
5. Use "European air pollutant analysis.ipynb" to analyse the data and recreate graphs
