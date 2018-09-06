# Vector Error Correction Model (VECM): Applications In Finance

<img align="left" width="60" height="200" src="https://img.shields.io/badge/R-v3.5.0-green.svg">
<br>

### Notebook by [Marco Tavora](https://marcotavora.me/)

## Table of contents

1. [Introduction](#Introduction)
2. [Installing Packages](#Installing-Packages)
3. [Johansen Test for Cointegration](#Johansen-Test-for-Cointegration)
3. [Fitting the VECM](#Fitting-the-VECM)

## Introduction
[[go back to the top]](#Table-of-contents)

From [Wikipedia](https://en.wikipedia.org/wiki/Error_correction_model#VECM):

> The Engle–Granger approach as described above suffers from a number of weaknesses. Namely it is restricted to only a single equation with one variable designated as the dependent variable, explained by another variable that is assumed to be weakly exogeneous for the parameters of interest. It also relies on pretesting the time series to find out whether variables are I(0) or I(1). These weaknesses can be addressed through the use of Johansen's procedure. Its advantages include that pretesting is not necessary, there can be numerous cointegrating relationships, all variables are treated as endogenous and tests relating to the long-run parameters are possible. 

VECM models add error correction terms to the vector autoregression (VAR) model. Using simple matrix algebra,
the model can be written as follows:

<br>
<p align="center">
  <img src="images/VECM.png" 
       width="900">
</p>

Following [this excellent blog post](http://blog.mindymallory.com/2018/02/basic-time-series-analysis-a-drunk-and-her-dog-explain-cointegration-and-the-vecm-model/) I will choose the components of the y vector to be the following time series:

- SPY (the S&P 500 exchange traded fund)
- SHY (iShares 1-3 year Treasury Bond) prices. 

If both series are cointegrated, this information must included in the model. This is done introducing, as mentioned above, error correction terms.

## Installing Packages
[[go back to the top]](#Table-of-contents)
```
# install.packages('ggplot2')
# install.packages('xts')
# install.packages('quantmod')
# install.packages('broom')
# install.packages('tseries')
# install.packages("kableExtra")
# install.packages("knitr")
# install.packages("vars")
# install.packages("urca")
```

To load the data we will use the library `quantmod` which contains the function `getSymbols`. From the [documents](https://www.rdocumentation.org/packages/quantmod/versions/0.4-13/topics/getSymbols)

> getSymbols is a wrapper to load data from various sources, local or remote.

In our case we will load data from Yahoo Finance.
```
rm(list=ls()) 
library(tseries)
library(dynlm)
library(vars)
library(nlWaldTest) 
library(lmtest)
library(broom) 
library(car)
library(sandwich)
library(knitr)
library(forecast) 
library(quantmod)
setSymbolLookup(SHY='yahoo',SPY='yahoo')
getSymbols(c('SHY','SPY'))  
```
Defining `y1` and `y2` as the adjusted prices and joining them:
```
y1 <- SPY$SPY.Adjusted
y2 <- SHY$SHY.Adjusted
time_series <- cbind(y1, y2)
print('Our dataframe is:')
colnames(time_series) <- c('SHY', 'SPY') 
```

## Stationarity
[[go back to the top]](#Table-of-contents)

We can check for stationary of the series individually:
```
adf.test(time_series$SHY)
adf.test(time_series$SPY)
print('p-values:')
adf.test(time_series$SHY)$p.value
adf.test(time_series$SPY)$p.value
```

The output is:
```
[1] "p-values:"
0.774665887814869
0.0215848302364735

```
Plotting the series:
```
par(mfrow = c(2,1))
plot.ts(time_series$SHY,
        type='l')
plot.ts(time_series$SPY, 
        type='l')
```
## Johansen Test for Cointegration
[[go back to the top]](#Table-of-contents)

The most well known cointegration test is the Johansen test which estimates the VECM parameters and determines whether the determinant of 

<br>
<p align="center">
  <img src="images/alphabeta.png" 
       width="900">
</p>

is zero or not. If the determinant is not zero, the series are cointegrated.

```
library(urca)
johansentest <- ca.jo(time_series, type = "trace", ecdet = "const", K = 3)
summary(johansentest)

###################### 
# Johansen-Procedure # 
###################### 

Test type: trace statistic , without linear trend and constant in cointegration 

Eigenvalues (lambda):
[1] 2.248927e-02 3.620157e-03 4.437196e-18

Values of teststatistic and critical values of test:

          test 10pct  5pct  1pct
r <= 1 | 10.65  7.52  9.24 12.97
r = 0  | 77.46 17.85 19.96 24.60

Eigenvectors, normalised to first column:
(These are the cointegration relations)

             SHY.l3      SPY.l3  constant
SHY.l3      1.00000      1.0000   1.00000
SPY.l3    -84.59851    163.3147  -8.78908
constant 6927.78683 -12622.5372 543.67420

Weights W:
(This is the loading matrix)

            SHY.l3        SPY.l3      constant
SHY.d 2.147229e-05  1.049741e-04 -1.531138e-17
SPY.d 2.088562e-05 -1.870590e-06  1.148836e-17
```

The lines r=0 and r>= 1 are the results of the test. More specifically:

- line r=0: these are the results of the hypothesis test with null hypothesis $r=0$. More concretely, this test checks if the matrix has zero rank. In the present case the hypothesis is rejected since the test variable is well above the $1\%$ value;
- line r>= 1: these are the results of the hypothesis test $r\le 1$. Now since the test value is below the $1\%$ value value we fail to reject the null hypothesis. Hence we conclude that the rank of $\alpha \beta$ is 1 and therefore the two series are cointegrated and we can use the VECM model.

Note that if hypotheses were reject we would have r=2 corresponding to two stationary series.

## Fitting the VECM
[[go back to the top]](#Table-of-contents)

```
t <- cajorls(johansentest, r = 1)
t

$rlm

Call:
lm(formula = substitute(form1), data = data.mat)

Coefficients:
         SHY.d       SPY.d     
ect1      2.147e-05   2.089e-05
SHY.dl1  -4.754e-02   6.806e-04
SPY.dl1   5.658e-01  -1.192e-01
SHY.dl2  -6.092e-02   4.440e-04
SPY.dl2   3.243e-02  -6.055e-02


$beta
               ect1
SHY.l3      1.00000
SPY.l3    -84.59851
constant 6927.78683

```
