## MODELING AND FORECASTING U.S. INFLATION FROM 1972-2022
#### PROBLEM STATEMENT:

The aim of this project is to model inflation in the United States using various economic indicators from Jan 1972 to Dec 2022 and forecast inflation from Jan 2023 to Jan 2024. This is a univariate time series analysis and forecasting. I have converted Inflation Rate into time-series to forecast it.

#### OVERVIEW:

Importing, Cleaning and Merging Data
Exploratory Data Analysis(EDA)
Data Modeling
Final Model Selection and Ensembling models
Forecasting results

#### DATASET:

Economic Indicators are gathered from various websites like https://fred.stlouisfed.org/searchresults/?st=gold, 
https://beta.bls.gov/dataQuery/search and https://www.gold.org/goldhub/data/gold-prices.
 
Nine datasets gathered are only for the united states,
1. Monthly Inflation rates
2. Federal Funds
3. Unemployment rates
4. Consumer sentiment prices
5. All commodities prices
6. Money supply rates
7. Oil prices
8. Gold prices
9. Mortgage Rates

After gathering the datasets, I have cleaned the datasets by getting only the required columns for the project, removing null values if required, filling missing values for few columns, renaming columns for better understandability and finally merging all datasets using Year, month and Date columns.

#### EDA:

From the data visualizations, we can see that there is a drastic rise in inflation rate till mid of 2022 and now we can see that inflation rate is decreased a bit in the past few months.

![image](https://user-images.githubusercontent.com/58209985/235057051-bafd92ae-75e0-4e20-bd18-09c0b077bfdf.png)

It is also clear from the plots that Federal Funds and Commodities Price have a highly positive correlation and consumer sentiment have a negative relation with Inflation Rate.

![image](https://user-images.githubusercontent.com/58209985/235056946-96170d77-30ef-4c96-aa09-1791c084a171.png)

![image](https://user-images.githubusercontent.com/58209985/235057383-6b579627-a5e2-45bb-a2ad-57859e0e88f8.png)

![image](https://user-images.githubusercontent.com/58209985/235056869-872e3cd6-dd21-4e23-ac3b-0d3f8ef0ed66.png)


#### DATA MODELING:

##### MULTIPLE LINEAR REGRESSION MODEL

I have used multiple linear regression model to model the data but after validating and assessing the model found that all assumptions of linear model have been violated except the linear model being able to fit data well based on linear model forecast. So, because of that I tried improving model performing step wise regression with both forward and backward selection based on AIC. But even after doing this step model is not better than linear model. 

##### Linear model performance metrics:

MAE       MSE      RMSE      MAPE

1 0.6250961 0.6109154 0.7816108 0.2451675

##### Step wise model performance metrics:

MAE       MSE      RMSE      MAPE

1 0.6597868 0.6764271 0.8224519 0.2450378

So, now I want to use NNAR and ARIMA models which are suitable working with autocorrelated data.

##### ARIMA MODELING

I have used different combination of variables to create ARIMA models. 


| Model |      Variables_Used      |   MAE    |  MedAE   |  MaxAE   |    AIC    |    BIC    |
|:-----:|:------------------------:|:--------:|:--------:|:--------:|:---------:|:---------:|
|  13   |       Commodities        | 2.352409 | 2.581968 | 3.872895 | -51.96714 | -21.06146 |
|   4   |           All            | 2.280996 | 2.533534 | 3.831243 | -39.57735 | 35.50710  |
|   5   |       All w/o Oil        | 2.106606 | 2.261766 | 3.560799 | -47.33902 | 10.05724  |
|   3   |           None           |   NaN    |    NA    |   -Inf   | 259.91260 | 304.06360 |
|   2   |         FEDFUNDS         | 2.406897 | 2.427850 | 4.660428 | 261.53800 | 310.10400 |
|   1   | FEDFUNDS and Commodities | 2.335980 | 2.552046 | 3.843022 | -50.10043 | -14.77965 |
|   6   |  UMCSENT and GoldPrice   | 2.168048 | 1.803672 | 5.390516 | 281.74960 | 330.31570 |

From the above summary table of ARIMA models, we can say that model without any external variables did not perform much better than models with external variables. 

##### NNAR MODELING

Similar to ARIMA modeling, used same combination of variables to create NNAR models too.

| Model.. |      Variables.Used      | Mean.Absolute.Error | Median.Absolute.Error | Max.Error  |
|:-------:|:------------------------:|:-------------------:|:---------------------:|:----------:|
| NNAR 1  | Commodities and FEDFUNDS |     1.686139%%      |      1.565136%%       | 3.408511%% |
| NNAR 2  |         FEDFUNDS         |     3.401931%%      |      3.693137%%       | 7.430945%% |
| NNAR 3  |           None           |         NaN         |          NA           |    -Inf    |
| NNAR 4  |           All            |     0.6290642%%     |      0.214221%%       | 3.136537%% |
| NNAR 5  |       All w/o Oil        |     0.7747749%%     |      0.2088606%%      | 3.495451%% |
| NNAR 6  |  UMCSENT and GoldPrice   |     4.487923%%      |      3.773435%%       | 3.495451%% |
| NNAR 7  |       Commodities        |     1.198748%%      |      0.7068593%%      | 3.665603%% |

From the above summary table of NNAR models, we can also say that model without any external variables did not perform much better than models with external variables.

#### ARIMA top models:

![image](https://user-images.githubusercontent.com/58209985/235061578-93e6d784-7afe-4b1f-8b77-93452f1130f7.png)

#### NNAR top models:

![image](https://user-images.githubusercontent.com/58209985/235061686-6bcc78db-66af-4170-840f-f4f03e1d0b13.png)

Compared to ARIMA models and linear model, NNAR models performed very better by all performance metrics used. In both NNAR and ARIMA models, Commodities, FEDFUNDS and all variables without oil performed better. The above NNAR and ARIMA models are sorted based on their performance metrics. The top three best models are NNAR 4, NNAR 5 and NNAR 7. I have ensembled these three models to forecast Inflation rate. 

#### Predictions versus Actuals from Jan 2018 to Dec 2022:

![image](https://user-images.githubusercontent.com/58209985/235063403-2a19bde1-8bf0-4ac6-9e78-b806999a00bc.png)

From this plot we also see that every model predicts a similar movement in inflation, where it is expected to drop off by September 2022. We can see that forecasted inflation values of ensembled model is very close to the actual inflation rate in the United states respective years.

#### Forecasting Results from Jan 2023 to Jan 2024:

![image](https://user-images.githubusercontent.com/58209985/235063542-820fabde-cf59-4092-8d09-7b448ec0d74e.png)

From the above plot, we can see that forecasted inflation rate in Jan 2024 is between 1 and 2.3 which is close to the experts forecast.Inflation has dropped in the year 2023 but with a small ups and downs in the respective months and by the end of this year we can see that there is a drop in inflation.


