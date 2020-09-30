![seattle-skyline-and-mountains](https://user-images.githubusercontent.com/71023894/94705662-4e6b9c00-030f-11eb-97a2-b685984cdd9a.jpg)


## Summary

## Exploratory Data Analysis
There are 145 superhosts out of total 305 hosts in Queen Anne. Out of total 305 hosts, 163 hosts' identity is verified. 

The histogram for price (response variable) is not normally distributed, and therefore it makes much more sense to take log transformation. The histogram for log-transformed price seems to be normally distributed. 

I drew scatterplots for the variable accommodates, bedrooms and bathrooms. In general, there doesn't seem any special pattern, but all three variables seem to have somewhat linear relationship with the response variable. For example, when the number of accommodates (or bedrooms, bathrooms) increases, the price tend to increase as well.

I first tried making the model without any transformation. The Residual vs. Fitted graph shows that the points are not equally spread out around zero, indicating that there is an issue with equal variance assumption, and also QQ graph doesn't look good, meaning that normality assumption is violated. Therefore, I decided to take log transformation of the response variable, price. 

The p-values for host_is_superhost and host_identity_verified is over 0.05, indicating that these two variables are not statistically significant. All other variable's p-value are smaller than 0.05, therefore statistically meaningful. 


I tried log transformation of price, and the graphs look much better. QQ plot improved. It follows the 45 degree dotted line much better than before. The plot for residuals vs. fitted seem to be better, as the dots are more spread out around zero.

The p-values for variables have changed as well. The p-value for host_identity_verified was 0.1707 but has gotten smaller to 0.0231. It exceeded 0.05 before log transformation, but became smaller than 0.05 after log transformation. Unfortunately, the p-value for host_is_superhost still exceeds 0.05, and still remains as statistically insignificant. 


## Final Model

### Interpretation of the final model

The coefficient is described in the table below. 

The estimated intercept of the model is 4.43, which means that when all x variables are zero, the price is 83.93 dollars. 

If the host's identity is verified, then we would expect the price to be less by 10% compared to non-verified hosts (holding value of all other predictors the same). We would expect the price to be less by 0.009% when the host is superhost, compared to non-superhosts (when all other conditions remain the same) 

Compared to entire house/apt room type, we would expect the price to be less by 56.8% when the room is private type. Also, if the room is shared type, we would expect the price to be 31% higher compared to entire house/apt type. Holding value of all other predictors applies the same here.

Holding all other factors constant, when accommodates increases by 1 unit, the price increases by exponential of 0.04, which is 4%. Also, holding all other conditions constant, when bathroom increases by 1 unit, the price will increase by exponential of 0.21, which equals to 23%. Similarly, when bedroom increases by 1 unit, the price will increase by exponential of 0.13 which is 14%. 

3.4 Points 31 and 138 seems to be influential points. Their Di value is almost 3.0, which exceeds 1. The graph 'Leverage Scores' confirms what we have seen in Cook's Distance plot. 

After taking out points 31 and 138, the multiple r-squared has gotten better. Before taking out the outlier points, the multiple R-squared was 0.6682. Now it became 0.6839, which means that the model explains better. 

### Assessment
We can check Normal QQ plot for normality assumption. QQ plot has improved compared to the graph where we did not perform any transformation. The QQ plot below is less deviating from the dotted line. 

Residuals vs. Fitted plot has improved as well. Before taking the log, the dots were concentrated in specific areas. Now they are more spread out in the graph. 


## Conclusion
The variable, 'host_is_superhost' has p-value of 0.8319 which exceeds 0.05. Since it is statistically insignificant variable, we could consider taking out this variable. If we take out, we can still leave the model as it is, or go find some other variable that would have p-value less than 0.05, and predict the response variable better. 

I computed correlation of the variables 'accommodates', 'bathrooms' and 'bedrooms'. The correlation between 'accommodates' and 'bedrooms' were 0.87, which means that they are positively correlated. For 'bathrooms' and 'bedrooms', the correlation is 0.79. For 'accommodates' and 'bathrooms', the correlation is 0.73. Since the correlations are quite close to 1, it means that they are not independent to each other. This can cause multicollinearity issue. We can consider centering the variable in order to reduce multicollinearity. 
