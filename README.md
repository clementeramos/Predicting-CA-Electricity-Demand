# Predicting-CA-Electricity-Demand
<h1>Random Forests and GLM Modeling</h1>
<h1>Senior Capstone Research Project (Synthesis)</h1>

<h2>Description</h2>

- <b>Research Question:</b> How can historical weather data predict electricity demand in California?

<h2>Background</h2>

The weather data comes from the National Oceanic and Atmospheric Administration (NOAA), a government agency that houses historical weather data globally. The data was retrieved from an API request, using a token provided by the National Centers for Environmental Information (NCEI) website. I pulled a year’s worth of daily historical weather data from particular weather stations in California, as well as downloaded hourly electricity data from 2021 from the US EIA website. I chose 7 stations from diverse geographical locations in California, including: Palm Springs, San Diego, San Francisco, Fresno, Santa Maria, Los Angeles, and Redding, to make sure we represent the diversity of weather patterns across the state in our sample. From the API path, I saved the results as a dataframe, converting it into a CSV file to avoid having to pull the data live each time. Then, I conducted exploratory data analysis and data cleaning to explore the relationships between variables. This dataset was merged with hourly electricity load data in the state of California by coinciding dates. 
 
Subsequently, the granularity for the EIA dataset was originally hourly electricity load information. However, a roll up was performed by aggregating the hourly data into daily data, so, ultimately, each row represented the average electricity load in MegaWatts per day in 2021, according to the California Independent System Operator – which oversees the bulk of electric power systems such as transmission lines and the electricity market based on its member utility companies actual load (real-time electricity demand) such as PG&E, San Diego Gas & Electric, and so on. The target variable “CAISO Total Actual Load (MW)" represents information about electricity load demand in California, given aggregated electricity demand with the average across all utility companies in the state. Opting for the mean weather data to make predictions on the average consumption load was the direction we took to construct the design matrix. By pivoting the weather data, we computed average values for the entire state via the max temperature, windspeed, minimum temperature, and precipitation, thus aggregating away the individual weather station data.

A concern here might be selection bias because the particular weather stations picked were mainly from larger cities, and the noise that our model picks up on might not be representative of different regions with smaller, rural cities. Additionally, the concern for measurement error arises due to potential equipment failure on a station’s end, since some of the values were missing for the wind speed for a few consecutive days. The only column with missing values in the original data was the wind speed, which constituted less than 10% of the data, so I used linear interpolation to impute the missing values. This method of imputation is best because the data is time-dependent, so grabbing values from neighboring time points is more appropriate than simply using summary statistics like the mean. 

<h2>Environments Used </h2>

- <b>Jupyter Notebook</b>
- <b>Google Colab</b> 

<h2>Walk-through:</h2>

<p align="center">
Loading the data:
 <br/>
 
<img src="https://i.imgur.com/uYJjmT8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/FTZ7mFh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/O1rzrbw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/owy1sUv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ODNtMGC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<br />
<br />

<p align="center">
Data Wrangling:  <br/>
 
<img src="https://i.imgur.com/3QBym8n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ixD36Cf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/AV1Jk1G.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<br />
<br />

<p align="center">
EDA: <br/>
 
<img src="https://i.imgur.com/CxMH7aR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/pl3avET.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

<p align="center">
Results:  <br/>
 
<img src="https://i.imgur.com/FRuQ7e3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/YTjzJga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/T7HVPwH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/UzDUjI4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<br />
<br />

<h2>Discussion</h2>

For the Random Forest model, the model yielded an R-squared value of 0.883, suggesting that it accounts for 88.3% of the variation in unseen data; this indicates that the model mostly captures how demand for electricity varies and a robust predictive power with the five weather attributes alone. Also, the RMSE is 1504.22 MW, so our predictions are off by this amount on average, since this is the expected error on our test data.  Essentially, when our model tries to predict the daily consumption in California, it is off by this much, which can power hundreds of thousands of homes, so for our purposes of making a general forecast, it is not bad, but for higher-stakes decisions, it would be advisable to improve the model.

The quantity certification shows that the first GLM model is a close contender for the best model, considering it strikes a balance between LLH, AIC, and R-squared. The AIC for the first model is the smallest at 6356, while the other two have larger values. All of its coefficients are statistically significant because the p-values are less than .01. This allows us to see that it offers a balance between overfitting, goodness-of-fit, and statistical significance. 

Overall, the random forests are more reliable and a better model because they had a higher R-squared of 88.3% and a decent performance on the test set with an RMSE of 1504.22 megawatts. It also accounts for nonlinear relationships and avoids overfitting, leading to better performance on a test set.

<h2>Conclusion</h2>

Ultimately, the key results mirror the findings of the Berkeley National Lawrence Laboratory study, such that extreme weather causes a significant impact on the demand for electricity. Our model also shows the reality that the grid may not be able to support the increase in demand, as in the study. They used a similar approach to predict electricity demand using random forests. It would be helpful to train a model using more time series data to perhaps generalize this to the larger trend over time in state consumption.


