![hand-painted-no-smoking-sign](https://user-images.githubusercontent.com/71023894/94619139-575e5e00-027a-11eb-8f1f-fa1e024a714b.jpg)

## Introduction


The fact that maternal smoking seriously affects the baby is a common knowledge nowadays. However, this was not the case fifty years ago. One of the first studies to reveal the relationship between maternal smoking and the effect on the baby was first done in the 1960s, and researchers have been developing the idea till now. Although we now all know that smoking during pregnancy is dangerous, this analysis is conducted to compare the odds of giving pre-term birth for a smoking mother versus a non-smoking mother, and if there are any interesting associations among the predictors besides smoking status. 

Our questions of interest is:

* Compared to non-smoking mothers, do smoking mothers have higher possibility of giving pre-term birth? 
* Can we find any evidence that the chances of giving pre birth differs by mother's race? 
* Besides the variables mentioned above, are there any other interesting findings?

In this report, logistic regression model was made to address the questions of interest. 



## Exploratory Data Analysis 


### Data
Before starting the analysis, the variable 'premature' was created based on gestation days: if the gestation is less than 270 days, it is categorized as 1, else 0. Predictors such as race, income bracket, education and whether mothers smoked or not were factored. Height, mother's pre-pregnancy weight, age and parity were considered as continuous variable. 


### Exploring Data
The boxplot for parity, age, height and pregnancy weight against the response variable showed that there is not much difference between two groups of mothers with regards to each numeric variable. The median parity value for moms who gave premature birth was similar to that of moms who did not give premature birth. This was same for all other numeric variables; the median value and the overall distribution was similar when comparing mothers who gave premature birth and mothers who did not. 


```yaml
---
#Parity vs. Pre-term birth
ggplot(smoke,aes(x=parity, y=premature_fac, fill=parity)) +
  geom_boxplot() + coord_flip() + scale_fill_brewer(palette="Reds") +
  labs(title="Parity vs Gestation", x="Parity",y="Gestation") + theme_classic() + theme(legend.position="none")
---
```
![parity-boxplot 2](https://user-images.githubusercontent.com/71023894/94618578-609afb00-0279-11eb-9085-05baa18daf1a.jpg)

The probabilities for premature birth given mother's race is white was 0.16, which was the lowest of all race categories. On the other hand, when mother's race is asian, odds of premature birth was 0.32, two times higher compared to white mothers. The p-value of premature and race was lower than 0.05, which indicated that these two variables are related and this variable be included in the model. 


```yaml
---
tapply(smoke$premature_fac, smoke$mrace, function(x) table(x)/sum(table(x)))
chisq.test(table(smoke[,c("premature_fac","mrace")]))
---
```

Another variable that showed p-value under 0.05 on their chi-squared test was pre-term birth and education. As the education level went up, the probability of giving premature birth decreased. 


```yaml
--- 
# R code for making a table
tapply(smoke$premature_fac, smoke$med, function(x) table(x)/sum(table(x)))
chisq.test(table(smoke[,c("premature_fac","med")]))
---
```

For the variable 'smoke', the probability of giving premature given that the mother smoked was higher than non-smoking mothers. But the results of chi-squared test showed that smoke and premature is not related to each other. 

The binned plots for pregnancy weight and premature birth showed a specific trend. The probability tended to decrease as pregnancy weight increased from 100 to 120, but the odds peaked again when the pregnancy weight was around 140 pounds, and decreased afterwards. 

```yaml
---
binnedplot(y=smoke$premature,smoke$mpregwt,xlab="Pregnancy Weight",ylim=c(0,1),col.pts="navy",
           ylab ="Premature baby?",main="Pregnancy Weight and Premature", col.int="white")
---
```

<img width="774" alt="binned-plot" src="https://user-images.githubusercontent.com/71023894/94618825-cab3a000-0279-11eb-8b2d-352fe9c846ed.png">


## Final Model

The final model was selected based on (1) stepwise using AIC and (2) ANOVA chi-squared test. 

The baseline model was structured with all the variables and the interaction between race and smoke. This interaction was included to address our main question of interest. 

After spotting the trend in 'parity' in the binned residual plot, I made parity a factor variable; moms who have never had any babies were categorized as 1, moms who have had 1 to 3 children as 2, moms with 4 or more children as 3. A model was made with binned parity, but was dropped eventually after performing ANOVA chi-squared test with the baseline model (chi-value exceeded 0.05). 

I went back to the baseline model where I had a number of variables and performed stepwise using AIC. The result suggested that I eliminate the interaction between race and smoke but I kept the interaction intentionally. Final model is described below:

<img width="568" alt="final-model" src="https://user-images.githubusercontent.com/71023894/94618921-f767b780-0279-11eb-9c61-45214917dd3c.png">

### Interpretations

* Based on the final model, there were no interesting associations with the odds of pre-term birth that are worth mentioning. The one and only interaction term included in the final model (i.e. race:smoke) all had p-values over 0.05. 

* The p-value for smoke was under 0.05, making it a statistically significant variable. This implies that mothers who smoke have higher chances of giving premature birth compared to non-smoking mothers. When mother smokes, the odds of giving pre-mature birth increases by multiplicative effect e to the exponential 0.46. 95% confidential interval for the predictor smoke is (0.03, 0.90).

* The coefficient for race 7 (black) is statistically significant. This indicates that if the mother's race is black, then the odds of pre-mature birth is increased by a multiplicative effect e to the exponential 1.02 compared to the case when mother's race is white. 

* Another significant variable is pregnancy weight. Keeping everything else constant, as pregnancy weight increases by 1 unit, the odds of pre-mature birth decreases by multiplicative effect e to the exponential 0.01. 95% confidential interval for the predictor pregnancy weight is (-0.02, -0.002).



Predictors | Estimated Std. | P-Value |  
------------ | ------------ | ------------- |  
(Intercept) |-1.92 | 0.00 |
Pre-pregnancy weight |-0.01 | 0.02 |
Mexican |0.50 | 0.40 | 
Black |1.02 | 0.00 | 
Asian |0.76 | 0.12 |
Mixed |-13.68 | 0.97 |
If they smoke |0.46 | 0.04 |
Mexican:Smoke |0.10 | 0.92 |
Black:Smoke |-0.52 | 0.21 |
Asian:Smoke |0.25 | 0.76 |
Mixed:Smoke |14.54 | 0.97 |


For the final model, there was no issues of multicollinearity as all the VIF values were small.

```yaml
---
kable(vif(final))
---
```

Variable | VIF Values | 
------------ | ------------ | 
Pre-pregnancy weight |1.11e | 
Mexican |1.49e | 
Black |2.09e | 
Asian |1.59e | 
Mixed |1.12e | 
Smoke |1.57e | 
Mexican:Smoke |1.47e | 
Black:Smoke |2.23e | 
Asian:Smoke |1.52e | 
Mixed:Smoke |1.12e | 

<img width="1005" alt="curve" src="https://user-images.githubusercontent.com/71023894/94619970-d30cda80-027b-11eb-8009-e83b758b27da.png">

Looking at the binned residual plots for the final model, we should be checking randomness of the points as well as whether there are many points that are outside the red line. The points are randomly distributed. Also, there are three points that are outside the red line, but because most of them are inside the red line it can be considered okay. The points that are outside the red line are the outliers, and thus we can say that this model has few outliers. The value of AUC is 0.63 which seems to be fine. 

```yaml
---
par(mfrow=c(1,2)) 
roc(smoke$premature,fitted(final),plot=T,print.thres="best",legacy.axes=T,
    print.auc =T,col="red3")

rawresid1 <- residuals(final, "resp") #resp = response scale (true y-predicted probability)
binnedplot(x=fitted(final),y=rawresid1,xlab="Pred. probabilities",
           col.int="red4",ylab="Avg. residuals",main="Binned residual plot",col.pts="navy")

Conf_mat <- confusionMatrix(as.factor(ifelse(fitted(final) >= 0.5, "1","0")),
                            as.factor(smoke$premature),positive = "1")
---
```



This confusion matrix is based on 0.5 level is depicted below. The sensitivity level is extremely low. That is, this model is valid for identifying actual cases but also has high rate of false positives. Another confusion matrix was made with the mean threshold. The accuracy decreased to 0.58 compared to the confusion matrix above, but the values for sensitivity and specificity are more balanced. 



## Conclusion


The logistic regression model was used for predicting premature birth with the variables 'smoke', 'race', 'pregnancy weight' and the interaction term between race and smoke. As a result, the predictors smoke, pregnancy weight was significant, as well as race category 'black'. The interaction term between race and smoke was included to address question of interest, but the p-value revealed that it is not statistically significant. The final model confirmed that smoking mothers have higher chances of giving premature birth compared to non-smoking mothers. Also, the model implied that the odd ratio of pre-term birth for smokers and non-smokers differed by mother's race when mother's race was black. 

The fact that sensitivity is extremely low can be the limitation of this analysis. Also, there are predictors that have not been considered in this analysis (e.g. information on the father-side) which could potentially improve the model. 

