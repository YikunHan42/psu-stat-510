# Time Series Basics

## Overview of Time Series Characters

**Univariate Time Series**: A sequence of measurements of the same variable collected over time. Most often, the measurements are made at regular time intervals.

difference from linear regression:  data are not necessarily independent and not necessarily identically distributed, **the ordering matters**

### Types of Models
1. Models that relate the present value of a series to past values and past prediction errors - ARIMA models(Autoregressive Integrated Moving Average)

2. Ordinary regression models that use time indices as x-variables

### Important Characteristics to Consider First

+ Is there a **trend**
+ Is there a **seasonity**
+ Are there **outliters**
+ Is there a **long-run cycle**
+ Is there **constant variance**
+ Are there any **abrupt changes**

### Example 1-1
![Annual number of >7.0 earthquakes](https://pic4.zhimg.com/80/v2-ff3785fa8db6a415837695c97bbbc8a2.png)

features:
+ No consistent trend 
+ No seasonality as the data are annual data.
+ No obvious outliers.
+ Difficult to judge whether the variance is constant or not

set $x_{t-1}$ as the lag 1 value of $x_t$, the plot is:

![lag1](https://pic4.zhimg.com/80/v2-788fb83bdd527490531253664aa8cb24.png)

Although it’s only a moderately strong relationship, there is a positive linear association so an AR(1) model might be a useful model.

#### The AR(1) model
$$
x_t = \delta + \phi_1x_{t-1}+\omega_t
$$

Assumptions:

+ $$\omega_t \overset{iid} \sim N\left(0, \sigma_w^{2}\right)$$
+ $w_t$ are independent of $x$

| Predictor | Coef     | SE Coef  | T     | P       |
|-----------|----------|----------|-------|---------|
| Constant  | 9\.191   | 1\.819   | 5\.05 | 0\.000  |
| lag1      | 0\.54339 | 0\.08528 | 6\.37 | 0\.000  |

$quakes = 9.19 + 0.543 lag1$

$S = 6.12239 $

$R-Sq = 29.7% $

$R-Sq(adj) = 29.0%$

The slope coefficient is significantly different from $0$, so the $lag1$ variable is a helpful predictor. The 
 value is relatively weak at $29.7%$, though, so the model won’t give us great predictions.

 #### Residual Analysis
![quake residuals](https://pic4.zhimg.com/80/v2-449e36d272d19808c953d6353e53a785.png)

 Residuals doesn’t show any serious problems.

 ### Example 1-2
![Production of beer](https://pic4.zhimg.com/80/v2-c21796f85c94b68ec59ff4700b110c60.png)

features:
+ An upward trend, possibly a curved one.
+ Seasonality – a regularly repeating pattern of highs and lows related to quarters of the year.
+ No obvious outliers.
+ Might be increasing variation as we move across time, although that’s uncertain.

#### Classical regression methods for trend and seasonal effects

Suppose that the observed series is $x_t$, for $t=1,2,...,n$.

+ For a linear trend, use 
 (the time index) as a predictor variable in a regression.
+ For a quadratic trend, we might consider using both $t$ and $t^2$. 
+ For quarterly data, with possible seasonal (quarterly) effects, we can define indicator variables such as $S_j=1$
 if the observation is in quarter 
 of a year and $0$ otherwise. There are 4 such indicators.

 Let $\epsilon_t \overset{iid} \sim N\left(0, \sigma^{2}\right)$

 both **linear** and **seasonal**:
 $$x_t=\beta_1t+\alpha_1S_1+\alpha_2S_2+\alpha_3S_3+\alpha_4S_4+\epsilon_t$$

 add quadratic:
  $$x_t=\beta_1t+\beta_2t^2+\alpha_1S_1+\alpha_2S_2+\alpha_3S_3+\alpha_4S_4+\epsilon_t$$

> Note!
>
> We’ve deleted the “intercept” from the model. This isn’t necessary, but if we include it we’ll have to drop one of the seasonal effect variables from the model to avoid collinearity issues.

all factors are significant
| Predictor  | Coef     | SE Coef  | T     | P      |
|------------|----------|----------|-------|--------|
| Noconstant |          |          |       |        |
| Time       | 0.5881   | 0.2193   | 2.68  | 0.009  |
| tsqrd      | 0.031214 | 0.002911 | 10.72 | 0.000  |
| quarter_1  | 261.930  | 3.937    | 66.52 | 0.000  |
| quarter_2  | 212.165  | 3.968    | 53.48 | 0.000  |
| quarter_3  | 228.415  | 3.994    | 57.18 | 0.000  |

#### Residual Analysis
The plot of residuals versus fits doesn’t look too bad

![Beer residuall](https://pic4.zhimg.com/80/v2-bda2f64ea6402250e8b889f33caabcf5.png)

### Sample Autocorrelation Function(ACF)
The ideal for a sample ACF of residuals is that there aren’t any significant correlations for any lag.

For example 1 and 2,  there appears to be no significant autocorrelation in the residuals. 

![Example 1 ACF](https://pic4.zhimg.com/80/v2-4c59e7add3148b1fb7db24600d1f4c63.png)

![Example 2 ACF](https://pic4.zhimg.com/80/v2-7c47093b91e5b25fb7a09fb3d5cf710e.png)

## Sample ACF and Properties of AR(1) Model
### Stationary Series
**(Weakly) Stationary Series**
+ The mean $E(x_t)$ is the same for all 
.

+ The variance of $x_t$ is the same for all .

+ The covariance (and also correlation) between $x_t$ and $x_{t-h}$ is the same for all at each lag $h$= 1, 2, 3, etc.

**Autocorrelation Function(ACF)**
$$\frac{Covariance(x_t,x_{t-h})}{Std.Dev.(x_t)Std.Dev.(x_{t-h})}=\frac{Covariance(x_t,x_{t-h})}{Variance(x_t)}$$
Most series that we encounter in practice, however, is not stationary. 

### The First-order Autoregression Model
AR(1) model(linear function) algebraic expression:

$$x_t=\delta+\phi_1x_{t-1}+\omega_t$$

#### Assumptions
+ $$\omega_t \overset{iid} \sim N\left(0, \sigma_w^{2}\right)$$
+ $w_t$ are independent of $x$
+ The series $x_1$, $x_2$, ... is (weakly) stationary, $\left| \phi_1\right|<1$

#### Properties of the AR(1)
+ The (theoretical) mean of $x_t$ is
$$E(x_t)=\mu=\frac{\delta}{1-\phi_1}$$
+ The variance of $x_t$ is
$$Var(x_t)=\frac{\sigma^2_{\omega}}{1-\phi_1^2}$$
+ The correlation between observations $h$ time periods apart is
$$\rho_h=\phi_1^h$$

> $\phi_1$ as the slope in the AR(1) model, also the lag 1 autocorrelation.

#### Pattern of ACF for AR(1) Model
$\phi_1=0.6$
![Phi=0.6](https://pic4.zhimg.com/80/v2-29a4c159e6771b2354c358929c3cd70f.png)

$\phi_1=-0.7$
![Phi=-0.7](https://pic4.zhimg.com/80/v2-f48843e4e849f841b05fafd172f0ae38.png)

### Example 1-3
![ACF for quakes](https://pic4.zhimg.com/80/v2-5edc3c08f63fe83c3b55794e8357f44b.png)

| **Lag.** | **ACF**   |
|----------|------------|
| 1.       |  0.541733  |
| 2.       |  0.418884  |
| 3.       |  0.397955  |
| 4.       |  0.324047  |
| 5.       |  0.237164  |
| 6.       |  0.171794  |
| 7.       |  0.190228  |
| 8.       |  0.061202  |
| 9.       | -0.048505  |
| 10.      | -0.106730  |
| 11.      | -0.043271  |
| 12.      | -0.072305  |

Here, the observed lag 2 autocorrelation = .418884. That’s somewhat greater than the squared value of the first lag autocorrelation (.5417332= 0.293).

The residuals looked okay. This brings up an important point – **the sample ACF will rarely fit a perfect theoretical pattern.**

**A reminder:** Residuals usually are theoretically assumed to have an ACF that has correlation = 0 for all lags.

### Example 1-4
![the daily cardiovascular mortality rate in Los Angeles County](https://pic4.zhimg.com/80/v2-fa3ab5cfe1169b487109c455bfd35f4b.png)

There is a slight downward trend, so the series may not be stationary. To create a (possibly) stationary series, we’ll examine the first differences $y_t=x_t-x_{t-1}$.

Think about a straight line – there are constant differences in average $y$ for each change of 1-unit in $x$.

![First Difference](https://pic4.zhimg.com/80/v2-8d68f5955c688d3a3fd9619a0bd275cf.png)

![ACF of First Difference](https://pic4.zhimg.com/80/v2-95138e9adb46f366a8255dd44dd8af80.png)

| **Lag.** | **ACF **   |
|----------|------------|
| 1.       | -0.506029  |
| 2.       |  0.205100  |
| 3.       | -0.126110  |
| 4.       |  0.062476  |
| 5.       | -0.015190  |

This looks like the pattern of an AR(1) with a negative lag 1 autocorrelation.

> Note! The lag 2 correlation is roughly equal to the squared value of the lag 1 correlation. The lag 3 correlation is nearly exactly equal to the cubed value of the lag 1 correlation, and the lag 4 correlation nearly equals the fourth power of the lag 1 correlation. Thus an AR(1) model may be a suitable model for the first differences $y_t=x_t-x_{t-1}$.

Let $y_t$ denote the first differences, so that $y_{t}=x_{t}-x_{t-1}$ and $y_{t-1}=x_{t-1}-x_{t-2}$. We can write this AR(1) model as

$$y_t=\delta+\phi_1 y_{t-1}+\omega_t$$

Using R, we found that the estimated model for the first differences is

$$\hat{y_t}=-0.04627-0.50636t_{t-1}$$

### Appendix Derivations of Properties of AR(1)

#### Mean
$$
E\left(x_t\right)=E\left(\delta+\phi_1 x_{t-1}+w_t\right)=E(\delta)+E\left(\phi_1 x_{t-1}\right)+E\left(w_t\right)=\delta+\phi_1 E\left(x_{t-1}\right)+0
$$

With the stationary assumption,  $E\left(x_{t}\right)=E\left(x_{t-1}\right)$. Let  $\mu$  denote this common mean. Thus  $\mu=\delta+\phi_{1} \mu$. Solve for  $\mu$  to get

$$\mu=\frac{\delta}{1-\phi_{1}}$$

#### Variance
$$
\begin{aligned}
\operatorname{Var}\left(x_{t}\right) & =\operatorname{Var}(\delta)+\operatorname{Var}\left(\phi_{1} x_{t-1}\right)+\operatorname{Var}\left(w_{t}\right) \\
& =\phi_{1}^{2} \operatorname{Var}\left(x_{t-1}\right)+\sigma_{w}^{2}
\end{aligned}
$$

By the stationary assumption,  $\operatorname{Var}\left(x_{t}\right)=\operatorname{Var}\left(x_{t-1}\right)$. Substitute $\operatorname{Var}\left(x_{t}\right)$  for  $\operatorname{Var}\left(x_{t-1}\right)$  and then solve for  $\operatorname{Var}\left(x_{t}\right)$ .  $\operatorname{Because} \operatorname{Var}\left(x_{t}\right)>0$  it follows that  $\left(1-\phi_{1}^{2}\right)>0$  and therefore  $\left|\phi_{1}\right|<1$ .

#### Autocorrelation Function(ACF)
To start, assume the data have mean 0 , which happens when  $\delta=0$ , and  $x_{t}=\phi_{1} x_{t-1}+w_{t}$ . In practice this isn't necessary, but it simplifies matters. Values of variances, covariances and correlations are not affected by the specific value of the mean.

Let  $y_{h}=E\left(x_{t} x_{t+h}\right)=E\left(x_{t} x_{t-h}\right)$ , the covariance observations  $h$  time periods apart (when the mean  =0  ). Let  $\rho_{h}$=  correlation between observations that are  $h$  time periods apart.

*Covariance and correlation between observations one time period apart*

$$
\gamma_{1}=\mathrm{E}\left(x_{t} x_{t+1}\right)=\mathrm{E}\left(x_{t}\left(\phi_{1} x_{t}+w_{t+1}\right)\right)=\mathrm{E}\left(\phi_{1} x_{t}^{2}+x_{t} w_{t+1}\right)=\phi_{1} \operatorname{Var}\left(x_{t}\right) \\
$$

$$
\rho_{1}=\frac{\operatorname{Cov}\left(x_{t}, x_{t+1}\right)}{\operatorname{Var}\left(x_{t}\right)}=\frac{\phi_{1} \operatorname{Var}\left(x_{t}\right)}{\operatorname{Var}\left(x_{t}\right)}=\phi_{1}
$$

Covariance and correlation between observations  $h$  time periods apart
To find the covariance  $\gamma_{h}$ , multiply each side of the model for  $x_{t}$  by  $x_{t-h}$  , then take expectations.

To find the covariance  $\gamma_{h}$ , multiply each side of the model for  $x_{t}$  by  $x_{t-h}$  , then take expectations.
$$

x_{t}=\phi_{1} x_{t-1}+w_{t} \\
$$

$$
x_{t-h} x_{t}=\phi_{1} x_{t-h} x_{t-1}+x_{t-h} w_{t} \\
$$
$$
E\left(x_{t-h} x_{t}\right)=E\left(\phi_{1} x_{t-h} x_{t-1}\right)+E\left(x_{t-h} w_{t}\right) \\
$$
$$
\gamma_{h}=\phi_{1} \gamma_{h-1}

$$
If we start at  $\gamma_{1}$ , and move recursively forward we get  $\gamma_{h}=\phi_{1}^{h} \gamma_{0}$ . By definition,  $\gamma_{0}=\operatorname{Var}\left(x_{t}\right)$ , so this is  $\gamma_{h}=\phi_{1}^{h} \operatorname{Var}\left(x_{t}\right)$ . The correlation

$$
\rho_{h}=\frac{\gamma_{h}}{\operatorname{Var}\left(x_{t}\right)}=\frac{\phi_{1}^{h} \operatorname{Var}\left(x_{t}\right)}{\operatorname{Var}\left(x_{t}\right)}=\phi_{1}^{h}
$$

## R Code for Two Examples in Lessons 1.1 and 1.2 

For the pdf version, view on [output]()

```r
x=scan("quakes.dat")
x=ts(x) #this makes sure R knows that x is a time series
plot(x, type="b") #time series plot of x with points marked as "o"
```

```r
library(astsa) # See note 1 below
lag1.plot(x,1) # Plots x versus lag 1 of x.
```

```r
acf(x, xlim=c(1,19)) # Plots the ACF of x for lags 1 to 19
```

```r
xlag1=lag(x,-1) # Creates a lag 1 of x variable. See note 2
y=cbind(x,xlag1) # See note 3 below
ar1fit=lm(y[,1]~y[,2])#Does regression, stores results object named ar1fit
summary(ar1fit) # This lists the regression results
```

```r
plot(ar1fit$fit,ar1fit$residuals) #plot of residuals versus fits
```

```r
acf(ar1fit$residuals, xlim=c(1,18)) # ACF of the residuals for lags 1 to 18
```

```r
mort=scan("cmort.dat")
plot(mort, type="o") # plot of mortality rate
```

```r
mort=ts(mort)
mortdiff=diff(mort,1) # creates a variable = x(t) – x(t-1)
plot(mortdiff,type="o") # plot of first differences
```

```r
acf(mortdiff,xlim=c(1,24)) # plot of first differences, for 24 lags
mortdifflag1=lag(mortdiff,-1)
y=cbind(mortdiff,mortdifflag1) # bind first differences and lagged first differences
```

```r
mortdiffar1=lm(y[,1]~y[,2]) # AR(1) regression for first differences
summary(mortdiffar1) # regression results
```

```r
acf(mortdiffar1$residuals, xlim = c(1,24)) # ACF of residuals for 24 lags. 
```
