Proyect-Regression-
===================

Proyect Regression 
## Executive summary
  
 -In this report we try to answer the question : "Is automatic or manual transmission better for mpg ?". To answer this question we used a dataset from the 1974 Motor Trend US magazine, and ran some statistical tests and a regression analysis. 
 
  
  ## Cleaning data
  
 @@ -46,7 +46,7 @@ We then plot the relationships between all the variables of the dataset (see Fig
  
  ## Inference
  
 -We may also run a some tests to compare the mpg means between automatic and manual transmissions.
 +We may also run some tests to compare the mpg means between automatic and manual transmissions.
  
  ### T-test
  
 @@ -79,28 +79,31 @@ model.init <- step(model.all, direction = "backward", k = log(n))
  ```
  
  ```{r}
 -summary(model.init)
 +summary(model.init)$coefficients
  ```
  
 
    
  ```{r}
  model <- lm(mpg ~ wt + qsec + am + wt:am, data = mtcars)
 -summary(model)
 +summary(model)$coefficients
  ```
  
 
  
  ```{r}
 -anova(lm(mpg ~ am, data = mtcars), lm(mpg ~ am + wt, data = mtcars), model.init, model)
 +anova <- anova(lm(mpg ~ am, data = mtcars), lm(mpg ~ am + wt, data = mtcars), model.init, model)
 +cbind(anova[1], anova[2], anova[3], anova[4], anova[5], anova[6])
  ```
  
 
 +Examining these coefficients allows us to determine exactly the point at which the fuel efficieny plots for automatic versus manual cars intersect.
 
  ## Residuals and diagnostics
  
  ### Residual analysis
 @@ -154,7 +157,7 @@ title(main = "Mpg by transmission type", xlab = "am", ylab = "mpg")
  pairs(mtcars, panel = panel.smooth, main = "Pairs graph for MTCars")
  ```
  
 -### Figure 3 : Scatter plot of "mpg" vs. "wt" by transmission type
 +### Figure 3 : Scatter plot of "mpg" and "qsec" vs. "wt" by transmission type
  
  ```{r  fig.height = 10, fig.width = 10}
  plot(mtcars$wt, mtcars$mpg, col = mtcars$am, pch = 19, xlab = "weight", ylab = "mpg")

 ```
 -## 
 -## Call:
 -## lm(formula = mpg ~ wt + qsec + am, data = mtcars)
 -## 
 -## Residuals:
 -##    Min     1Q Median     3Q    Max 
 -## -3.481 -1.556 -0.726  1.411  4.661 
 -## 
 -## Coefficients:
 -##             Estimate Std. Error t value Pr(>|t|)    
 -## (Intercept)    9.618      6.960    1.38  0.17792    
 -## wt            -3.917      0.711   -5.51    7e-06 ***
 -## qsec           1.226      0.289    4.25  0.00022 ***
 -## amManual       2.936      1.411    2.08  0.04672 *  
 -## ---
 -## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 -## 
 -## Residual standard error: 2.46 on 28 degrees of freedom
 -## Multiple R-squared:  0.85,  Adjusted R-squared:  0.834 
 -## F-statistic: 52.7 on 3 and 28 DF,  p-value: 1.21e-11
 +##             Estimate Std. Error t value  Pr(>|t|)
 +## (Intercept)    9.618     6.9596   1.382 1.779e-01
 +## wt            -3.917     0.7112  -5.507 6.953e-06
 +## qsec           1.226     0.2887   4.247 2.162e-04
 +## amManual       2.936     1.4109   2.081 4.672e-02
  ```
  
  The BIC algorithm tells us to consider "wt" and "qsec" as confounding variables. The individual p-values allows us to reject the hypothesis that the coefficients are null. The adjusted r-squared is 0.8336, so we may conclude that more than 83% of the variation is explained by the model.
  
 -However, if we take a look a the scatter plot of "mpg" vs. "wt" by transmission type (see Figure 3 in the appendix) we may notice that the "wt" variable depends on whether or not the car is automatic transmitted (as automatic transmitted cars tend to weigh more than manual transmitted ones). This fact suggests that it would be appropriate to include an interaction term between "wt" and "am".
 +However, if we take a look a the scatter plot of "mpg" vs. "wt" by transmission type (see Figure 3 in the appendix) we may notice that the "wt" variable depends on whether or not the car is automatic transmitted (as automatic transmitted cars tend to weigh more than manual transmitted ones). Apparently, manual transmission only confers an advantage to lighter cars. If the car is heavier than approximately 2.75 tons, an automatic transmission is actually more fuel-efficient than a manual one. This fact suggests that it would be appropriate to include an interaction term between "wt" and "am".
  
  
  ```r
  model <- lm(mpg ~ wt + qsec + am + wt:am, data = mtcars)
 -summary(model)
 +summary(model)$coefficients
  ```
  
  ```
 -## 
 -## Call:
 -## lm(formula = mpg ~ wt + qsec + am + wt:am, data = mtcars)
 -## 
 -## Residuals:
 -##    Min     1Q Median     3Q    Max 
 -## -3.508 -1.380 -0.559  1.063  4.368 
 -## 
 -## Coefficients:
 -##             Estimate Std. Error t value Pr(>|t|)    
 -## (Intercept)    9.723      5.899    1.65  0.11089    
 -## wt            -2.937      0.666   -4.41  0.00015 ***
 -## qsec           1.017      0.252    4.04  0.00040 ***
 -## amManual      14.079      3.435    4.10  0.00034 ***
 -## wt:amManual   -4.141      1.197   -3.46  0.00181 ** 
 -## ---
 -## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 -## 
 -## Residual standard error: 2.08 on 27 degrees of freedom
 -## Multiple R-squared:  0.896,	Adjusted R-squared:  0.88 
 -## F-statistic: 58.1 on 4 and 27 DF,  p-value: 7.17e-13
 +##             Estimate Std. Error t value  Pr(>|t|)
 +## (Intercept)    9.723      5.899   1.648 0.1108925
 +## wt            -2.937      0.666  -4.409 0.0001489
 +## qsec           1.017      0.252   4.035 0.0004030
 +## amManual      14.079      3.435   4.099 0.0003409
 +## wt:amManual   -4.141      1.197  -3.460 0.0018086
  ```
  
  The adjusted r-squared is now 0.8804, so we may conclude that more than 88% of the variation is explained by the model. We will choose this model as our final model.
  
  
  ```r
 -anova(lm(mpg ~ am, data = mtcars), lm(mpg ~ am + wt, data = mtcars), model.init, model)
 +anova <- anova(lm(mpg ~ am, data = mtcars), lm(mpg ~ am + wt, data = mtcars), model.init, model)
 +cbind(anova[1], anova[2], anova[3], anova[4], anova[5], anova[6])
  ```
  
  ```
 -## Analysis of Variance Table
 -## 
 -## Model 1: mpg ~ am
 -## Model 2: mpg ~ am + wt
 -## Model 3: mpg ~ wt + qsec + am
 -## Model 4: mpg ~ wt + qsec + am + wt:am
 -##   Res.Df RSS Df Sum of Sq     F  Pr(>F)    
 -## 1     30 721                               
 -## 2     29 278  1       443 101.9 1.2e-10 ***
 -## 3     28 169  1       109  25.1 3.0e-05 ***
 -## 4     27 117  1        52  12.0  0.0018 ** 
 -## ---
 -## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 +##   Res.Df   RSS Df Sum of Sq      F    Pr(>F)
 +## 1     30 720.9 NA        NA     NA        NA
 +## 2     29 278.3  1    442.58 101.89 1.161e-10
 +## 3     28 169.3  1    109.03  25.10 2.963e-05
 +## 4     27 117.3  1     52.01  11.97 1.809e-03
  ```
  
  We may notice that when we compare the model with only "am" as independant variable and our chosen model, we reject the null hypothesis that the variables "wt", "qsec" and "wt:am" don't contribute to the accuracy of the model.
  
  The regression suggests that, "wt" and "qsec" variables remaining constant, manual transmitted cars can drive 14.0794 + -4.1414 * "wt" more miles per gallon than automatic transmitted cars, and the results are statistically significant. This means that for example, a 1000lbs manual transmitted car can drive 9.9381 more miles per gallon than a same weight automatic transmitted one with the same 1/4 mile time.
  
 +Examining these coefficients allows us to determine exactly the point at which the fuel efficieny plots for automatic versus manual cars intersect, this point occurs at a weight of 3399.6977lbs. We can explain this fact by noticing that almost all of the manual transmission cars are quite small. Therefore, even though cars with automatic transmission might get better mileage across almost all weights, the sample of manual cars consists almost solely of those weights in which manual cars win out.
 +
  ## Residuals and diagnostics
  
  ### Residual analysis
 @@ -286,7 +251,7 @@ pairs(mtcars, panel = panel.smooth, main = "Pairs graph for MTCars")
  
  ![plot of chunk unnamed-chunk-15](figure/unnamed-chunk-15.png) 
  
 -### Figure 3 : Scatter plot of "mpg" vs. "wt" by transmission type
 +### Figure 3 : Scatter plot of "mpg" and "qsec" vs. "wt" by transmission type
  
  
