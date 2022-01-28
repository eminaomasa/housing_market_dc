# Forecasting the 2022 housing market in the Washington DC metro area 
Author: Emiko Naomasa

## Business Understanding:

In 2021, the U.S. housing market marked the highest housing sales in the past 15 years. Very low mortgage loan interest rates and change in people's work styles during the pandemic boosted housing demand all across the U.S.; as a consequence, the housing inventory in the market has dropped sharply, and housing prices have been rising. This trend of supply shortage and ascending house prices is expected to continue for the rest of 2022. 

In this project, I develop a forecasting model of the 2022 housing prices for the Washington, DC, metropolitan area. A targeting stakeholder is a real estate developer who wants to take advantage of the current market and start new housing development in an area that promises a high return. My model will help a developer decide on places for the next housing project.  

## Deliverables: 

![image](https://user-images.githubusercontent.com/38669459/151544970-cc989a08-96ae-4e77-a9ba-72bdb4a35f8e.png)


I produced (1) a housing price forecasting model and (2) a next up-and-coming neighborhoods forecasting model. First, as a housing price forecast, I developed a model that returns a unique price forecast for each zip code. Because remote work has become the new normal during the pandemic, people can now choose a place for themselves, not commuting to work. Then, the hottest spot for real estate investment is no longer larger cities; rather, investment has spread across regions. Even in a same county, housing prices move very differently. Thus, I built a time series model for each zip code and provided a unique forecast. The second model predicts the next up-and-coming neighborhoods for 2022. Most homebuyers in today's market are people from large cities. Their preference is slight to a mid-sized towns with economic vibrancy. These two models will be used together for a more granular view of the 2022 housing market. First, I use the first model to forecast zip codes with the highest increases in housing prices in 2022. Then, I use the second model to filter the zip codes in the up and coming neighborhoods. 


## Data: 

- Housing Prices [Realtor.com](https://www.realtor.com/research/data/): This monthly data covers July 2016 to December 2021. I use median listing price data as housing price data and the number of active listings to measure housing supply in a given area. My sample includes only single-family homes. After I eliminated zip codes with fewer than 20 listings, my sample includes 191 zip codes in the Washington, DC, metro area. 

- [USPS Change of Address Data](https://www.google.com/covid19/mobility/index.html?hl=en): This monthly data recodes the inflow and outflow of people from the zip codes. 

- [Google's Community Mobility Data](https://www.google.com/covid19/mobility/index.html?hl=en): This is a daily measurement of time spent and the number of people in different types of places, including parks, retail and recreation, grocery, transit locations, and workplaces. I combine mobility data from retail, recreation, and grocery to measure vibrancy in the commercial place of a county. This daily data is available from January 2020.   

-	Small Merchants' Revenue Data from [Economic Tracker](https://tracktherecovery.org/): This is weekly revenue data for small merchants at a county level. Data is in a year-over-year change. It covers from January 2019 to January 2020. Having active small merchants is a good indicator of a robust local economy. 

-	COVID-19 daily new cases from [Economic Tracker](https://tracktherecovery.org/): This is daily data available at the county level. 

For data cleaning and EDA, please read my notebooks. 
- [Data_Cleaning_EDA1_Housing_Price](https://github.com/eminaomasa/housing_market_dc/blob/main/Data_Cleaning_EDA1_Housing_Price.ipynb)
- [Data_Cleaning_EDA2_Neighborhood_Economy](https://github.com/eminaomasa/housing_market_dc/blob/main/Data_Cleaning_EDA2_Neighborhood_Economy.ipynb).

## Modeling: 

Housing Price Forecasting: I developed a time-series forecasting model for each zip code in the Washington, DC, metropolitan area and derived the price forecast for January to December 2022. I used a multivariate autoregression (VAR) model with three time series: (1)housing price times series (my main focus), (2) number of active listings in an area, and (3) population net inflow in an area. In a competitive market, housing prices in each zip code are set by supply and demand in the area. The number of active listings indicates housing supply in an area, and net population inflow captures the size of the demand in an area. As I show in my EDA notebook, these three time series relate to each other. To choose the best parameter, I ran a grid search. 

Neighborhood Economy Forecasting: I developed two different time-series forecasting models for each county in the DC metro area. One forecasts people's mobility in retail, recreational stores, and groceries. This forecast provides insight into which county will have more vibrant stores and restaurants. This will be one indicator of which counties will be more populated locations for 2022, and therefore excellent places to develop new housing complexes. Another forecasts business revenue changes for small merchants in a county. Having many small merchants (10 or fewer employees) doing good business indicates strong economic activity in a neighborhood. I use seasonal ARIMA with an exogenous variable (SARIMAX) with daily COVID-19 cases as an exogenous variable for both time series. A surge in new cases of COVID-19 in a region influences the time people spend in commercial stores, and therefore impacts small merchants’ businesses. By running a grid search for each zip code, I pick the best set of parameters. 

For modeling, please read my notebooks.
- [Model1_VAR_HousingPrice](https://github.com/eminaomasa/housing_market_dc/blob/main/Model1_VAR_HousingPrice.ipynb)
- [Model2_SARIMA_MerchantRevenue](https://github.com/eminaomasa/housing_market_dc/blob/main/Model2_SARIMA_MerchantRevenue.ipynb)
- [Model3_SARIMA_GoogleMobility](https://github.com/eminaomasa/housing_market_dc/blob/main/Model3_SARIMA_GoogleMobility.ipynb)


## Model Validation: 
I use the root mean squared error (RMSE) to measure a model's prediction accuracy. I compare the VAR of the housing price model and SARIMAX of the economic forecasting model with naïve time-series modeling. The naïve time-series model is a simple shift of time series by one period.   
 
Housing Price Forecasting: RMSE by VAR model varies widely across zip codes (from 3,641 to 545,165). Median RMSE is 32,300. Given that median housing prices in this region are around 600,000, the forecast of this model will be off by 6%, which is not a bad number. However, prediction performance varies widely by zip code. Compared with the RMSE of the naïve model ( 51,124) , the VA model shows a significantly lower RMSE. Thus, my VAR model improves prediction performance. 

Local Economy Forecasting:

- Small merchant revenue:  The RMSE of the SARIMAX model takes from 0.05 to 0.5, with the median at 0.13. Since small merchant revenue changes range from -0.555 to 0.141, the prediction is off by about 10% of the size of the original data. RMSE of the naïve  model is 0.12. This means that the SARIMAX forecast is no different from the naïve model forecast. 

- Google mobility in the commercial area:  The RMSE of the SARIMAX model varies from 162 to 394, with a median of 285. As the mobility index ranges from -669.0 to 686.0, the prediction is off about 20-25%. Compared with the RMSE of the naïve model (200), the SARIMAX model shows a larger RMSE. This means that the prediction accuracy of my SARIMAX model is no better than the naïve model's prediction.


## Findings:
Figure 1 plots the top 20 zip codes with largest expected housing price increase in 2022. The hotspots in 2022 are spread across Virginia and Maryland. 
Figure 2A and 2B plot a list of counties with robust neighborhood economies. Counties in the middle of the metro, such as Washington DC or Arlington, are not on this list. Instead, a county distant from the city center shows strong local economy in terms of small businesses and the popularity of commercial areas. Especially, Frederick, a northern county on the map, seems like a promising county for local economic growth.
Using these two figures, I filter the top 10 zip codes for the next real estate development project in 2022. Top 10 zip code recommendations are in Figure 3. 

Figure 1. Top 20 zip codes for the largest housing price increases in 2022. 


Figure 2 A: Top 5 counties in terms of an increase in small merchants’ revenues in 2022.

Figure 2B: Top 5 counties in terms of an increase in visitors to commercial areas. 

Figure 3: Top 10 recommended zip codes for real estate development in 2022. 


For forecasting analysis, please read my notebooks.
- [PostAnalysis1_HousingPrice](https://github.com/eminaomasa/housing_market_dc/blob/main/PostAnalysis1_HousingPrice.ipynb)
- [PostAnalysis2_MerchantRevenue](https://github.com/eminaomasa/housing_market_dc/blob/main/PostAnalysis2_MerchantRevenue.ipynb)
- [PostAnalysis3_GoogleMobility](https://github.com/eminaomasa/housing_market_dc/blob/main/PostAnalysis3_GoogleMobility.ipynb)


## Next steps: 
- Incorporate the mortgage loan interest rate. The FED announced a possible interest rate hike in March. Depending on how much the interest rate increases, US housing demand could decrease. So by analyzing the housing demand response to the interest rate in DC’s neighboring areas, my model can provide more in-depth analysis for real estate developers.

-	Use time-series data with longer coverage. The time-series data used in this model is relatively short. Housing data is monthly and covers a period beginning in July 2016. This means that I had only 65 data points to build a model for each zip code. 

-	Try a deep learning model: The time-series model has to clear many strict assumptions, and the model is limited to a linear or multiplicative relationship. The advanced machine learning model will create more flexibility in modeling and improve the accuracy of the model.

-	Expand the model to other metropolitan regions. The base model and programming codes are ready. I can easily apply these time series models to other metropolitan areas.  

## For More Information 
Please review my full analysis in notebooks or my presentaion [slides](https://github.com/eminaomasa/housing_market_dc/blob/main/PPTs.pdf).
For any additional questions, please contact: [Emiko Naomasa](https://www.linkedin.com/in/emiko-n-58782158/) 

## Repository Structure

```
├── README.md                           
├── Data_cleaning_GeoIDs.ipynb                      <- Data Celaning Notebook for create GEO IDs
├── Data_Cleaning_EDA1_Housing_Price.ipynb          <- Data Celaning and EDA Notebook for Model 1
├── Data_Cleaning_EDA2_Neighborhood_Economy.ipynb   <- Data Celaning and EDA Notebook for Model 2 
├── Model1_VAR_HousingPrice.ipynb                   <- Modeling Notebook for Model 1
├── Model2_SARIMA_MerchantRevenue.ipynb             <- Modeling Notebook for Model 2 (Small Merchant's Revenue)
├── Model3_SARIMA_GoogleMobility.ipynb              <- Modeling Notebook for Model 2 (Google Mobility)
├── PostAnalysis1_HousingPrice.ipynb                <- Post Analysis Notebook for Model 1
├── PostAnalysis2_MerchantRevenue.ipynb             <- Post Analysis Notebook for Model 2 (Small Merchant's Revenue)
├── PostAnalysis3_GoogleMobility.ipynb              <- Post Analysis Notebook for Model 2 (Google Mobility)
├── PPTs.pdf                                        <- PDF version of project presentation
└── Data                                            <- Both sourced externally and generated from code
                           
```  
Note: Large or sensitive files are listed in .gitignore and not pushed to GitHub. 
 
