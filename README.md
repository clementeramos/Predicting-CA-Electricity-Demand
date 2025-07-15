# Predicting-CA-Electricity-Demand
<h1>Estimating the Probability of Infection For COVID-19 in Symptomatic and Non-symptomatic Individuals</h1>

<h2>Description</h2>
Research Question: How can historical weather data predict electricity demand in California?

<h2>Background</h2>

The weather data comes from the National Oceanic and Atmospheric Administration (NOAA), a government agency that houses historical weather data globally. The data was retrieved from an API request, using a token provided by the National Centers for Environmental Information from the NOAA website. We pulled a year’s worth of daily historical weather data from particular weather stations in California, as well as downloaded hourly electricity data from 2021 from the US EIA website. I chose 7 stations from diverse geographical locations in California, including: Palm Springs, San Diego, San Francisco, Fresno, Santa Maria, Los Angeles, and Redding, to make sure we represent the diversity of weather patterns across the state in our sample. From the API path, I saved the results as a dataframe, converting it into a CSV file to avoid having to pull the data live each time. Then, I conducted exploratory data analysis and data cleaning to explore the relationships between variables. This dataset was merged with hourly electricity load data in the state of California by coinciding dates. 
 
Subsequently, the granularity for the EIA dataset was originally hourly electricity load information. However, a roll up was performed by aggregating the hourly data into daily data, so, ultimately, each row represented the average electricity load in MegaWatts per day in 2021, according to the California Independent System Operator – which oversees the bulk of electric power systems such as transmission lines and the electricity market based on its member utility companies actual load (real-time electricity demand) such as PG&E, San Diego Gas & Electric, and so on. The target variable “CAISO Total Actual Load (MW)" represents information about electricity load demand in California, given aggregated electricity demand with the average across all utility companies in the state. Opting for the mean weather data to make predictions on the average consumption load was the direction we took to construct the design matrix. By pivoting the weather data, we computed average values for the entire state via the max temperature, windspeed, minimum temperature, and precipitation, thus aggregating away the individual weather station data.

A concern here might be selection bias because the particular weather stations picked were mainly from larger cities, and the noise that our model picks up on might not be representative of different regions with smaller, rural cities. Additionally, the concern for measurement error arises due to potential equipment failure on a station’s end, since some of the values were missing for the wind speed for a few consecutive days. The only column with missing values in the original data was the wind speed, which constituted less than 10% of the data, so I used linear interpolation to impute the missing values. This method of imputation is best because the data is time-dependent, so grabbing values from neighboring time points is more appropriate than simply using summary statistics like the mean. Some additional data preprocessing steps were making sure the column we merged on (date) was of the same type in both datasets, as well as making sure the granularity aligned concerning the weather data and the electricity demand.


<h2>Environments Used </h2>

- <b>Jupyter Notebook</b> 

<h2>Walk-through:</h2>

<p align="center">
Create a Bayesian hierarchical model: <br/>
<img src="https://i.imgur.com/b0sXwLW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Compute a trivial estimate of the true posterior probability:  <br/>
<img src="https://i.imgur.com/4DAAu7S.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Approximate the empirical distribution of SAR given the observed data: <br/>
<img src="https://i.imgur.com/NG3kfLt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/lTYhGRG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/PlauRyq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Compare the theoretical and empirical distribution w/ a weak prior:  <br/>
<img src="https://i.imgur.com/TcDHUoa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Compare the distributions using a strong prior:  <br/>
<img src="https://i.imgur.com/MPAWBN4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Hierarchical Model Accounting For Asymptomatic Individuals:  <br/>
<img src="https://i.imgur.com/TD06DeK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
The asymptomatic rate for COVID-19 falls between 0.18 and 0.35; it is unlikely that the posterior distribution of infection has a large fraction of asymptomatic cases:  <br/>
<img src="https://i.imgur.com/T1EZVnY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/NpQPtDu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
