 
![wit-beer-in-glass](https://user-images.githubusercontent.com/71023894/94621679-c047d500-027e-11eb-927c-f36b1cb8c04b.jpg)

## Introduction
 
 
This analysis will focus on:
* What 

You will demonstrate the impacts of some variables on beer consumption in a given region and the consumption forecast for certain scenarios. The data (sample) were collected in Sao Paulo, Brazil, in a university area, where there are some parties with groups of students from 18 to 28 years of age (average).
 
 Original dataset can be found in Kaggle: https://www.kaggle.com/dongeorge/beer-consumption-sao-paulo/.
 
 
 ## Exploratory Data Analysis
 
 ## Response variable and predictors
 
 The histogram for the response variable reveals that distribution is normal. Normality assumption is plausible here. 
 
 With the exception of precipitation, all other variables appear to be roughly linear. Precipitation does not appear to be linear. The beer consumption is clustered around zeros with many more values for 0 than any other value for precipitation. This makes sense because there will be many days when it does not rain and precipitation will be zero, vs it will almost never rain the same amount on two different days.
 
 No, it does not make sense to include all three because these are not independent variables. They are all highly correlated with one another, so if we include all three the assumption of independence does not hold. We should select only one of the three variables.
 
 Intercept: If all variables are zero - meaning that there is 0 precipitation, it is not a weekend, and it is 0 degrees celsius, there will be an average of 6.47 liters of beers consumed.
Weekend1 = Weekend is a significant variable. If all other variables are held constant , beer consumption increases by 5.23 liters on average if it is a weekend.
Temp_median_c = The median temperature is significant. If all other variables are held constant, every additional degree the temperature increases, beer consumption increases by .84 liters.
Precipitation = The precipitation is significant. Holding all other variables constant, for every additional mm of precipitation, beer consumption decreases by .07 liters.

Our multiple r-squared is 0.6612 meaning that 66.12% of variability can be explained by the variables examined in our model (Weekend1, temp_median_c, precipitation_mm). The median temperature appears to be the best variable for predicting beer consumption due to the fact that it has the highest absolute t-value.
