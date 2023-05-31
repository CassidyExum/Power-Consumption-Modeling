# Power Consumption Modeling

## 1. Business Understanding

Due to climate change, increased energy demands, and the need for renewable energy, power consumption is an important data point to research for all major cities. The city government of Tetuoan, Morocco has tasked me with researching the power consumption of three different zones within the city. The use of this analysis is broad and to be determined by the city officials later. First, I hope to determine what feature supplied in the dataset, if any, is the driving factor and most correlated with the power consumption of each zone. Secondly, I'd like to report simple and understandable time-series analysis to the officials, things like if the series is stationary, what the moving average and standard deviation are, etc. Lastly, I'd to leave the officials with forecast ready models and next steps for further research and analysis. 

## 2. Data Understanding

The data was recorded by Professors out of Abdelmalek Essaadi University, in Morocco. The all features were recorded every 10 minutes from January 1st 2017 to December 30th 2017. There are 3 potential Y values, and 6 features. There are over 52,000 datapoints due to the frequency of recording. For the purpose of simplicity I'll be focusing my modeling onto Zone 1, but the best model and other analysis techniques used can be applied to the other zones as well.

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/eaa5cc64-0076-4aac-9149-c524fc50ce00)

## 3. Data Preperation

The data was cleaned and prepared prior to releasing it. I adjusted a few column names and converted the data to a proper time-series for modeling. I did normalized the features, and look into the correlation of each feature to check for anything of concern.

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/4bac023d-9e94-455f-ae71-39bb158e5a0c)

## 4. Modeling

The task is to obtain a quantitative value, so I will be using regression models. I generated a list of 11 models that can quickly be trained without much effort, these models are not time-series specific and will be used as a baseline for comparison purposes. I then used 2 time-series specific models, SARIMA and ARIMA. The time series models should perform better than the baseline models. 

The baseline models were trained using training and test splits, and cross validated to obtain a mean squared error. Mean Squared Error is a common evaluation metric for regression tasks, the lower the MSE the better. 

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/e3bac8db-928b-4d12-af05-aecd2a313093)

From the image above its clear that XGBoost Regressor, Random Forest Regressor, and Extra Trees Regressor are the top three models. I will use these for grid search later.

For the time-series specific models, the 10-minute, 52,000+ data points became too computing intensive. I decided to downsample to hourly and use the aggregated mean for the time-series models. I'm not too concerned about losing accuracy due to this as a large portion of the data points were likely redundant because of the frequency of recording. The hourly resample is much easier for the algorithms to use. The SARIMA model was able to achieve an MSE of 562 when predicting on just the final month of the dataset, and an MSE of 965.16 when predictiing accross the entire dataset. The ARIMA model performed better than the baseline models with an MSE of 1971, but that is still worse than the SARIMA model.

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/487cdf50-95a4-47e6-963b-7228d2600e27)

In the image above you can see the SARIMA model's One-step ahead forecase is incredibly close to the actual values.

## 5. Evaluation

The SARIMA 

## 6. Deployment

Deployment for this project is just generating the presentation and report. 

### Refrences

