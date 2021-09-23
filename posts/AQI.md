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
4.	Log Transformation: variables exhibiting skewed distributions were log-transformed.

## Exploratory Data Analysis (EDA)
---
Several plots were created within the EDA phase to visualize the variable distribution and determine necessary preprocessing techniques.
#### Data Distribution
First, a histogram plotting of the variables' distribution revealed multiple variables, particularily aerosol variables, to exhibit a skewed distribution. Such results were expected, as aerosol concentrations typically remain low, but may suddenly increase to large values during specific environmental and weather events (e.g.: fires, duststorms, etc.).  For variables with skewed distributions, a log transformation was applied to procure a more normal distribution.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/data_distribution.png?raw=true"></p>

#### Correlation Matrix
Next, a correlation matrix of the variables revealed certain variables to exhibit high correlations, for example: BCMASS & OCMASS (0.95), T500 & T850 (0.87), and SED & T10m (0.70). Such high correlations were also expected, as some features, such as T500 and T850 are simply the same measurements at different atmospheric heights, while others, such as BCMASS and OCMASS typically closely follow one another.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/correlation_matrix.png?raw=true"></p>

#### Geographical Distribution
Finally, map of the contiguous U.S., Alaska, Hawaii, and Puerto Rico was produced to visualize the geographic distribution of our sites. From our training data, thee appeared to be the highest concentration of sites across the coastal and Midwest regions and the lowest concentration of sites across the mountain, southern, and non-contiguous regions.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/geo_distribution.png?raw=true"></p>


## Modelling
Given the large environmental and geographical diversity of the United States, rather than creating one singular machine learning model for predicting the PM2.5 values at all U.S sites, we elected to create multiple regional models. As such, we divided the data based on their sites' EPA regions in order to a seperate model for each region. For example, all the data points in Iowa/Kansas/Missouri/Nebraska, the four states constituting Region 7, will be binned to create a singular model. By disaggregating the data, we sought to create models capable of tailoring to the environmental and geographic characeristic of each region. Conseqeuntially, although Puerto Rico, Hawaii, and Alaska are grouped into regions 2, 9, and 10 respectively, given their unique characerstics, these areas were removed from their respective regions and binned into their own region.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/epa_regions.png?raw=true"></p>

---
#### Model, Step 1: Base Regressor Models
Before creating seperate models for each region, we sought to first identify a uniform base regressor that would perform consistently well across all regions. That is, we did not wish to have one region’s model use Random Forest and another region’s model use Linear Regression. As our dataset was quite large, we chose to sample 10% of our data for training and validation. From the sampled data, 70% was allocated for training and 30% for testing. After scaling our data, we then trained our model on multiple base regoressor and identified the Random Forest regressor to have yielded the highest validation r2 score.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model1_table.png?raw=true"></p>

#### Model, Step 2: Untuned Regional Models
Now that we have selected our base regressor, we divide the data based off EPA regions in order to build models for each region. Our regional models performed considerably better than our aggregating model, with each regional model obtaining a validation r2 score higher than the 0.690 validation r2 score of the aggregated random forest model. Yet, similar to the aggregated model, the regional validation r2 scores were consistently lower their corresponding training r2 score, indicating that the models were likely overtrained and that regularization would be necessary.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model2_table.png?raw=true"></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model2_boston.png?raw=true"></p>

#### Model, Step 3: Tuned Regional Models
###### <i> Comparison with Untuned Regional Models </i>
To induce regularization, the hyperparameters of <i>n_estimators</i>, <i>max_depth</i>, and <i>min_samples_leaf</i> were tuned in an attempt to reduce overfitting of the training data. For each region, six separate random forest models with different hyperparameter configurations were trained and the “best-performing” hyperparameter setting was selected for that region. In this study, the “best-performing” hyperparameter setting is defined as the setting with the highest validation r2 score amongst all settings in which the difference between validation and training r2 scores was less than 0.12. Hyperparameter tuning was found was found to be an effective method for reducing overfitting in multiple regions. For example, in Region 1 (Boston), while the difference between validation and training r2 scores was almost 0.20 in the untuned model, the difference was only 0.09 wihthin the tuned model.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_boston.png?raw=true"></p>

###### <i> Comparison with Standard Formula</i>
A common method for calculating PM2.5 values from Merra2 data is given from the equation: `PM2.5 = DUSTMASS + SSSMASS25 + BCSMASS + 1.4*OCSMASS + 1.375SO4MASS` (Malm et al., 2011, Buchard et al., 2016). To compare our models' forecasting performance compared to the formula's forecasting perfomance, we aggregated the regional dataset results back into a pooled results dataset in order to calculate our metrics. Compared to both the predictions generated from the untuned/aggregated model from Model 1 and the predictions generated from the standard formula, our the predictions generated from our tuned regional models performed substantially better, yielding a pooled r2 validation score of 0.815, compared to the 0.690 of Model 1 and 0.375 of the standard formula. Moreover, our model reduced RMSE by eight-folds and percent error by three-folds when compared to the standard model. In particular, a time-series plotting 
based off the time-series for individual regions, we can see that the formula-based predictions often excessively overestimate the peaks of high PM2.5 events while our model is able to capture the variations in PM2.5 distribution without overestimating.

#### Model, Step 4: Site Cross-Validation Models

## Conclusion
---
#### Part 1: U.S. Sites
#### Part 2: Embassy Sites
#### Part 3: Overseas Sites
