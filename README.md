# Power Consumption Modeling

## 1. Business Understanding

Due to climate change, increased energy demands, and the need for renewable energy, power consumption is an important data point to research for all major cities. The city government of Tetouan, Morocco has tasked me with researching the power consumption of three different zones within the city. The stakeholders goal is to both reduce power consumption, and ensure a robust system from the production of the energy to the consumer. First, I hope to determine what feature supplied in the dataset, if any, is the driving factor and most correlated with the power consumption of each zone. Secondly, I'd like to report simple and understandable time-series analysis to the officials, things like if the series is stationary, what the moving average and standard deviation are, etc. Lastly, I'd like to leave the officials with forecast ready models and next steps for further research and analysis. 

## 2. Data Understanding

The data was recorded by Professors out of Abdelmalek Essaadi University, in Morocco. All of the features were recorded every 10 minutes from January 1st 2017 to December 30th 2017. There are 3 potential Y values, and 6 features. There are more than 52,000 data points due to the frequency of recording. For the purpose of simplicity I'll be focusing my modeling on Zone 1, but the best model and other analysis techniques used can be applied to the other zones as well.

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/eaa5cc64-0076-4aac-9149-c524fc50ce00)

The above image is the mean, standard deviation, and other important statistics of the dataset. With more than 54,000 rows of data, learners should be able to produce decent results, but it may be time consuming. Prior to any modeling I hypothesized that temperature would be important, it ranges from 3.2 degrees C (37.76 degrees F) to 40 degrees C (104.018 degrees F) meaning it's quite hot there and AC could be the driving factor. The humidity is another feature that is generally high (average of 68%).

## 3. Data Preparation

The data was cleaned and prepared prior to releasing it. I adjusted a few column names and converted the data to a proper time-series for modeling. I normalized the features, and looked into the correlation of each feature to check for anything of concern.

![image](https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/4bac023d-9e94-455f-ae71-39bb158e5a0c)

The above image is a correlation matrix. Temperature is the most correlated feature with zone power consumption.

## 4. Modeling

The task is to obtain a quantitative value, so I will be using regression models. I generated a list of 11 models that can quickly be trained without much effort, these models are not time-series specific and will be used as a baseline for comparison purposes. I then used 2 time-series specific models, SARIMA and ARIMA. The time series models should perform better than the baseline models. 

The baseline models were trained using training and test splits, and cross validated to obtain a Mean Squared Error (MSE). MSE is a common evaluation metric for regression tasks, the lower the MSE the better. 

<img width="917" alt="Screen Shot 2023-05-31 at 6 32 19 PM" src="https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/53670062-60b1-4c7e-af9a-28f81e3db86f">

From the image above its clear that the models farthest to the right (XGBoost Regressor, Random Forest Regressor, and Extra Trees Regressor), are the top three models. I will use these for grid search later.

For the time-series specific models, the 10-minute, 52,000+ data points became too computing intensive. I decided to downsample to hourly and use the aggregated mean for the time-series models. I'm not concerned about losing accuracy due to this, as a large portion of the data points were likely redundant because of the frequency of recording. The hourly resample is much easier for the algorithms to use. The SARIMA model was able to achieve an MSE of 562 when predicting on just the final month of the dataset, and an MSE of 965.16 when predicting across the entire dataset. The ARIMA model performed better than the baseline models with an MSE of 1971, but that is still worse than the SARIMA model.

<img width="906" alt="Screen Shot 2023-06-05 at 5 46 53 PM" src="https://github.com/CassidyExum/Power-Consumption-Modeling/assets/104473048/54f9f1f9-8376-46f9-b8d9-043a0ff765c8">

In the image above you can see the SARIMA model's One-step ahead forecast is incredibly close to the actual values. This plot is only showing November and December, with November being the true values and December being both true and forecasted values.

The blue on the left of the plot shows the observed or true value of the power consumption in zone 1, the orange shows the One-Step ahead Forecasted value. And the green in there is a highlighted region between the two values. So the reason that plot in december looks so colorful is because the forecasted value is so close to the observed value that the colors almost blend together

## 5. Evaluation

The SARIMA model was the best performing model with an MSE of 965.16. The SARIMA model predictions closely followed the actual values from the data. In order to obtain more valuable information from this dataset, I believe introducing population / demographic statistics for the three zones would allow us to draw more interesting conclusions. Regardless, the following conclusions are for consideration of the city officials.

1. Temperature is the most correlated feature. As temperature changes, power consumption follows. This makes logical sense because of air conditioning / heating. If your goal is to reduce power consumption, consider implementing limits on AC / heat usage during moderate days to conserve energy for more extreme days. If your goal is to understand when power consumption is at its greatest, consider safeguards and more robust facilities that can handle increased loads in more extreme weather (i.e. summer and winter).
2. Consider further research with demographics data. There are a lot of factors that will drive power consumption, housing and population for example. If this dataset could be compared with population data or zone type (i.e. residential, industrial, or commercial), conclusions like the impact of population or zone type on power consumption could be made.
3. SARIMA Models worked the best and the SARIMA model I trained above provided an error of only 965.16, which is within 3% of the observed value. That is quite accurate and can be used for forecasting power consumption in the future.

## 6. Deployment

Deployment for this project is generating the presentation and report. 

## 7. References

Dataset Citation:

Salam, A., & El Hibaoui, A. (2018, December). Comparison of Machine Learning Algorithms for the Power Consumption Prediction:-Case Study of Tetouan cityâ€“. In 2018 6th International Renewable and Sustainable Energy Conference (IRSEC) (pp. 1-5). IEEE.â€

Notebook resources:

https://github.com/tatsath/fin-ml/blob/master/Chapter%205%20-%20Sup.%20Learning%20-%20Regression%20and%20Time%20Series%20models/Regression-MasterTemplate.ipynb

https://github.com/learn-co-curriculum/dsc-sarima-models-lab/blob/solution/index.ipynb

https://towardsdatascience.com/machine-learning-part-19-time-series-and-autoregressive-integrated-moving-average-model-arima-c1005347b0d7

## Repository Structure

```
.
└── Power-Consumption-Modeling
    ├── Data
    │   └── Data.csv
    ├── .gitignore
    ├── Notebook.pdf
    ├── Power Consumption Modeling.ipynb
    ├── Presentation.pdf
    └── README.md
