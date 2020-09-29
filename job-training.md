![workers-breaktime](https://user-images.githubusercontent.com/71023894/94620373-8249b180-027c-11eb-8f71-0bc39bd6c9ad.jpg)

## Introduction

A study was conducted by the National Supported Work (NSW) to assess effectiveness of job training on disadvantaged workers. In this analysis, a subset of the data was used to dig deeper into the effect of job training on disadvantaged worker’s wages.

In particular, this analysis is focused on finding the evidence that workers who receive job training tend to earn higher wages than workers who do not, and the likely range for the effect of training. Also, this analysis explores interesting associations with wages and other predictors. To address questions of interest, a multiple linear regression model was built to predict the difference in wages for disadvantaged workers between 1974 and 1978 based on variables such as whether or not they received training, years of education, race, ethnicity, age, marital status, and degree obtained. According to the final model, job training was positively associated with wage difference after age 18. The significant variables were job training, age, interaction between age and job training.



## Exploratory Data Analysis 


### Before we start

```yaml
---
lalonde$educ_bin <- ifelse(lalonde$educ < 9, "lt_hs", ifelse(lalonde$educ > 12, "gt_hs", "hs"))
lalonde$pos_wage <- ifelse(lalonde$re78 <= 0, 0, 1)
fac_cols = c('treat', 'black', 'hispan', 'married', 'nodegree', 'educ_bin', 'pos_wage')
for (col in fac_cols) {lalonde[[col]] = lalonde[[col]] %>% as.factor()}
lalonde$wage_diff <- lalonde$re78 - lalonde$re74
lalonde$age_sq <- (lalonde$age)^2
lalonde$pos_wage <- ifelse(lalonde$re78 <= 0, 0, 1)
---
```

A new response variable was created to capture the change in wage growth before and after the training program. This variable was created by subtracting 1974 wage from 1978. A histogram of the response variable was plotted to check normality assumption, and whether any transformation is needed. The response variable seemed to be normally distributed, and thus did not require transformation.

<img width="755" alt="log-y" src="https://user-images.githubusercontent.com/71023894/94620228-44e52400-027c-11eb-8e1d-670c65491454.png">

For the predictors, job training, race, ethnicity, marital status, degree obtained was considered as factor variable, whereas age and education was considered as continuous variable. Also, age sqaured term, and binned education was created in order to see if the relationships are better explained when the variables are squared or binned. 


### Exploring Data

When exploring data, box plots and scatter plots were created to explore relationships between variables. The most notable relationship was the association between age and wage difference categorized by job training.

```yaml
---
#Age vs. Wage Differential
ggplot(lalonde, aes(x =age, y = wage_diff)) + geom_point(alpha = .5,colour="blue4") + geom_smooth(method="lm",col="red3") +
  labs(title="Wage difference vs age by treatment", x="Age",y="Wage difference") + facet_wrap( ~ treat,ncol=4)
---
```

For the workers who did not receive job training, there are a number of data points with negative wage differential especially in the age range of 40 to 50. On the contrary, there were not many data points with a negative wage differential for workers who received job training. This plot seemed to demonstrate that further investigation is needed on the variables age and job training and their interactions.


<img width="766" alt="job-training" src="https://user-images.githubusercontent.com/71023894/94620199-37c83500-027c-11eb-8e05-9a992a80928f.png">



## Final Model

After the EDA, we performed the model selection. We decided to use stepwise model selection procedures. Since we are most interested in the association between job training and the difference in wage, our null model included job training. The full model included all the variables in this dataset. We used two selection methods: stepwise selection using AIC and stepwise selection using BIC. The model minimizes AIC and the model minimizes BIC had the same predictors: job training, age, and married, and all three variables are significant at 0.05 level. The result of the F-test proved that adding any other predictor would not improve this model, so we are convinced that our model would include job training, age and married. 

From EDA, we found the interaction between age and treat to be interesting, so we wanted to further explore if including this interaction would improve the model. The f-test confirmed that including the interaction would improve the model, with p-value equals to .003.

```yaml
--- 
# Final Model
final <- lm(wage_diff~ age + treat + married + age:treat, data=lalonde)
pander(summary(final))
---

```

Predictors | Estimated Std. | P-Value |  
------------ | ------------ | ------------- |  
(Intercept) |6,072 | 9.62e |
Age |-135.8  | 0.00 |
Received job training |-4,586 | 0.05 | 
Married |-1,757 | 0.01 | 
Age:Job Training |255.9 | 0.00 |



From the final model, the coefficient for intercept is 6072: for a worker who is at the age of 0, received no training, and unmarried, we expect his/her wage to be on average $6072. If marital status changes from unmarried to married, we expect a worker to earn $1757 less on average, holding everything else constant. Age on its own is negatively associated with wage. As age increases by 1 additional year, the wage is expected to decrease by $136. 

Moreover, job training on its own, is negatively associated with wage as well. When everything else holds constant, and when the workers receive job training, we expect the wage to decrease by $4586 on average. This seems to be counterintuitive. Here, considering the effect of the interaction term is critical: the wage for the trained group would be boosted $256 per year compared to the untrained group. So as a worker grows older, if he is trained, the growth in age would bring much more increase in wage than if he is untrained. We predicted that, as soon as a worker turned 18, if other variables held constant, he would earn more if he is trained. 

#### Model Assessment

<img width="991" alt="fitted" src="https://user-images.githubusercontent.com/71023894/94620272-5af2e480-027c-11eb-9599-cbfb760e7993.png">
<img width="992" alt="normal-qq" src="https://user-images.githubusercontent.com/71023894/94620245-4f072280-027c-11eb-95d2-f7f168dcbcf4.png">

After building the model, four basic model assumptions were verified: linearity, independence, equal variance, and normality. There were no serious violations found in the plot.
Confidence interval at 95% level confirmed that the effect of job training for disadvantaged workers on annual earnings was -$9,268 to $95. 

For multicollinearity issue, VIF were computed. The values were small, indicating that there is no multicollinearity issue. The cook's distance plot showed that there are no data points falling outside of 0.5 cook's distance line. 


## Conclusion


In this analysis, the association between wage and job training was explored. The final model confirmed that after the age of 18, job training was positively correlated with earning higher wages. It also confirmed that the effect of job training differed by age. For every additional increase in age, wage is expected to increase by $120 for trained workers and decrease by $136 for non-trained workers, holding everything else constant.
 
There are potential limitations of this study. First, macroeconomic factors are not considered. For example, US economy was acutely strained by the oil crisis in 1973. Since the data is mostly collected in 1974 and 1978, the predictors and response variable used in this analysis could have been affected seriously by the macroeconomic factor. Another limitation is that the data did not specify zero-income. In this analysis, zero income was considered as unemployability, but it could also indicate retirement or going back to school.
