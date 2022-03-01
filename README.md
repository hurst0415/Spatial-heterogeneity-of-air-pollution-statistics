# Spatial-heterogeneity-of-air-pollution-statistics
## Introduction
In this project, we propose a technique for analyzing the spatial heterogeneity of air pollution data in order to better understand the dynamics of air pollution over time. The method was applied to data from Europe, but it can be applied to data from anywhere in the world.

## How to use
1. Download data using the R code provided, utilizing the Saqgetr" package: "https://github.com/skgrange/saqgetr", saving them as csv files. It can be implemented as follow:
```
# Install saqgetr package
install.packages("saqgetr")
# Load packages
library(dplyr)
library(saqgetr)
# Import site information
data_sites <- get_saq_sites()
write.csv(data_sites, "data_sites.csv")
europecode = data_sites["site"]
for (siteIndex in 0:10000){
  print(siteIndex)
  selected_site = europecode[siteIndex,]
  print(selected_site)
  thedata <- get_saq_observations(site = selected_site, start = 2017, end=2021) %>% 
    saq_clean_observations(summary = "hour", valid_only = TRUE)  
  filename=paste(europecode[siteIndex,],".csv" , sep = "") 
  # name by site code
  write.csv(thedata, filename)
}
```

2. Open all ".ipynb" files in Jupyter Notebook
3. Use "dataframe.ipynb" to set up dataframes
4. Use "Europe basemap.ipynb" to create an overview of the data sets
5. Use "European air pollutant analysis.ipynb" to analyse the data and recreate graphs
