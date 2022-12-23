# TFM_trafico
![Cars](https://cdn.pixabay.com/photo/2019/04/29/05/12/indoor-4165199_1280.jpg)

## Introduction

This repository contains the relevant notebooks and other documents related to my master's thesis. In this work, I have tried to study the performance 
of different Machine Learning models based on the LightGBM algorithm for regression. I have tried different sets of predictors, and looked for the optimal
combination of them. The main set of models has been trained for traffic data in the district of Chamberí (Madrid, Spain), containing traffic information 
that spans for a year, with a 15-minute frequency. 

After figuring out the most 'exportable' models, these have been tried in two other cities, i.e. Bordeaux (France) and Constance (Germany).

In the process of analyzing the traffic, a significant amount of knowledge has been acquired, which has permitted to identify the critical points of car
circulation in Chamberí (Madrid).

## Predictors

The predictors included are:

* Car traffic data (source: City of Madrid open data).
* Air quality data (source: City of Madrid open data).
* Meteorological data, i.e., wind, solar radiation, total precipitation and 2m temperature (source: Era5-Land database/Copernicus).
* Land use data (source: OpenStreetMap/Geofabrik).
* School holidays and public holidays data (source: Madrid Autonomous Region official calendar). 

## Work structure

This thesis consists of a main report (in Spanish), which will be acessible at the UOC university's institutional repository (https://openaccess.uoc.edu/).

In this repository, all of the notebooks used for this research can be found, along with the main conclusions and highlights of the work.

## Preprocessing

The data comes from various sources, so in each case a different process has been followed.

The traffic data has been analyzed in order to discard the stations that didn't give an acceptable result in a simple preliminary predictive model.
It has also been filtered through many conditions. I have made sure there weren't any inconsistent values (i.e., intensity incoherent with occupancy), 
time slots with less than 85% of valid measurements or days with significant missing data. Stations that were consistent in giving bad measurements have
been removed altogether. Outliers have also been cleaned.

Air quality data has been resampled in order to fit the traffic data structure. It has been related to its nearest traffic station, cleaned and interpolated. 
Weather data is processed through a similar process.

Land use data is clipped in QGIS, and the layers of interest are selected. Buffers of the traffic stations are also created, at variable radiuses. Then, everything 
is exported to Python and Geopandas, where some operations allow to shape the layers with the desired information. This information consists basically of 
buffers containing land use data for each traffic station, including surface of schools, hospitals or parks, or distance of the road network according to the
road's importance in the network.

Once this preprocessing is done, some features are added to the final dataset, such as buffers containing the traffic intensity of the surrounding stations
(with a gap of 24 hours, giving the information of what happened the day before around each station), and a column of the intensity 24h before in each station.

## Models

| Model Number  | Info included                                                |
| ------------- | ------------------------------------------------------------ |
| 1             | All variables except intensity memory and stations' buffers  |
| 2             | Most important predictors from model 1                       | 
| 3             | Most important predictors from model 2                       |
| 4             | Most important predictors from model 3 (meteo and int memory)| 
| 5             | Meteo and most important land use predictors                 |
| 6             | Occupancy and int memory                                     | 
| 7 & 8         | Surrounding stations buffers                                 |

## Results


| Model Number  | R2                                               |
| ------------- | --------|
| 1             | |
| 2             | | 
| 3             | |
| 4             | | 
| 5             | |
| 6             | | 
| 7 & 8         | |
