# Forecasting AQI Values across U.S. Embassy Sites and EPA Regions
---
**Author**: Paul Lin (Mentors: Pawan Gupta - Universities Space Research Association)

**Date**: August 2021

**Project Description:** 
Air Quality Index (AQI) is a metric for determining the degree of pollution contamination persistent within the air. One major component of AQI is particular matter, in particular PM2.5. The capability to forecast AQI and PM2.5 are particularily important, as the U.S. Environmental Protection Agency (EPA) utilizes such metrics to communicate public health risk related to poor air quality. As such, the goal of this project is to bolster AQI and PM2.5 forecasting by creating machine learning models capable of accurately predicting PM2.5 and AQI values across both a wide geographical scale and individual cities. Within this project, the sections are split into three parts
- United States sites
- Embassy sites
- Overseas sites

## Data
---
#### Datasets
Variable data were procured from four sources: <a href="https://docs.airnowapi.org/"> AirNow </a>, <a href="https://www.purpleair.com/sensorlist"> PurpleAir </a>, <a href="https://disc.gsfc.nasa.gov/datasets/M2T1NXAER_5.12.4/summary"> Merra2 Aerosols</a>, and <a href="https://disc.gsfc.nasa.gov/datasets/M2T1NXSLV_5.12.4/summary"> Merra2 Metereology</a>. AirNow and PurpleAir provide values for the target variables, PM2.5 and AQI, while Merra2 Aerosol provides the aersol feature variables and Merra2 Metereology provides the metereology feature variables.  Target and feature variables are collected hourly and as follows:
- Target Variables
  - PM2.5
- Geographic Features
  - Latitude (Lat)
  - Longitude (Lon)
- Temporal Features
   - Radius (SRadius)
   - Solar-Earth Distance (SED)
   - Solar Zenith Angle (SZA)
- Meteorological Features
   - Surface Pressure (PS)
   - Humidity at 10m (QV10m), 500 mb (Q500), and 850 mb (Q850) pressure levels
   - Air Temperature at 10m (T10m), 500 mb (T500), and 850 mb (T850) pressure levels
   - Wind Speed at surface (WIND)
- Aerosol Features
   - Black Carbon (BCSMASS)
   - Dust PM2.5 (DUSMASS25)
   - Organic Carrbon (OCSMASS)
   - Sulfur Dioxide (SO2SMASS)
   - SO4 (SO4SMASS)
   - Sea Salt (SSSMASS25)
   - Total Extinction Aerosol Optical Depth at 550 nm (TOTEXTTAU)

#### Pre-processing
A series of steps were undertaken to process the data:
1.	Data Validation: row entries were filtered to ensure all feature values were within a specified range to ensure scientific plausibility.
2.	Calculation of New Features: two new features - Solar-Earth Distance (SED) and Solar Zenith Angle (SZA) were calculated to account for temporal and seasonal variations.
3.	Reverse Geocoding: State and EPA regions of individual sites were determined by reverse geocoding based on the site latitude and longitude. Sites located outside of the United States were removed.

## EDA
---
#### Data Distribution
A histogram plotting of the variables' distribution revealed multiple variables to exhibit a skewed distribution. Such results are expected, as aerosol concentrations typically remain low, but may suddenly increase to large values during specific environmental and weather events (e.g.: fires, duststorms, etc.).  
such as SO4SMASS, DUSMASS25, and BCMASS are normally low, but increase suddenly during specific environmental and weather events. A log transformation of the skewed variables to procure a more normal distribution.

#### Correlation Matrix
A correlation matrix plotting of the variables revealed certain variables to exhibit high correlations.  Some highly-correlated variable pairs include: BCMASS & OCMASS (0.95), T500 & T850 (0.87), and SED & T10m (0.70).  Such high correlations were also expected, as some features, such as T500 and T850 are simply the same measurements at different atmospheric heights, while others, such as BCMASS and OCMASS often follow similar distributions.

#### Geographical Distribution
Finally, the number of sites within the contiguous U.S., Alaska, Hawaii, and Puerto Rico.  There appears to be the highest concentration of sites across the coastal and Midwest regions and the lowest concentration of sites across the mountain, southern, and non-contiguous regions.

## Modelling
---
#### Model, Step 1: Base Classifier Models
#### Model, Step 2: Untuned Regional Models
#### Model, Step 3: Tuned Regional Models
#### Model, Step 4: Site Cross-Validation Models

## Conclusion
---
#### Part 1: U.S. Sites
#### Part 2: Embassy Sites
#### Part 3: Overseas Sites
