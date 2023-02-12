# Tracking Erroneous Measurements for earth's Planetary Operations (TEMPO)
---
**Author**: Paul Lin (Mentor: Brent Smith - NASA Goddard, Global Modeling and Assimilation Office)

**Date**: August 2019

**Project Description:** With modern satellite and instruments collecting millions of data per day, it is imperative to validate such data by highlighting any measurements that fail to fall within a verified range. While abnormal measurements may sometimes derive from extraordinary environmental events, other times, they indicate errors in model processing and data collection. As such, NASA’s Global Modeling and Assimilation Office (GMAO) is developing a database and reporting system that better tracks errors within dataset from the Soil Moisture Active Passive (SMAP) satellite mission, Composition Forecast, MERRA-2 reanalysis, amongst others.

## Introduction
---
Synergizing Python packages (cartopy, netCDF4, h5Py) and front-end web development (leaflet, Bootstrap), TEMPO produces interactive web pages that allow users to dynamically visualize Earth Science data. That is, users may select from a multitude of options for displaying data projections of various satellite data on a wide spectrum of view levels ranging from global to street-level. In addition, users also have the option of displaying different ranges of values, such as the top 50%, bottom 50%, or those above/below an expected maximum/minimum threshold.

**Project Goals:**
- Update the existing data verification package Quality Assurance of Data sets from Python 2 to Python 3
- Develop web interfaces allowing for dynamic visualization of satellite datasets and error points
- Remodel report interface and alert system for erroneous data to increase user-friendliness

## Home Interface
---
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/TEMPO/home_interface.PNG?raw=true"></p>

## Features
---
### Erroneous Value Detection

### Administrative & Physical Layer Overlays
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/TEMPO/layering.PNG?raw=true"></p>

### Extended Features

<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/TEMPO/error_masking.PNG?raw=true"></p>
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/TEMPO/features.PNG?raw=true"></p>

## Discussion
---
Although TEMPO was originally intended as an operations-oriented service to identify errors in data ingestion and transformation, the GMAO team recognized the potential of TEMPO's application for climate modelling and analysis purposes. Due to TEMPO's capabilities for identifying data outliers, TEMPO is particularly useful for identifying extreme weather events. For example, after analyzing several Earth Science datasets, TEMPO identified a range breach for horizontal wind velocity (u-wind) and ozone levels that indicated unusually strong high Polar Westerly zonal wind velocity for July 22, 2019.
<p align = "center"><img src="https://github.com/paulslin/paulslin.github.io/blob/main/images/TEMPO/polar_event.PNG?raw=true"></p>
Such strong velocities allow the Antarctic polar vortexes to trap more cold air, thereby encouraging ice crystals and polar stratospheric cloud formations. As winter transitions into springs, the polar vortex will weaken and induce chemical reactions from within clouds to produce chlorine, the primary catalyst for ozone depletion. TEMPO’s identification of the strong Polar westerly alerts for special attention for analyzing Ozone levels in the upcoming Southern Hemisphere’s spring season. Such is just one example of TEMPO's capability to detect unusual environmental phenomena, as well as potential operational errors. With further development, TEMPO seeks to investigate the spatial, temporal, and seasonal aspects of GEOS data, as well as develop an alert system for reporting erroneous data.

