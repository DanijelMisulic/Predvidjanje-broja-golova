# Prediction of the number of goals

##	About the project
The idea of the project is to predict how many goals will be scored by a team in the next match based on the data collected on individual English Premier League matches. Data were collected from the Sports Tracking Website Resultati (http://www.rezultati.com/nogomet/engleska/premier-league-2015-2016/rezultati/). Matches from the 2015/16 season have been observed and some basic statistics extracted from those matches that are characteristic to a football match. How much some statistical parameters affect the number of goals a team will have in a match can be determined by linear regression and this is a method that is used in this project.
The whole project was implemented in the programming language R.

##	Linear regression
Linear regression is a method by which a so-called dependent variable can be predicted using one or more independent variables.Multiple independent variables were used in this project, so in this project it is not used single linear regression, but multiple. The number of goals was selected for the independent variable which should be predicted based on other variables. The following project work will outline what variables have proven to be a good indicator of the total number of goals a team has in a game. In the model which is a result of linear regression, there are as many coefficients as there are independent variables and another additional coefficient, a free member. The values of the coefficients near the independent variables can be positive, so the relation is directly proportional between that independent variable and the one predicted by the model, or inversely proportional if there is a negative sign in front of the independent variable.

The basic idea of linear regression is to provide a model that, based on some new data set, some specific values of independent variables, will give a prediction of the value of the observed dependent variable.

##	Data 
The data used was collected from Rezultati.com, one of the most popular real-time sports performance monitoring websites in Serbia. All data collected is stored in the file [Premier_train.csv](https://github.com/DanijelMisulic/Predvidjanje-broja-golova/blob/master/Premier_train.csv). Data were collected from all matches in the English Premier League 2015/16 season, the first 37 rounds. The idea is that, based on the model trained with the data from those first 37 rounds, the goals of the last 38th round will be predicted, and thus the model will tested in that way. The data of that last competition round is in the Premier_test.csv file. The collected data represent standard statistical parameters that are monitored on every football match such as: number of attempts on the target, percentage of possession, number of yellow cards, number of corners and more. An example of the data used in the analysis is given in Listing 1.

```
Team, Goals, Possession, TotalNumberOfAttempts, AttemptsOnTheTarget, AttemptsOfTheTarget, BlockedShots, FreeKicks, Corners, Offsides, GoalkeeperSaves, Fouls, YellowCards, RedCards, Odds, Winner
Manchester United,1,67,15,2,6,7,11,6,1,2,10,1,0,2.1,1
```
*Listing 1 - An example of the data used for analysis*

##	Project realization and interpretation of the results
The project was implemented in programming language R.The given programming language was chosen for ease of manipulation of CSV files and for
ease of making linear regression models. First, it is necessary to upload the Premier_train.csv file containing the information that has been collected. The **lm*** function is then called, which will make the required linear regression model. The initial idea is to set most of the variables contained in the training dataset as independent, and Goals as dependent. The result of this step alone will not be a perfect model, but it will be possible in several iterations to refine the model given the interpreted results.
```R
Premier = read.csv("Premier_train.csv")
model1 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + TotalNumberOfAttempts + AttemptsOfTheTarget + FreeKicks + Corners + Offsides + Odds +  Winner + RedCards, data = Premier)
summary(model1)
```
*Listing 2 - Regression model1*

In the initial solution model1 was obtained by calling the function lm. To the left of the ~ character is defined the dependent variable, which is in this case variable  *Goals*. To the right of the ~ character are independent variables that are combined with the + sign. Further, it is necessary to specify which dataset is being used, and the one that has been collected and loaded for the previous step will be passed as a parameter. The linear regression model itself is shown below by calling the method:

```R
summary(model1)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3524 -0.5397 -0.1026  0.4679  3.1676 

Coefficients:
                        Estimate Std. Error t value Pr(>|t|)    
(Intercept)             0.733277   0.337704   2.171 0.030479 *  
Possession             -0.006232   0.005044  -1.235 0.217383    
AttemptsOnTheTarget     0.234419   0.047317   4.954 1.07e-06 ***
BlockedShots -0.023064  0.048700  -0.474 0.636044    
TotalNumberOfAttempts   0.010440   0.043700   0.239 0.811309    
AttemptsOfTheTarget    -0.013583   0.042052  -0.323 0.746861    
FreeKicks               0.007883   0.011375   0.693 0.488699    
Corners                -0.064742   0.016553  -3.911 0.000108 ***
Offsides                0.009508   0.025654   0.371 0.711120    
Odds                   -0.039100   0.021199  -1.844 0.065850 .  
Winner                  1.015347   0.095957  10.581  < 2e-16 ***
RedCards               -0.034538   0.150987  -0.229 0.819177    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8237 on 408 degrees of freedom
Multiple R-squared:  0.5443,    Adjusted R-squared:  0.532 
F-statistic: 44.31 on 11 and 408 DF,  p-value: < 2.2e-16


```
*Listing 3 - Statistical parametres of regression model1*

The *estimate* column contains the coefficients for the given independent variables of the model, and among them is a free member. Of the biggest importance are the two values: Multiple R-squared and Adjusted R- squared. They are 0.5443 and 0.532 and based on this it can be concluded that the model is very suitable for predicting the number of goals in a match for now. The maximum value these two coefficients can take is 1 and the closer the value of these two measures is to 1, the more convenient the model is for prediction. The basic measure observed is Multiple R-squared, which represents the mean square error of the model. Further adjusting the model tends to make the Adjusted R-squared measure close to Multiple R-squared. Multiple R-squared coefficient comes to light when variables are added to or thrown out of a model. If, by adding a variable, the value of Multiple R-squared remains the same and Adjusted R-squared decreases then there are problems when there are too many variables in the model. Then the model itself overfits over the data that was passed to it for training, and the model will not behave best when handling the data that is passed to it for the first time - the data used for testing.

As the model seems very complex so far, it will endeavor to be as simple as possible so that only the most important variables are extracted, even at the cost of reducing the R-squared coefficient slightly. Sometimes it is good to have a simpler model, because it's easier way to spot the variables that are really relevant to the problem being observed.

It is further necessary to interpret if one, two or three stars appear in the extension of the variables used, or if those stars do not appear at all in the interpretation of the model. When there are 3 stars (``***``) in the extension of an independent variable, such as *AttemptsOnTheTarget*, *Winner* and *Corners*, it means that these variables are very important to the model and that it is important to keep them. Three stars represent variables with a statistical significance level of 0 or the highest level, and as the number of stars decreases, the level of significance decreases also. It can be noted that there is currently only one other variable that is relevant to the model, but with a significantly lower significance level of 0.1, it is the *Odds* variable. The value of the significance level of variables can vary across iterations of the model, so it tends to simplify the model by throwing out one variable at a time rather than all at once.

In each iteration it is necessary to interpret the results of the model, so it may happen that some variables that were not statistically significant at first become very significant. This is due to the problem of multicollinearity, when there is a large correlation between two variables. This phenomenon may also occur in the proposed model because some variables are derived from others. The idea is to keep the model clear and simple enough, with statistically very significant variables and a relatively high value of both R-squared coefficients.

Of the variables that were not statistically significant in this first iteration, one will be selected to be dropped from the model. The variable that has the highest value in the column  *Pr(>|t|)* is selected, which is the variable *RedCards* with a value of 0.819177 in the given column. Then the same function is called as in the previous iteration only without the *RedCards* variable and **model2** is created.

```R
model2 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + TotalNumberOfAttempts + AttemptsOfTheTarget + FreeKicks + Corners + Offsides + Odds +  Winner, data = Premier)
```
*Listing 4 - Regression model2*

In Model 2, the Multiple R-squared value is 0.5443 and the Adjusted R-squared value is 0.5331. Multiple R-squared value compared to
model1 remained the same, and Adjusted R-squared increased slightly, justifying the removal of the *RedCards*. variable.

```R
summary(model2)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3569 -0.5347 -0.1167  0.4615  3.1722 

Coefficients:
                        Estimate Std. Error t value Pr(>|t|)    
(Intercept)             0.715062   0.327801   2.181 0.029723 *  
Possession             -0.006093   0.005002  -1.218 0.223863    
AttemptsOnTheTarget     0.234396   0.047262   4.960 1.04e-06 ***
BlockedShots           -0.023103   0.048644  -0.475 0.635074    
TotalNumberOfAttempts   0.010488   0.043649   0.240 0.810234    
AttemptsOfTheTarget    -0.013129   0.041956  -0.313 0.754493    
FreeKicks               0.007941   0.011359   0.699 0.484919    
Corners                -0.064745   0.016534  -3.916 0.000106 ***
Offsides                0.009972   0.025544   0.390 0.696449    
Odds                   -0.038302   0.020886  -1.834 0.067402 .  
Winner                  1.019246   0.094322  10.806  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8228 on 409 degrees of freedom
Multiple R-squared:  0.5443,    Adjusted R-squared:  0.5331 
F-statistic: 48.85 on 10 and 409 DF,  p-value: < 2.2e-16

```
*Listing 5 - Statistical parametres of regression model2*

The procedure is repeated further, the next variable to be removed is *TotalNumberOfAttempts*.

```R
model3 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + AttemptsOfTheTarget + FreeKicks + Corners + Offsides + Odds +  Winner, data = Premier)
```
*Listing 6 - Regression model3*

Multiple R-squared = 0.5442, Adjusted R-squared = 0.5342. The Adjusted R - squared measure increased again, representing an improvement in the regression model.

```R
summary(model3)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3462 -0.5354 -0.1185  0.4640  3.1702 

Coefficients:
                       Estimate Std. Error t value Pr(>|t|)    
(Intercept)            0.711408   0.327072   2.175   0.0302 *  
Possession            -0.005930   0.004950  -1.198   0.2316    
AttemptsOnTheTarget    0.244648   0.020303  12.050  < 2e-16 ***
BlockedShots          -0.012381   0.019340  -0.640   0.5224    
AttemptsOfTheTarget   -0.003788   0.015760  -0.240   0.8102    
FreeKicks              0.007920   0.011346   0.698   0.4855    
Corners               -0.064927   0.016498  -3.936 9.75e-05 ***
Offsides               0.010410   0.025450   0.409   0.6827    
Odds                  -0.038045   0.020835  -1.826   0.0686 .  
Winner                 1.020691   0.094022  10.856  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8218 on 410 degrees of freedom
Multiple R-squared:  0.5442,    Adjusted R-squared:  0.5342 
F-statistic: 54.39 on 9 and 410 DF,  p-value: < 2.2e-16

```
*Listing 7 - Statistical parametres of regression model3*

The next variable to be thrown out of the model is  *AttemptsOfTheTarget*.

```R
model4 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + FreeKicks + Corners + Offsides + Odds +  Winner, data = Premier)
```
*Listing 8 - Regression model4*

Multiple R-squared = 0.5441, Adjusted R-squared = 0.5353. 

```R
summary(model4)
Residuals:
    Min      1Q  Median      3Q     Max 
-2.3733 -0.5314 -0.1135  0.4618  3.1804 

Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.701734   0.324214   2.164   0.0310 *  
Possession           -0.006017   0.004931  -1.220   0.2231    
AttemptsOnTheTarget   0.244333   0.020238  12.073  < 2e-16 ***
BlockedShots         -0.013289   0.018946  -0.701   0.4835    
FreeKicks             0.008112   0.011305   0.718   0.4734    
Corners              -0.065608   0.016234  -4.041 6.34e-05 ***
Offsides              0.010421   0.025420   0.410   0.6821    
Odds                 -0.037948   0.020807  -1.824   0.0689 .  
Winner                1.020616   0.093913  10.868  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8209 on 411 degrees of freedom
Multiple R-squared:  0.5441,    Adjusted R-squared:  0.5353 
F-statistic: 61.32 on 8 and 411 DF,  p-value: < 2.2e-16


```
*Listing 9 - Statistical parametres of regression model4*

The process is being repeated, now another variable will be removed which has been shown not to be statistically significant in the model: *Offsides*.

```R
model5 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + FreeKicks + Corners + Odds +  Winner, data = Premier)
```
*Listing 10 - Regression model5*

Multiple R-squared = 0.544, Adjusted R-squared 0.5362.  

```R
summary(model5)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3651 -0.5353 -0.1132  0.4609  3.1902 

Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.729772   0.316598   2.305   0.0217 *  
Possession           -0.006100   0.004922  -1.239   0.2160    
AttemptsOnTheTarget   0.244570   0.020209  12.102  < 2e-16 ***
BlockedShots         -0.013125   0.018923  -0.694   0.4883    
FreeKicks             0.007791   0.011266   0.692   0.4896    
Corners              -0.065560   0.016217  -4.043 6.31e-05 ***
Odds                 -0.038441   0.020751  -1.853   0.0647 .  
Winner                1.021901   0.093766  10.898  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8201 on 412 degrees of freedom
Multiple R-squared:  0.544,     Adjusted R-squared:  0.5362 
F-statistic:  70.2 on 7 and 412 DF,  p-value: < 2.2e-16

```
*Listing 11 - Statistical parametres of regression model5*

It can be noticed that the model is still improving, so iterations continue. The next variable that is being removed is *FreeKicks*.

```R
model6 = lm(Goals ~ Possession + AttemptsOnTheTarget + BlockedShots + Corners + Odds +  Winner, data = Premier)
```
*Listing 12 - Regression model6*

Multiple R-squared = 0.5434, Adjusted R-squared = 0.5368. It can be concluded that the model has improved slightly over several consecutive iterations, but has also simplified quite a bit, which is certainly desirable.

```R
summary(model6)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3303 -0.5452 -0.1030  0.4733  3.1746 

Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
(Intercept)           0.822941   0.286321   2.874  0.00426 ** 
Possession           -0.005867   0.004907  -1.196  0.23256    
AttemptsOnTheTarget   0.244366   0.020194  12.101  < 2e-16 ***
BlockedShots         -0.013024   0.018911  -0.689  0.49141    
Corners              -0.065911   0.016199  -4.069 5.66e-05 ***
Odds                 -0.039411   0.020691  -1.905  0.05750 .  
Winner                1.019287   0.093631  10.886  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8195 on 413 degrees of freedom
Multiple R-squared:  0.5434,    Adjusted R-squared:  0.5368 
F-statistic: 81.93 on 6 and 413 DF,  p-value: < 2.2e-16

```
*Listing 13 - Statistical parametres of regression model6*

In the next iteration, the variable *BlockedShots* will be omitted from the model.

```R
model7 = lm(Goals ~ Possession + AttemptsOnTheTarget + Corners + Odds +  Winner, data = Premier)
```
*Listing 14 - Regression model7*

Multiple R-squared = 0.5429, Adjusted R-squared = 0.5374. 

```R
summary(model7)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3486 -0.5514 -0.1102  0.4581  3.1274 

Coefficients:
                     Estimate Std. Error t value Pr(>|t|)    
(Intercept)          0.828374   0.286031   2.896  0.00398 ** 
Possession          -0.006434   0.004835  -1.331  0.18403    
AttemptsOnTheTarget  0.243746   0.020161  12.090  < 2e-16 ***
Corners             -0.070095   0.015007  -4.671 4.06e-06 ***
Odds                -0.039188   0.020675  -1.895  0.05873 .  
Winner               1.023903   0.093331  10.971  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.819 on 414 degrees of freedom
Multiple R-squared:  0.5429,    Adjusted R-squared:  0.5374 
F-statistic: 98.34 on 5 and 414 DF,  p-value: < 2.2e-16

```
*Listing 15 - Statistical parametres of regression model7*

The next variable that won't stay in the end model is *Possesion*.

```R
model8 = lm(Goals ~ AttemptsOnTheTarget + Corners + Odds +  Winner, data = Premier)
```
*Listing 16 - Regression model8*

Multiple R-squared = 0.5409, Adjusted R-squared = 0.5365. The values of both coefficients decreased compared to the previous iteration, but a variable that was not statistically significant was dropped and thus a simpler model was obtained.
```R
summary(model8)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.3352 -0.5520 -0.1211  0.4925  3.1250 

Coefficients:
                    Estimate Std. Error t value Pr(>|t|)    
(Intercept)          0.50127    0.14638   3.425 0.000677 ***
AttemptsOnTheTarget  0.24045    0.02003  12.006  < 2e-16 ***
Corners             -0.07569    0.01442  -5.250 2.44e-07 ***
Odds                -0.02646    0.01835  -1.442 0.149982    
Winner               1.03715    0.09289  11.166  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8198 on 415 degrees of freedom
Multiple R-squared:  0.5409,    Adjusted R-squared:  0.5365 
F-statistic: 122.3 on 4 and 415 DF,  p-value: < 2.2e-16

```
*Listing 17 - Statistical parametres of regression model8*

When the model has shown satisfactory results it can be selected to stop with iterations or to remove another variable *Odds*.

```R
model9 = lm(Goals ~ AttemptsOnTheTarget + Corners +  Winner, data = Premier)
```
*Listing 18 - Regression model9*

```R
Residuals:
    Min      1Q  Median      3Q     Max 
-2.3581 -0.5566 -0.1177  0.4626  3.1630 

Coefficients:
                    Estimate Std. Error t value Pr(>|t|)    
(Intercept)          0.34641    0.09962   3.477  0.00056 ***
AttemptsOnTheTarget  0.24544    0.01975  12.426  < 2e-16 ***
Corners             -0.07017    0.01392  -5.041 6.92e-07 ***
Winner               1.06676    0.09071  11.761  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.8208 on 416 degrees of freedom
Multiple R-squared:  0.5386,    Adjusted R-squared:  0.5353 
F-statistic: 161.9 on 3 and 416 DF,  p-value: < 2.2e-16
```
*Listing 19 - Statistical parametres of regression model9*

The residuals of the final model vary between -2.2581 and 3.1630. This statistical parameter represents the difference between the values obtained and those predicted by the model. Because the median has a value close to zero, this means that the regression model makes good predictions close to real values. The median in this context is the value of the residual before which 50% of the other residual values are located, followed by the remaining 50%.

For the last created model, we read the Fisher value of 161.9 with 416 degrees of freedom. Degrees of freedom are obtained when the number of variables that participate in the model is subtracted from the total number of observations in the sample. The greater the value of F statistics than the number 1, the better for the model. This measure depends on the sample size and the number of variables that are predicted, so it must be viewed relative to the number of observations.

Residual standard error is a measure of the quality of a linear regression model. This is the average value of how many goals will deviate from the straight regression line.

In the end, a model is obtained in which all the remaining variables are significant with the highest level of significance, so we stop with iterations. Given linear regression model with great precision can predict the number of goals a team has in a game only based on the attempts in the target, number of corners and the ultimate winner of the match.  

##	Model improvement suggestions
The data on which the model is based depends a lot on the offensive game of the football team, and not so much attention is paid to defensive skills. Perhaps even more important would be the defensive features of the opposing team. For example, it is not the same whether a team like Manchester United will have 10 shots in the frame of a lower class team or against a title contender. Also, if a team has a red card during the game, they are more likely to receive a goal, so it is advisable for one team to take into account some of the basic characteristics of the opponent, not just their own characteristics. For example, an additional attribute could be used: whether or not a team had a player more than their opponent at some point or the length of the period with player more in the field.

The presented model produces solid results using only some of the basic statistical parameters available to all. The fact is that the model can be improved by adding some more statistical parameters. There are statistics that are not available to everyone and it is needed to pay for them, but can give a better insight into some aspects of the game of football teams, such as the total mileage players run in a match or the total number of duels they won, number of driblings made etc.

##	Literature
-	EDX course, Analytics Edge, link: https://courses.edx.org/courses/course-v1:MITx+15.071x_3+1T2016/courseware/f8d71d64418146f18a066d7f0379678c/6248c2ecbbcb40cfa613193e8f1873c1/ , date of access: 12.07.2016
-	 Wikipedia, “Linear regression” link: https://en.wikipedia.org/wiki/Linear_regression, date of access: 11.07.2016
-	 Quick guide:Interpreting simple linear model output in R, link: https://rstudio-pubs-static.s3.amazonaws.com/119859_a290e183ff2f46b2858db66c3bc9ed3a.html, date of access: 13.07.2016
