# Citi Bike Analysis



NATIONAL UNIVERSITY OF SINGAPORE
SCHOOL OF COMPUTING
 
ACADEMIC YEAR 2017/2018 SEMESTER 2
BT2101 DECISION MAKING METHODS AND TOOLS


CitiBike 2013

Group Members: 
Chay Yi Yang Desmond (A0154965R)
Liu Song Yu (A0155148A)
Jason Yip (A0173053M)
Jerald Koh Kah Leong (A0173746W)






Motivation
The bicycle-sharing system has been around since 1965. It is only until recent years that the Sharing Economy started to change our world and bike-sharing became commonplace. It is interesting to explore insights that we can derive from new business models especially the bike-sharing system which is abundant with data.

Objective
We attempt to analyse 2 years’ worth of rides in the largest bike sharing company in USA, Citi Bikes. It had about 17 million rides from July 13 to July 15 and we aim to provide more business value to the company in our report. At the same time, we also want to provide insights using methods taught in this class namely 1) Panel Data Analysis, 2) Clustering and 3) Time-series Forecasting along with descriptive statistics.

Descriptive Analysis & Quick Insights
Visualizing the data would help us to develop our hypotheses to test and carry out further analyses. It also helps us to develop quick insights. We can see that most of the trips are made by subscribers who are around age 35. 

This finding could be useful to Citi Bike in customizing the promotion of their subscription service to suit the tail-end age groups more and maximize subscription rates.
We also observed how subscribers and non-subscribers affect the hourly distribution of rides. We can see that the frequency of trips made by subscribers peak during near peak hours (8am / 5pm) while the frequency of trips made by non-subscribers are higher during non-peak hours. We can thus deduce that most subscribers use it for commute to work since most of the trips are made by those aged 35 as well.

1. Unsupervised Learning with K-Means Clustering 
It is useful to see if we could derive any interesting associations between variables and unique business insights with unsupervised learning algorithms such as K-Means Clustering. Our first hypothesis is to test if there exists a relationship between trip durations and the riders’ subscription rate / age / start station location.
Data
The data consists of information on the 17 million Citi Bike trips which includes:
Trip Duration (seconds)
Start Time and Date
Stop Time and Date
Start Station Name
End Station Name
Station ID
Station Lat/Long
Bike ID
User Type (Customer = 24-hour pass or 3-day pass user; Subscriber = Annual Member)
Gender (Zero=unknown; 1=male; 2=female)
Year of Birth

Methodology
We performed the clustering with different permutations of variables and found that using the trip duration and gender of the rider allowed us to create large and distinct clusters. In order to decide the optimal number of clusters, we could look at the greatest reduction in SSE (Sum of Squared Errors) for each number of clusters. More commonly known as the Elbow Method, this gives us an optimal amount of 4. 

Figure 1: Plot of Average Total Sum of Square Errors against Different Values of K

Results

Figure 2: K- Means Clustering Means with k=7
The clustering reveals that there are two large groups, group 2 (size = 632882) and group 4 (size = 205168) which we will choose to focus on since they represent the majority of the trips.  The mean of group 2 shows trip duration being 607 seconds (10minutes) and gender being 1.03 while the mean of group 4 shows trip duration being 1797 seconds (30minutes) and gender being 0.81. This indicates that bikers in group 2 tend to take a shorter time to travel and are more likely to be male compared to bikers in group 4.
 
We can test the first part of our hypothesis that there exists a relationship between trip duration and subscription type. In order to confirm the hypothesis, we look at the proportion of customers and subscribers for both groups.
Proportion
Group 2
Group 4
Customers
94973 (0.15%)
76438 (0.37%)
Subscribers
537909 (0.85%)
128730 (0.63%)

Since the proportion of subscribers are higher in group 2 and the rides take a shorter time on average in group 2, we can deduce that subscribers are more likely to make shorter trips. 
Next, we can test the second part of the hypothesis that there might also exist a relationship between trip duration and age. We can get the average birth year of riders in group 2 and group 4.
Group 2: 1975.598 Birth Year, Group 4: 1974.97 Birth Year. Thus, we can deduce that distance cycled might not have any correlations with the biker’s birth year.
Lastly we can test the third part of our hypothesis that there is also a relationship between trip duration and location. We visualize the geographical locations of group 2 and group 4. From the visualization, it is not apparent as to whether the geographical locations have any correlation with the distance travelled as all the locations are scattered around evenly.

Figure 3: Two visualizations of group 2 and group 4. The top visualization calculates the total number of bikers at that geographical location. The bottom visualization depicts the locations of group 2 and group 4. 


Insights
We found that subscribers tend to make shorter trips. We can also see from the bar chart that they ride more on weekdays. This could be due to the fact that most riders who subscribe use it for pragmatic purposes such as transitioning between other transport modes. 

Non-subscribers on the other hand, could possibly take longer trips because they might be tourists and would not want to pay for subscription just to use it for their visit here. It could also mean that there are riders who use them once in a while for leisure on weekends. We can also see from the bar chart that the proportion of non-subscribers increases during the weekends and ridership decreases overall because there are less who use it for their work commute.

This could help Citi Bikes to understand their consumer segments and perhaps focus on targeting their advertising efforts differently.

2. Exploring the effects on duration of Citi Bike trips

Our second hypothesis is carried out to explore the significance and effects of other variables on the duration of Citi Bike trips. We intend to use the riders’ age and weather data for that day to predict how long Citi Bike trips will take.

Processing the Data
We included the previously-mentioned bike data and the weather data of New York Central Park which included the daily measurements for multiple metrics from the start of July 2013 to the end of June 2015. We combined both datasets and kept only the meaningful features. We also took averages by Start Station IDs across months, converted the scale of Duration from seconds to minutes, filtered out only the data from Subscribers, converted riders’ Birth Date to Age and filled missing data with previous-known values since it is a time series.


Dependent Variable (DV) and Independent Variables (IV)
Our DV is the average duration in minutes per trip. Our IVs include average rider’s age, average daily precipitation (inches), average daily snow depth (inches), average daily maximum temperature, average daily wind speed (m/s).

Methodology
Since we could segment the data into cross-sectional units over a period of time. We decided to do a Panel Data Analysis. It is also known as a cross-sectional time series data where the data is usually a small number of observations over time on usually a large number of cross-sectional units. Since we could observe 333 unique Bike Station ID monthly over a period of 24 months, the model is appropriate for the size. 

We can observe that there is heterogeneity in the data across months and similarly across stations though the plot would be too large to fit it in.

With Panel Data, we can account for unobserved factors such as how steep the location of the station is (whether it is on top of a hill), how crowded the area around the station is and how many bents that are nearby which all do not generally change over long period of time. These are factors that could affect the ease of cycling around the area and affect the duration of trips. 
We can first compare between the Fixed Effect (FE) and Random Effect (RE) models by regressing Duration on Age, Precipitation, Snow Depth, Max Temperature and Wind Speed.


Hausman Test
data:  Duration ~ Age + Precipitation + SnowDepth + MaxTemp + WindSpeed
chisq = 1.259, df = 5, p-value = 0.9391
alternative hypothesis: one model is inconsistent

Using the Hausman Test,  since p-value = 0.9391 > 0.05, we fail to reject the null and can conclude that the random effects model is a better fit. A random model should be used when the omitted variables are uncorrelated with the independent variables. It will provide unbiased estimates. The random effects model helps to control for any autocorrelation in the error term due to μi being fixed over time.


Duration
WindSpeed
-0.238**
Age
-0.120***


(0.097)


(0.040)
Constant
16.507***
Precipitation
2.426***


(1.819)


(0.806)
Observations
7,992
SnowDepth
0.147***
R2
0.039


(0.021)
Adjusted R2
0.039
MaxTemp
0.037***
F Statistic
65.260*** (df = 5; 7986)

All of the IVs in the RE model are significant at the 5% significance level.
Multicollinearity affects our ability to single out the effects of an individual variable though it does not affect the overall fit of the model.  We can also see that there is a strong correlation between MaxTemp and WindSpeed with a magnitude of 0.92. It makes sense that high wind speeds is associated with lower temperatures and hence we could consider dropping a term since the extra information is redundant. Since WindSpeed (**p<0.05) is a less significant variable than MaxTemp (***p<0.01), we could drop it.

Correlation matrix


Duration
Age
Precipitation
SnowDepth
MaxTemp
WindSpeed
Duration
1.0
-0.11
-0.015
-0.029
0.14
-0.15
Age
-0.11
1.0
0.030
0.10
-0.21
0.21
Precipitation
-0.015
0.030
1.0
0.10
-0.25
0.29
SnowDepth
-0.029
0.10
0.10
1.0
-0.61
0.47
MaxTemp
0.14
-0.21
-0.25
-0.61
1.0
-0.92
WindSpeed
-0.15
0.21
0.29
0.47
-0.92
1.0



Duration




Age
-0.126***
Constant
14.529***


(0.040)


(1.632)
Precipitation
2.159***
Observations
7,992


(0.799)
R2
0.039
SnowDepth
0.161***
Adjusted R2
0.038


(0.020)
F Statistic
80.010*** (df = 4; 7987)
MaxTemp
0.052***






(0.003)




All of the IVs still remain significant at the 5% significance level after dropping WindSpeed.
The variance inflation factor (VIF) is the ratio of variance in a model with multiple terms, divided by the variance of a model with one term alone. It measures how much the variance of an estimated regression coefficient is increased because of multicollinearity.


VIF
       Age         Precipitation  SnowDepth    MaxTemp    WindSpeed 
     1.181740      1.094342      1.723731      8.304875      6.778088

       Age         Precipitation  SnowDepth  MaxTemp 
     1.176431      1.074530      1.608169      1.918919 

The VIF has decreased significantly from before and suggests that the multicollinearity does not distort the coefficients as much now and it makes the model more robust.

Assumptions
The Panel Data model assumes a multi-linear relationship between the variables. Therefore, the assumptions for linearity must hold as well. There are also many other variables that could affect trip duration such as purpose of the trip, like what we suggested earlier. This could be very hard to collect. The data is also aggregated and may be subjected to sampling error.

Resulting Regression Equation
Duration = -0.126443*Age + 2.159205*Precipitation + 0.160708*SnowDepth + 0.052117*MaxTemp + 14.528680
This suggests that:
An increase in Age by 1 year results in a decrease in trip duration by 0.126443*60 = 7.6s.
An increase in Daily Precipitation by 1 inch results in an increase in trip duration by 2.2mins.
An increase in Daily Snow Depth by 1 inch results in an increase in trip duration by 9.6s.
An increase in Daily Max Temperature by 1F results in an increase in trip duration by 3.1s.
Insight
Since prices of rides could be determined by trip duration. This could help Citi Bike with dynamic pricing and fine-tuning their algorithm for calculating price per unit duration based on daily weather conditions and the profile of their riders.
3. Forecasting Bike Demand
We could also predict bike demand with the frequency of trips using forecasting techniques. We decided to forecast the demand for the most popular bike station as a gauge. 

Data Processing
Using the trip data from July 13 - June 15, we got the top 3 stations with the most number of trips for each month. Next, we obtained the station which appeared in the top 3 for the most number of times. The tabulated result shows that 8 Ave & W 31 St is the most popular station.

Method
We forecasted the demand for this station using the ARIMA model. We first plotted the time series below and we could observe that there is no clear trend but a presence of seasonality.

Next, we conduct the augmented Dicky-Fuller Test to test for stationarity in the time series.

Since the p-value = 0.2677 > 0.05, we do not reject the null hypothesis at the 5% significance level. Therefore, a non-stationary model could be a better fit. 

However, using the auto arima function, we found that the optimal model is ARIMA(2,0,0). Since it has the the lowest Akaike Information Criterion (AIC), it indicates that this model has the highest likelihood in predicting values relative to the other possible models.

ARIMA(2,0,0) with non-zero mean
Coefficients:
          ar1         ar2         mean
        1.1911  -0.6335  7797.9979
s.e.  0.1502   0.1509   900.2166
sigma^2 estimated as 4239109:  log likelihood=-216.46
AIC=440.93   AICc=443.03   BIC=445.64

We checked the robustness of this model for auto-correlation using the Auto-Correlation Function (ACF) and the Partial Auto-Correlation Function (PACF). These are measures to identify if there are correlations between the time lags. Accounting for auto-correlation would prevent our model from over-fitting and becoming unreliable.
 

We can see that none of the lags exceeded the significance region and we can conclude at the 5% significance level, there is no auto-correlation present.
This suggests that our model is good enough with ARIMA (2,0,0) and we can use it to predict the number of trips for the next 6 months. 


Assumptions
The ARIMA model has several assumptions. It assumes that there is homoscedasticity, that is variance is constant over time. It also assumes that there is no seasonality component. We can observe from the time-series that there is indeed some seasonality and intuitively, we can attribute that to things like the seasons in weather where demand for cycling may reduce in winter and peak in summer, We are also applying the forecasting only to the most popular station as a gauge for all stations so it might lead to atomistic fallacies.

Result
From the shaded region, we can see that the forecasted demand seems to decrease from the last-known value and begins flat-line a little. Therefore, it seems that demand is not expected to rise higher than the last-known value for the next 6 months. 

Insight
Citi Bike can use the forecasted demand to adjust the supply of bikes in each station. This could help to reduce wastages and optimize their assets to maximize revenue. For this most popular station, Citi Bike could probably redistribute its bikes to other stations which are expected to have to a surge in demand for the next 6 months.

Conclusion & Summary of Insights
Citi Bike can use the following actionable insights to: 
Focus on understanding their consumer segments
Concentrate on discovering the purpose of different rides to improve targeted advertising
Use weather conditions and profile of riders to develop dynamic pricing
Reduce wastages and optimize assets to maximize revenue with forecasted demand 
