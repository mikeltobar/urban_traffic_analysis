# TFM_trafico
![Cars](https://cdn.pixabay.com/photo/2019/04/29/05/12/indoor-4165199_1280.jpg)

## Introduction

This repository contains the relevant notebooks and other documents related to my master's thesis. In this work, I have tried to study the performance of different Machine Learning models based on the LightGBM algorithm for regression. I have tried different sets of predictors, and looked for the optimal combination of them. The main set of models has been trained for traffic data in the district of Chamberí (Madrid, Spain), containing traffic information that spans for a year, with a 15-minute frequency. 

After figuring out the most 'exportable' models, these have been tried in two other cities, i.e. Bordeaux (France) and Constance (Germany).

In the process of analyzing the traffic, a significant amount of knowledge has been acquired, which has permitted to identify the critical points of car circulation in Chamberí (Madrid).

## Predictors

The predictors included are:

* Car traffic data (source: City of Madrid open data [1]).
* Air quality data (source: City of Madrid open data [2]).
* Meteorological data, i.e., wind, solar radiation, total precipitation and 2m temperature (source: Era5-Land database/Copernicus [3]).
* Land use data (source: OpenStreetMap/Geofabrik [4]).
* School holidays and public holidays data (source: Madrid Autonomous Region official calendar [5]). 

## Work structure

This thesis consists of a main report (in Spanish), which will be acessible at the UOC university's institutional repository (https://openaccess.uoc.edu/).

In this repository, all of the notebooks used for this research can be found, along with the main conclusions and highlights of the work.

## Preprocessing

The data comes from various sources, so in each case a different process has been followed.

The traffic data has been analyzed in order to discard the stations that didn't give an acceptable result in a simple preliminary predictive model. It has also been filtered through many conditions. I have made sure there weren't any inconsistent values (i.e., intensity incoherent with occupancy), time slots with less than 85% of valid measurements or days with significant missing data. Stations that were consistent in giving bad measurements havebeen removed altogether. Outliers have also been cleaned.

Air quality data has been resampled in order to fit the traffic data structure. It has been related to its nearest traffic station, cleaned and interpolated. Weather data is processed through a similar process.

Land use data is clipped in QGIS, and the layers of interest are selected. Buffers of the traffic stations are also created, at variable radiuses. Then, everything is exported to Python and Geopandas, where some operations allow to shape the layers with the desired information. This information consists basically of buffers containing land use data for each traffic station, including surface of schools, hospitals or parks, or distance of the road network according to the road's importance in the network.

Once this preprocessing is done, some features are added to the final dataset, such as buffers containing the traffic intensity of the surrounding stations (with a gap of 24 hours, giving the information of what happened the day before around each station), and a column of the intensity 24h before in each station.

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


| Model Number  | R2    | R2-adj | MAE     | RMSE    | 
| ------------- | ----- | ------ | ------- | ------- |
| 1             | 0.977 | 0.977  | 58.599  | 93.539  |
| 2             | 0.960 | 0.960  | 76.445  | 122.740 |
| 3             | 0.941 | 0.941  | 87.496  | 148.159 |
| 4             | 0.926 | 0.926  | 98.494  | 166.243 |
| 5             | 0.959 | 0.959  | 74.015  | 124.865 |
| 6             | 0.821 | 0.821  | 149.044 | 258.998 |
| 7             | 0.789 | 0.789  | 228.876 | 338.726 |
| 8             | 0.739 | 0.737  | 53.012  | 70.834  |

## Transfer learning

After trying with many different possibilities, models 4 and 5 turn out to be the most exportable. Therefore, I have tried their predictors in two other cities: Bordeaux (France) and Constance (Germany).

Land use and meteo data come from the sources named before. However, traffic data differs, in this two models it comes from the UTD-19 research [6].

Results are the following:

| Model Number  | R2    | R2-adj | MAE     | RMSE    | 
| ------------- | ----- | ------ | ------- | ------- |
| 9             | 0.845 | 0.845  | 75.280  | 124.944 |
| 10            | 0.836 | 0.836  | 83.197  | 128.521 |
| 11            | 0.825 | 0.825  | 41.960  | 65.425  |
| 12            | 0.888 | 0.887  | 33.460  | 52.411  |

## Critical points

The accumulated knowledge allows us to have a picture of the traffic congestion in the district of Chamberi, and of its critical points.

## Conclusions

The models are satisfactory, and the exportable ones (4 and 5) prove useful in other cities. Moreover, the chosen predictors explain pretty accurately the variance, and they are easily portable. This is very interesting for cities that lack an extensive traffic detection network or that don't have one whatsoever (model 5 is capable of predicting traffic data without any traffic measurement).

However, it is important to note that although the model has given good results for other two European cities, it might not be as useful in others with special characteristics, such as heavy snowfall, very high or very low density, or other planning issues.

## Bibliography 

[1] Tráfico. Histórico de datos del tráfico desde 2013 [online] [accessed: 23 December 2022]. Available at: https://datos.madrid.es/portal/site/egob/menuitem.c05c1f754a33a9fbe4b2e4b284f1a5a0/?vgnextoid=33cb30c367e78410VgnVCM1000000b205a0aRCRD&vgnextchannel=374512b9ace9f310VgnVCM100000171f5a0aRCRD&vgnextfmt=default

[2] Calidad del aire. Datos horarios desde 2001 [online] [accessed: 23 December 2022]. Available at: https://datos.madrid.es/portal/site/egob/menuitem.c05c1f754a33a9fbe4b2e4b284f1a5a0/?vgnextoid=f3c0f7d512273410VgnVCM2000000c205a0aRCRD&vgnextchannel=374512b9ace9f310VgnVCM100000171f5a0aRCRD&vgnextfmt=defaultvgnextoid=33cb30c367e78410VgnVCM1000000b205a0aRCRD&vgnextchannel=374512b9ace9f310VgnVCM100000171f5a0aRCRD&vgnextfmt=default

[3] ERA5-Land hourly data from 1950 to present [online] [accessed: 23 December 2022]. Available at: https://cds.climate.copernicus.eu/cdsapp#!/dataset/10.24381/cds.e2161bac?tab=overview

[4] Geofabrik Download Server [online] [accessed: 23 December 2022]. Available at: https://download.geofabrik.de/

[5] Calendario escolar 2021-2022 [online] [accessed: 23 December 2022]. Available at: https://www.educa2.madrid.org/web/calendario-escolar-de-la-comunidad-de-madrid/calendario-escolar-2021-22

[6] Loder, A., Ambühl, L., Menendez, M., & Axhausen, K. W. (2019). Understanding traffic capacity of urban networks. Scientific reports, 9(1), 1-10.
