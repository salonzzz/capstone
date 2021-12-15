# CAPSTONE
---

## To Note
The code will be accessible but I believe opening the data file might cause issues. My capstone folder (capped) has been uploaded to my drive as backup – [here](https://drive.google.com/drive/folders/1nkejWRPnDGjq2aBxiqUlW3wF26uQEKnv?usp=sharing)


## Background and problem statement

We are all aware of the impending climate crisis. With growing climatic fluctuations and rising temperatures, we are unsure of how our wild flora and fauna will cope, especially our threatened species. One such species includes the lesser mousedeer or the oriental lesser chevrotain. In this project I hoped to see how changes in climatic variables such as temperature and precipitation (to name a few) would impact habitat suitability for the mousedeer.

The lesser mousedeer is a mammal found in the mature forests of Singapore. It is typically a frugivore and solitary in nature. In Singapore, it is currently restricted to the Central Nature Reserves with very few to no sightings elsewhere in Singapore.

Two reasons why I chose this species to work with include –
(Bearing in mind that I planned to use land cover data initially)

1)  Current distribution
There were numerous sightings of the Saltwater Crocodile and the Mangrove Pit Viper on iNaturalist (will elaborate further below) but both of these species are limited to our coastal regions and specifically the mangroves or swamps. Sourcing high resolution land cover data proved to be difficult for mangrove typologies in Singapore. Choosing a forest dwelling species seemed more feasible.

2) Availability of data
iNaturalist is a platform where citizen scientists can upload pictures of flora and fauna for identification by experts. Presence or occurrence of a species in an area can be obtained from the geospatial data provided by users when they upload a picture. A limitation with this platform is that there is spatial bias occurring in that members of the public can only take pictures of fauna they see in publicly accessible spaces. As a result, the true distribution of a species remains understudied. 

Why is this presence data important? In ecology, because of this exact issue or just the general lack of ability to know every nook and cranny an animal frequents, we model the species distributions based on estimations instead.

This is called species distribution modelling. Presence data allows us to estimate the distribution of a species. This coupled with absence data, or points where a species was surveyed to ‘not occur’ gives us some data to work with. Additional parameters are fed into the model such as elevation, NDVI, land cover, bioclimatic variables etc. to make the results more whole and robust. 

I chose the mousedeer then because it had at least 59 viable observations that I could use as presence points. 

I initially planned to use land cover data as well however, in the interest of time, I had to abandon the venture. 

In essence, this project hoped to explore how variation in bioclimatic variables impacted the distribution of the lesser mousedeer.  The aim was to use machine learning to draw associations between climatic features and mousedeer presence and predict whether an area would serve as suitable habitat in future scenarios.

Models used include RandomForestClassifier, KNeighborsClassifier, SVM and Logistic Regression (which is also termed the MaxEnt or maximum entropy model which is the most commonly used model and also a baseline model in this study.

## Process

Presence data for the lesser mousedeer was obtained from iNaturalist and pseudoabsence points were later generated. 

Land cover data was originally obtained from URA’s masterplans for 2019 and 2030 however, due to running into issues while rasterizing the files, these variables ha to be dropped.

Bioclimatic variable data for current and the two future scenarios was obtained from worldclim.org

Variables described below.

Current
Type: Continuous
Resolution: 1km
Year: 1970-2000

| Bioclimate variables | 
| --- |
| BIO1 Annual Mean Temperature |
| BIO2 Mean Diurnal Range (Mean of monthly (max temp-min temp)) |
| BIO3  Isothermality (BIO2/BIO7) (x100) |
| BIO4 Temperature Seasonality (standard deviation x100) |
| BIO5 Max Temperature of Warmest Month |
| BIO6 Min Temperature of Coldest Month |
| BIO7 Temperature Annual Range (BIO5–BIO6) |
| BIO8 Mean Temperature of Wettest Quarter |
| BIO9 Mean Temperature of Driest Quarter |
| BIO10 Mean Temperature of Warmest Quarter |
| BIO11 Meant Temperature of Coldest Quarter |
| BIO12 Annual Precipitation |
| BIO13 Precipitation of Wettest Month |
| BIO14 Precipitation of Driest Month |
| BIO15 Precipitation Seasonality (Coefficient of Variation) |
| BIO16 Precipitation of Wettest Quarter |
| BIO17 Precipitation of Driest Quarter |
| BIO18 Precipitation of Warmest Quarter |
| BIO19 Precipitation of Coldest Quarter |

Future 1 and Future 2
Type: Continuous
Resolution: 4.5km
Year: 2021-2040

Future 1 (SSP3-70) is defined as “middle of the road” scenario showing 4.5 °C of warming.
Future 2 (SSP2-45) is defined as warming to 3 °C by 2100 with a slow decline in CO2 emissions

Raster data for each variable was downloaded and clipped to the map extent of Singapore in QGIS (geospatial analysis software).  For future predictions, each band of the file represented a variable and hence had to be extracted and clipped accordingly. 

Presence-absence data and the raster files were then fed into pyimpute, a package that is able to work with raster data- go pixel by pixel to analyze and classify suitable habitat based on training data. 

## Results

Current

| Model | Accuracy | F1 Score |
| --- | --- | --- |
| RF | 0.92 | 0.93 |
| KNN | 0.83 | 0.85 |
| SVM | 0.58 | 0.71 |
| LogR | 0.52 | 0 |

Future 1

| Model | Accuracy | F1 Score |
| --- | --- | --- |
| RF | 0.85 | 0.87 |
| KNN | 0.88 | 0.9 |
| SVM | 0.42 | 0.59 |
| LogR | 0.85 | 0.87 |

Future 2

| Model | Accuracy | F1 Score |
| --- | --- | --- |
| RF | 0.93 | 0.94 |
| KNN | 0.9 | 0.92 |
| SVM | 0.65 | 0.74 |
| LogR | 0.81 | 0.83 |

Across the board, RandomForest can be seen to be the best model to classify suitable habitat for the mousedeer. This is closely followed by KNN. Interestingly LogR performs well for the future scenarios though I suspect this may have to do with the resolution of the raster data. 

Feature importance shows that for current predictions – bio16 is of value
For future 1 – bio10
For future 2 – bio12

BIO16 Precipitation of Wettest Quarter
BIO10 Mean Temperature of Warmest Quarter
BIO12 Annual Precipitation

Note:
According to the documentation - Aggregation was done for monthly precipitation, minimum, mean and maximum temperature. The Bioclimatic variables were calculated from these aggregated data.

Essentially the variables are derived from the tmean, tmin, tmax and prec.

## Conclusions and recommendations

In retrospect, given I did not use landcover data, it would have been interesting to see how the occurrence of the mangrove pit viper might change with fluctuating climatic variables. Especially so because it is a thermoconforming animal unlike the thermoregulating mousedeer. 

Also, after reading up on them, they do not have a thermal niche, or in other words, they have the potential to adapt to rising temperatures. However, what would then be interesting to see is how bioclimatic factors may impact vegetation in the region. Mousedeers are highly reliant on dense forests and fruiting vegetation. There is very little study done on how climate change will affect our plant diversity and resilience. 

Generally speaking, climate change affects the fruits in its various growth stages. It can delay maturity, delay ripening, reap poor quality fruit etc. All important considerations.

Given more time, it would be useful to input land cover data to better understand habitat use and how that might change over the coming years.

Neural networks could be used in future iterations of this project especially with rare, understudied species

## References

[CMIP6: the next generation of climate models explained](https://www.carbonbrief.org/cmip6-the-next-generation-of-climate-models-explained)

[Choosing the right CMIP6](https://iwaponline.com/jwcc/article/11/3/577/74144/Review-of-approaches-for-selection-and-ensembling)

[Novel Three-Step Pseudo-Absence Selection Technique for Improved Species Distribution Modelling](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0071218)

[Activity and Habitat Use of Lesser Mouse-Deer (Tragulus javanicus)](https://academic.oup.com/jmammal/article/84/1/234/2373235)

[A Robust Prediction Model for Species Distribution Using Bagging Ensembles with Deep Neural Networks](https://www.mdpi.com/2072-4292/13/8/1495/htm#app1-remotesensing-13-01495)

[Land Cover Prediction from Satellite Imagery Using Machine Learning Techniques](https://sci-hub.mksa.top/10.1109/icicct.2018.8473241)

[How To: Land-Use-Land-Cover Prediction for Slovenia](https://eo-learn.readthedocs.io/en/latest/examples/land-cover-map/SI_LULC_pipeline.html)

[Land Cover Classification of Satellite Imagery using Python](https://towardsdatascience.com/land-cover-classification-in-satellite-imagery-using-python-ae39dbf2929)

[Landcover Analysis using Python and ArcGIS](https://towardsdatascience.com/landcover-analysis-using-python-and-arcgis-74223a2c508a)

[Species Distribution Modeling for Machine Learning Practitioners: A Review](https://dl.acm.org/doi/fullHtml/10.1145/3460112.3471966)

[Why choose Random Forest to predict rare species distribution with few samples in large undersampled areas? Three Asian crane species models provide supporting evidence](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5237372/)

[Machine Learning for Conservation Planning in a Changing Climate](https://www.mdpi.com/2071-1050/12/18/7657/htm#B34-sustainability-12-07657)

[Using machine learning to predict habitat suitability of sloth bears at multiple spatial scales](https://ecologicalprocesses.springeropen.com/articles/10.1186/s13717-021-00323-3)

[Selecting pseudo-absences for species distribution models: how, where and how many?](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjgrPud9-P0AhW6SWwGHZOiCYcQFnoECBYQAQ&url=https%3A%2F%2Fwww.researchgate.net%2Fpublication%2F229149956_Selecting_Pseudo-Absences_for_Species_Distribution_Models_How_Where_and_How_Many&usg=AOvVaw0xYyewFBZJeP6hwL5oSaye)

[DEVELOPMENT ASSESSMENT OF THE SINGAPORE LAND: A GIS SPATIAL- TEMPORAL APPROACH BASED ON LAND COVER ANALYSIS](http://technicalgeography.org/index.php/latest-issue-2-2019/291-06_nistor)

[Comparison of five modelling techniques to predict the spatial distribution and abundance of seabirds](https://www.sciencedirect.com/science/article/abs/pii/S0006320711004319)

[A century of anthropogenic environmental change in tropical Asia: Multi-proxy palaeolimnological evidence from Singapore’s Central Catchment](https://journals.sagepub.com/doi/full/10.1177/0959683619875808)

[Correcting for bias in distribution modelling for rare species using citizen science data](https://onlinelibrary.wiley.com/doi/full/10.1111/ddi.12698)

[Modeling and Prediction of Land Use Land Cover Change Dynamics Based on Land Change Modeler (LCM) in Nashe Watershed, Upper Blue Nile Basin, Ethiopia](https://www.mdpi.com/2071-1050/13/7/3740)

[Machine Learning Techniques for Modelling Short Term Land-Use Change](https://www.mdpi.com/2220-9964/6/12/387)

[Machine Learning Algorithms for Urban Land Use Planning: A Review](https://www.mdpi.com/2413-8851/5/3/68)

