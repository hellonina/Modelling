![election-on-a-typewriter-machine](https://user-images.githubusercontent.com/71023894/94840371-50ecf500-03e6-11eb-87b0-633710feb842.jpg)

## Introduction

For the presidential election that took place in 2000, the votes for George Bush and Al Gore was extremely close. The media released articles with different predictions every other day, and the whole world was closely watching for the election results. The crucial battleground was Florida, where Bush only led by 1,738 votes which resulted in recounting the votes due to the small margin in counts. Recounted votes showed that Bush was leading by less than 400 votes. In the meantime, voters in Palm Beach County argued that the ballot was designed differently so that the voters accidentally voted for Buchanan instead of Gore. There were two evidences were presented to support this claim: Buchanan's votes were exceptionally high for Palm Beach County, and the number of discarded ballots were extremely large (19,000). 

In this context, this analysis focuses on:
* What evidence is there that suppports the argument that Buchanan received more votes than expected in Palm Beach County?
* If it is assumed that Buchanan's actual votes include votes intended for Gore, what can be said about the size of Buchanan's votes?

## Exploratory Data Analysis

<img width="712" alt="스크린샷 2020-10-01 오후 12 39 22" src="https://user-images.githubusercontent.com/71023894/94840503-80036680-03e6-11eb-9b36-ad7e01dc56cd.png">

```yaml
plot(elec$Buchanan2000,elec$Bush2000, main="Scatterplot of Buchanan and Bush Votes", 
      xlab="Buchanan Votes", ylab="Bush Votes", pch=20)
```

The scatter plot for Buchanan's votes show that one data point is relatively high compared to the other data points. Intuitively, this plot shows evidence supporting the claim that Palm Beach County voters made. 

Histogram for response variable y showed that Buchanan's votes seem to be skewed to the right, and not normally distributed. The log transformed version looks much more normally distributed. 

<img width="710" alt="스크린샷 2020-10-01 오후 12 39 46" src="https://user-images.githubusercontent.com/71023894/94840457-7417a480-03e6-11eb-8452-423557f41824.png">

Initially, a model was built with log-transforming Buchanan's votes only. The plot for 'Residual vs Fitted' seems problematic, because the points in the plot should be distributed random for independence assumption to be valid. However, the plot shows a specific trend, thus log-transformation of the predictor should also be considered.

<img width="727" alt="스크린샷 2020-10-01 오후 12 41 39" src="https://user-images.githubusercontent.com/71023894/94840412-62ce9800-03e6-11eb-813d-6d200dd6c75a.png">

## Final Model
The final model is made by taking log on both the response variable and the predictor. 

```yaml
par(mfrow=c(2,2))
newfit <- lm(log(Buchanan2000)~log(Bush2000), data=subset)
summary(newfit)
confint(newfit, level=.95)
ggplot(newfit, aes(x=log(subset$Bush2000), y=newfit$residual)) + geom_point(alpha=.4) 
      + geom_hline(yintercept=0, col ="red3") + theme_classic() 
plot(newfit)
```
The estimated intercept of the final regression model is -2.34 and coefficient is 0.73, which is the slope for log(x). Therefore, 1 unit increase in log(x) will lead to a increase by 0.73. In other words, when the independent variable increases by 1%, then the dependent variable will increase by 0.73%. 


### Model Assessment
<img width="689" alt="스크린샷 2020-10-01 오후 12 41 26" src="https://user-images.githubusercontent.com/71023894/94840880-2f403d80-03e7-11eb-857d-e826fcef7d06.png">
The model fits normality assumption reasonably well as shown in the QQ plot. The upper tail part seems to deviate from the dotted line, but generally speaking the points are aligned to the dotted line. Also, compared to the residual plot before doing the log transformation, the residual plot after doing the log transformation seem much better in the sense that the points are more randomly distributed. 

## Conclusion

The 95% prediction interval for the number of Buchanan votes is from 250.8 to 1399.2, and the fitted value is 592.4. This means that the best case for Buchanan is 1399 votes. But the number of votes for Buchanan in Palm Beach was 3407, which is 2008 votes above the upper bound of the interval, indicating that there was accidental votes which were not intended for Buchanan was included in the vote count for Palm Beach County. Here we can conclude that the claim that the voter made is supported by statistical predictions.
