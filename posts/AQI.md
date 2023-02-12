# Forecasting AQI Values across U.S. Embassy Sites and EPA Regions
---
**Author**: Paul Lin (Mentor: Pawan Gupta - Universities Space Research Association)

**Date**: August 2021

**Project Description:** 
Air Quality Index (AQI) is a metric for determining pollution contamination concentration. One major component of AQI is particular matter, in particular PM2.5. The capability to forecast AQI and PM2.5 are particularly important, as the U.S. Environmental Protection Agency (EPA) utilizes such metrics to communicate public health risk related to poor air quality. As such, this project's goal was to bolster AQI and PM2.5 forecasting by creating machine learning models capable of accurately predicting PM2.5 and AQI values across both a wide geographical scale and individual cities. This project was split into three parts:
- United States sites
- Embassy sites
- Overseas sites

## Data
---
#### Datasets
Variable data were procured from four sources: <a href="https://docs.airnowapi.org/"> AirNow</a>, <a href="https://www.purpleair.com/sensorlist"> PurpleAir</a>, <a href="https://disc.gsfc.nasa.gov/datasets/M2T1NXAER_5.12.4/summary"> Merra2 Aerosols</a>, and <a href="https://disc.gsfc.nasa.gov/datasets/M2T1NXSLV_5.12.4/summary"> Merra2 Metereology</a>. AirNow and PurpleAir provide values for the target variables, PM2.5 and AQI, while Merra2 Aerosol provides the aerosol feature variables and Merra2 Meteorology  provides the meteorology feature variables.  Target and feature variables are collected hourly and as follows:
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
   - Organic Carbon (OCSMASS)
   - Sulfur Dioxide (SO2SMASS)
   - SO4 (SO4SMASS)
   - Sea Salt (SSSMASS25)
   - Total Extinction Aerosol Optical Depth at 550 nm (TOTEXTTAU)

#### Pre-processing
A series of steps were undertaken to process the data:
1.	Data Validation: row entries were filtered to ensure all feature values were within a specified range to ensure scientific plausibility.
2.	New Features: two new features - Solar-Earth Distance (SED) and Solar Zenith Angle (SZA) were calculated to account for temporal and seasonal variations.
3.	Reverse Geocoding: State and EPA regions of individual sites were determined by reverse geocoding based on the site latitude and longitude. Sites located outside of the United States were removed.
4.	Log Transformation: variables exhibiting skewed distributions were log-transformed.

## Exploratory Data Analysis (EDA)
---
Several plots were created within the EDA phase to visualize the variable distribution and determine necessary preprocessing techniques.
#### Data Distribution
First, histograms revealed that multiple variables, particularly aerosol variables, exhibited a skewed distribution. Such results were expected, as aerosol concentrations typically remain low, but may suddenly increase to large values during specific environmental and weather events (e.g.: fires, dust storms, etc.).  Consequentially, log transformations were applied to generate normal distributions.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/data_distribution.png?raw=true"></p>

#### Correlation Matrix
Next, a correlation matrix also revealed that certain variables exhibited high correlations, for example: BCMASS & OCMASS (0.95), T500 & T850 (0.87), and SED & T10m (0.70). Such high correlations were also expected, as environmental variables are often closely reelaed.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/correlation_matrix.png?raw=true"></p>

#### Geographical Distribution
Finally, a map of the contiguous U.S., Alaska, Hawaii, and Puerto Rico was produced to visualize the geographic distribution of our sites. From our training data, thee appeared to be the highest concentration of sites across the coastal and Midwest regions and the lowest concentration of sites across the mountain, southern, and non-contiguous regions.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/geo_distribution.png?raw=true"></p>


## Modelling
Given the United States' wide environmental and geographical diversity, rather than creating one single machine learning model for predicting the PM2.5 values at all U.S sites, we created multiple regional models, based on EPA regions. By disaggregating the data, our models were tailored to the environmental and geographic characteristic of each region. Notably, although Puerto Rico, Hawaii, and Alaska are grouped into regions 2, 9, and 10 respectively, given their unique characteristics, these areas were removed from their respective regions and binned into their own region.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/epa_regions.png?raw=true"></p>

---
#### Model, Step 1: Base Regressor Models
Before creating separate regional models, we sought to first identify a uniform base regressor that would perform consistently well across all regions. That is, we did not wish to have one region’s model use Random Forest and another region’s model use Linear Regression, and so forth. <!--As our dataset was quite large, we chose to sample 10% of our data for training and validation.--> From the sampled data, 70% was allocated for training and 30% for testing. After scaling our data, we then trained our model on multiple base regressors, with the Random Forest regressor yielding the highest validation r2 score.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model1_table.PNG?raw=true"></p>

#### Model, Step 2: Untuned Regional Models
Next, we divided the data based off EPA regions to build regional models. Our regional models performed considerably better than our aggregated model, with each regional model obtaining a validation r2 score higher than that of the aggregated random forest model (r2=0.690). Yet, the regional validation r2 scores were consistently lower their corresponding training r2 score, indicating that the models were likely overtrained and that regularization would be necessary.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model2_table.PNG?raw=true"></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model2_boston.png?raw=true"></p>

#### Model, Step 3: Tuned Regional Models
###### <i> Comparison with Untuned Regional Models </i>
<!--To induce regularization, the hyperparameters of <i>n_estimators</i>, <i>max_depth</i>, and <i>min_samples_leaf</i> were tuned in an attempt to reduce overfitting of the training data.--> For each region, six separate random forest models with different hyperparameter configurations were trained and the “best-performing” hyperparameter setting was selected for that region. In this study, the “best-performing” hyperparameter setting is defined as the setting with the highest validation r2 score amongst all settings in which the difference between validation and training r2 scores was less than 0.12. Hyperparameter tuning was found was found to be an effective method for reducing overfitting in multiple regions. For example, in Region 1 (Boston), while the difference between validation and training r2 scores was almost 0.20 in the untuned model, the difference was only 0.09 within the tuned model.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_boston.png?raw=true"></p>

###### <i> Comparison with Standard Formula</i>
A common method for calculating PM2.5 values from Merra2 data is given from the equation: `PM2.5 = DUSTMASS + SSSMASS25 + BCSMASS + 1.4*OCSMASS + 1.375SO4MASS` (Malm et al., 2011, Buchard et al., 2016). To compare our models' forecasting performance compared to the formula's forecasting performance, we aggregated the regional dataset results back into a pooled results dataset in order to calculate our metrics. Compared to both the predictions generated from the untuned/aggregated model from Model 1 and the predictions generated from the standard formula, our predictions generated from our tuned regional models performed substantially better, yielding a pooled r2 validation score of 0.815, compared to the 0.690 of Model 1 and 0.375 of the standard formula. Moreover, our model reduced RMSE by eight-folds and percent error by three-folds when compared to the standard model. In particular, a time-series plotting of the observed, model prediction, and formula prediction showed that while the formula-based predictions often excessively overestimate the PM2.5 peak events, our model were able to capture the variations in PM2.5 distribution without overestimating.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_comparison.png?raw=true"></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_time_series.png?raw=true"></p>
<p align = "center"><i>Average daily PM2.5 values in blue (observed), model prediction (orange), and formula prediction (green)</i></p>

###### <i> Comparison with Geographical Distribution </i>
<!--Afterwards, we sought to visualize the model performance with regards to geographic distribution.--> A map of the r2 score over the contiguous U.S. revealed that the models appeared to perform the best within the Pacific Northwest, Northern California, and the Middle Atlantic regions while performing the worst within the Midwest and Southeast regions. 
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_geography.png?raw=true"></p>

###### <i> Comparison with Temporal Distribution </i>
Finally, we also sought to visualize the model performance with regards to temporal distribution. Box-and-Whisker plots of predicted-actual values across each season and hour demonstrated near-identical patterns, suggesting our model performed uniformly across seasons and time of day.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model3_temporal.PNG?raw=true"></p>

#### Model, Step 4: Site Cross-Validation Models
We were also interested in observing the models' capabilities to generalize and predict for new sites. As such, for each region, sites were split into 10 distinct subsets and a 10-fold cross validation was performed within each region. <!--That is, for each region, ten models were created, with 90% of the sites serving as training data and the remaining 10% of the sites as validation data for a singular model.--> After aggregating the results of all region’s models, a map distribution of the r2 score demonstrated near identical distribution to model predictions without cross-validation from Model 3, thereby suggesting that the models were capable of generalize to new sites within their region.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/model4_geography.png?raw=true"></p>

## Conclusion
---
#### Part 1: U.S. Sites
The study's first part was focused on creating machine learning models for sites within the U.S. After partitioning the sites by EPA regions, our regional machine model proved to be effective for predicting PM2.5 values for both existing and new sites and performed significantly better than the standard forecasting method. When considering model accuracy across a temporal and spatial scale, our models were found to have performed consistently across seasonal and diurnal distributions, but inconsistently across geographic distributions.

#### Part 2: Embassy Sites
Given the successes of our United States models, we sought to replicate our previous methods and create new models for overseas U.S. embassy. Instead of grouping embassy sites into geographical regions, a model is created and tuned for each embassy site. Daily data of aerosol and meteorological features are regularly collected and fed into pre-trained models, producing three-day PM2.5 predictions at each embassy site. Finally, prediction values are uploaded into an internal database and viewable within a web interface hosted on AWS. <!--To access the website, click <a href="http://ec2-18-208-186-74.compute-1.amazonaws.com:8081/"> here</a>--->. Within the web interface, users have the option of comparing model predictions, accessing historical observations & predictions, and analyzing model performance.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/interface_home.png?raw=true"></p>
<p align = "center"><i>The home interface for visualizing observation and prediction values across individual and all sites</i></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/interface_lines.png?raw=true"></p>
<p align = "center"><i>Historical observations and predictions of multiple machine learning models</i></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/interface_zoom.png?raw=true"></p>
<p align = "center"><i>Users have the capability to toggle site location and forecasting / prediction dates</i></p>

#### Part 3: Overseas Sites
In the last section, the same methodologies were replicated for overseas regions, including India, Thailand, and Japan. Within Japan, an ETL pipeline was also implemented to regularly procure and feed Merra2 feature data into our training models and output an animation of predicted PM2.5 for the day.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/AQI/japan2.gif?raw=true"></p>
