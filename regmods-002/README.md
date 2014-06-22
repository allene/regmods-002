Which transmission got better fuel efficiency
===============================================

Executive Summary
-----------------
The **fuel efficiency** of of an automobile is relationship between the distance traveled and the amount of fuel consumed by the vehicle. Consumption can be expressed in terms of volume of fuel to travel a distance, or the distance travelled per unit volume of fuel consumed.
One of the vehicle feature that contributes fuel effieciency is transmission mode of the vehicle.
As part of Coursera Regression Models course project, I have analysed relation between(if any) vehicle transmesion mode and fuel economy.

I am specifically interested in the effects of automatic vs manual transmission on the gas mileage. By looking at several possible models, I could see relationship does exist between fuel efficiency and transmission type, but that could also be due to other factors such as vehicle weight or number of cylenders. Correlation does not imply causation

## Data

Motor Trends data(1974) is used for the purpose of evaluating factors on fuel efficiency. This data is extracted from the 1974 Motor Trend US magazine, and contains fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models)


```r
data(mtcars)
```
##  Data Exploration

```r
names(mtcars)
```

```
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
```

```r
dim(mtcars)
```

```
## [1] 32 11
```

```r
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```
```
A data frame with 32 observations on 11 variables.

[, 1]   mpg	 Miles/(US) gallon
[, 2]	 cyl	 Number of cylinders
[, 3]	 disp	 Displacement (cu.in.)
[, 4]	 hp	 Gross horsepower
[, 5]	 drat	 Rear axle ratio
[, 6]	 wt	 Weight (lb/1000)
[, 7]	 qsec	 1/4 mile time
[, 8]	 vs	 V/S
[, 9]	 am	 Transmission (0 = automatic, 1 = manual)
[,10]	 gear	 Number of forward gears
[,11]	 carb	 Number of carburetors
Source

Henderson and Velleman (1981), Building multiple regression models interactively. Biometrics, 37, 391-411.
```
As we are interested in the relationshp between mpg and other variables, we first check the correlation between mpg and other variables.

```r
cor(mtcars$mpg,mtcars[,-1])
```

```
##          cyl    disp      hp   drat      wt   qsec    vs     am   gear
## [1,] -0.8522 -0.8476 -0.7762 0.6812 -0.8677 0.4187 0.664 0.5998 0.4803
##         carb
## [1,] -0.5509
```
Looking at the correlation, Cylenders, Displacement, Horse power, Weight and Carbuetors are in inverse relation with mile per gallon.
plots between these variables are included in Apprdix1.

## Data Processing

Adding new logical variable **automatic**  to **mtcars** which will be derived from  **am** .

## Regression Analysis
I would like to use ordinary least squares (OLS) method for estimating the unknown parameters in a linear regression model. As this method minimizes the sum of squared vertical distances between the observed responses in the dataset and the responses predicted by the linear approximation.


```r
olsModel <- lm( mpg ~ automatic, data=mtcars )
summary(olsModel )
```

```
## 
## Call:
## lm(formula = mpg ~ automatic, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -9.392 -3.092 -0.297  3.244  9.508 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      24.39       1.36   17.94  < 2e-16 ***
## automaticTRUE    -7.24       1.76   -4.11  0.00029 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.9 on 30 degrees of freedom
## Multiple R-squared:  0.36,	Adjusted R-squared:  0.338 
## F-statistic: 16.9 on 1 and 30 DF,  p-value: 0.000285
```
Looking at model output, it prooves that transmission type automatic have negative effect on fuel efficiency.

I tried stepwise algorithm to choose the best model.

```r
model1 = step(lm(data = mtcars, mpg ~ .),trace=0,steps=10000)
summary(model1)
```

```
## 
## Call:
## lm(formula = mpg ~ wt + qsec + am, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -3.481 -1.556 -0.726  1.411  4.661 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    9.618      6.960    1.38  0.17792    
## wt            -3.917      0.711   -5.51    7e-06 ***
## qsec           1.226      0.289    4.25  0.00022 ***
## am             2.936      1.411    2.08  0.04672 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.46 on 28 degrees of freedom
## Multiple R-squared:  0.85,	Adjusted R-squared:  0.834 
## F-statistic: 52.7 on 3 and 28 DF,  p-value: 1.21e-11
```
Looking at the results, we have a model which includes 3 variables wt, qsec and am and this model captured 0.8497 of total variance. 
This model can be further optimize our model, we examine mpg ~ wt + qsec and controled by am

```r
model2 <- lm(mpg~ factor(am):wt + factor(am):qsec,data=mtcars)
summary(model2)
```

```
## 
## Call:
## lm(formula = mpg ~ factor(am):wt + factor(am):qsec, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -3.936 -1.402 -0.155  1.269  3.886 
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        13.969      5.776    2.42   0.0226 *  
## factor(am)0:wt     -3.176      0.636   -4.99  3.1e-05 ***
## factor(am)1:wt     -6.099      0.969   -6.30  9.7e-07 ***
## factor(am)0:qsec    0.834      0.260    3.20   0.0035 ** 
## factor(am)1:qsec    1.446      0.269    5.37  1.1e-05 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.1 on 27 degrees of freedom
## Multiple R-squared:  0.895,	Adjusted R-squared:  0.879 
## F-statistic: 57.3 on 4 and 27 DF,  p-value: 8.42e-13
```
total variance is increased to 89%.
## Summary
The improvised model captured 89.5% of total variance. 
* There is a statistically significant relationship between fuel economy and transmission type.
* For every 1000 lbs increase in weight, for automatic vehicles mpg decreased by 3.176 and for manual vehicles mpg decreased by 6.09 miles. 
* For every  ? mile accelaration speed droped, for automatic vehicles mpg increased by 0.834 and manual mpg increased by 1.446 miles. 
* In conclusion, the mpg is largely determined by the interplay between weight, accelaration and tramsmission.
* Two confounding variables - 'weight' and 'quarter-mile time' play a part, and accounting for these reduces the observed effect of transmission type on fuel economy.
* Accounting for the effects of the confounding variables, our model suggests that a manual transmission vehicle will achieve fuel economy ~29.4mpg better than a vehicle with automatic transmission.

## Appendix

```r
pairs(mtcars[c("mpg","cyl","disp" ,"wt", "am", "carb")], panel = panel.smooth)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-81.png) 

```r
mtcarsAuto <- with(mtcars, mean(mpg[am==0]))
mtcarsManual <- with(mtcars, mean(mpg[am==1]))
fit <- lm(mpg ~ am, data=mtcars)
plotModel <- lm(mpg ~ I(am + 1), data=mtcars)

with(mtcars, plot(factor(am, labels=c("Automatic", "Manual")), mpg,
     main="Fuel Economy - Automatic and Manual Transmission",
     xlab="Transmission Type", ylab="Fuel Economy (mpg)"))
abline(plotModel, col='blue')
points(1, mtcarsAuto, col='blue')
points(2, mtcarsManual, col='blue')
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-82.png) 
convert R Mark down file to pdf

```r
library(knitr)
install.packages("markdown")
```

```
## Installing package into '/home/bhasker/R/x86_64-unknown-linux-gnu-library/3.1'
## (as 'lib' is unspecified)
```

```
## Error: trying to use CRAN without setting a mirror
```

```r
rmd.convert('dRM-CourseProject1.Rmd', 'pdf')
```

```
## Error: could not find function "rmd.convert"
```

## References
1. [Fuel economy in automobiles](http://en.wikipedia.org/wiki/Fuel_economy_in_automobiles) 
2. [Motor Trend Car Road Tests](http://stat.ethz.ch/R-manual/R-devel/library/datasets/html/mtcars.html)
3. [Correlation does not imply causation](http://en.wikipedia.org/wiki/Correlation_does_not_imply_causation)
4. [Ordinary least squares](http://en.wikipedia.org/wiki/Ordinary_least_squares)
