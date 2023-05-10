# Identifying and Estimating ARIMA models; Using ARIMA models to forecast future values

## Non-seasonal ARIMA Models
ARIMA models, are models that may possibly include autoregressive terms, moving average terms, and differencing operations.

+ When a model only involves autoregressive terms it may be referred to as an AR model. When a model only involves moving average terms, it may be referred to as an MA model.
+ When no differencing is involved, the abbreviation ARMA may be used.

> Note!

> Here we’re only considering non-seasonal models.

### Specifying the Elements of the Model
In most software programs, the elements in the model are specified in the order (AR order, differencing, MA order). As examples,

A model with (only) two AR terms would be specified as an ARIMA of order (2,0,0).

A MA(2) model would be specified as an ARIMA of order (0,0,2).

A model with one AR term, a first difference, and one MA term would have order  (1,1,1).

For the last model, ARIMA  (1,1,1) , a model with one AR term and one MA term is being applied to the variable  $Z_{t}=X_{t}-X_{t-1}$. A first difference might be used to account for a linear trend in the data.

The differencing order refers to successive first differences. For example, for a difference order = 2  the variable analyzed is  $z_{t}=\left(x_{t}-x_{t-1}\right)-\left(x_{t-1}-x_{t-2}\right)$, the first difference of first differences. This type of difference might account for a quadratic trend in the data.

### Identifying a Possible Model
Three items should be considered to determine the first guess at an ARIMA model: a time series plot of the data, the ACF, and the PACF.

#### **Time series plot of the observed series**

> Note!

> Nonconstant variance in a series with no trend may have to be addressed with something like an ARCH model which includes a model for changing variation over time.

+ You won’t be able to spot any particular model by looking at this plot, but you will be able to see the need for various possible actions.
+ If there’s an obvious upward or downward linear trend, a first difference may be needed. A quadratic trend might need a 2nd order difference (as described above). We rarely want to go much beyond two. In those cases, we might want to think about things like smoothing. Over-differencing can cause us to introduce unnecessary levels of dependency (difference white noise to obtain an MA(1)–difference again to obtain an MA(2), etc.)
+ For data with a curved upward trend accompanied by increasing variance, you should consider transforming the series with either a logarithm or a square root.


#### **ACF and PACF**

The ACF and PACF should be considered **together**. It can sometimes be tricky going, but a few combined patterns do stand out. Note that each pattern includes a discussion of both plots and so you should always describe how both plots suggest a model.

+ AR models have theoretical PACFs with non-zero values at the AR terms in the model and zero values elsewhere. The ACF will taper to zero in some fashion. An AR(1) model has a single spike in the PACF and an ACF with a pattern
![AR(1) Example](https://pic4.zhimg.com/80/v2-94ff037f525ffe5832086dc14ec6dd75.png)

$$
\rho_k=\phi_1^k
$$


+ An AR(2) has two spikes in the PACF and a sinusoidal ACF that converges to 0.
![AR(2) Example](https://pic4.zhimg.com/80/v2-1205c97c0365273cda557f651b97d452.png)

+ MA models have theoretical ACFs with non-zero values at the MA terms in the model and zero values elsewhere.  The PACF will taper to zero in some fashion.
![MA(1) Example](https://pic4.zhimg.com/80/v2-c1d671c649460c46472ac12a2bb76bae.png)

+ ARMA models (including both AR and MA terms) have ACFs and PACFs that both tail off to 0. These are the trickiest because the order will not be particularly obvious. Basically you just have to guess that one or two terms of each type may be needed and then see what happens when you estimate the model.
![ARMA(1,1)](https://pic4.zhimg.com/80/v2-cc03dc5ee8a51b79f7671aa7b5ce0ead.png)

+ If the ACF and PACF do not tail off, but instead have values that stay close to 1 over many lags, the series is **non-stationary and differencing will be needed**. Try a first difference and then look at the ACF and PACF of the differenced data.

+ If the series autocorrelations are non-significant, then the series is random (white noise; the ordering matters, but the data are independent and identically distributed.) You’re done at that point.

+ If first differences were necessary and all the differenced autocorrelations are non-significant, then the original series is called a random walk and you are done. (A possible model for a random walk is $x_t=\delta+x_{t-1}+\omega_t$
. The data are dependent and are not identically distributed; in fact both the mean and variance are increasing through time.)

> Note!
You might also consider examining plots of $x_t$ versus various lags of $x_t$.

### Estimating and Diagnosing a Possible Model
After you’ve made a guess (or two) at a possible model, use software such as R, Minitab, or SAS to estimate the coefficients. Most software will use maximum likelihood estimation methods to make the estimates. Once the model has been estimated, do the following.

- Look at the significance of the coefficients. In R, sarima provides pvalues and so you may simply compare the  p -value to the standard 0.05 cut-off. The arima command does not provide  p -values and so you can calculate a t-statistic:  $t=$  estimated coeff. / std. error of coeff. Recall that  $t_{\alpha, d f}$ is the Student t -value with area $\|\alpha\|$ to the right of  $t_{\alpha, d, f}$  on  $df$  degrees of freedom. If  $|t|>t_{0.025, n-p-q-1}$, then the estimated coefficient is significantly different from 0. When  $n$ is large, you may compare estimated coeff. / std. error of coeff to 1.96.

- Look at the ACF of the residuals. For a good model, all autocorrelations for the residual series should be non-significant. If this isn't the case, you need to try a different model.

- Look at Box-Pierce (Ljung) tests for possible residual autocorrelation at various lags.

- If non-constant variance is a concern, look at a plot of residuals versus fits and/or a time series plot of the residuals.

If something looks wrong, you’ll have to revise your guess at what the model might be. This might involve adding parameters or re-interpreting the original ACF and PACF to possibly move in a different direction.

### What if More Than One Model Looks Okay?
Sometimes more than one model can seem to work for the same dataset. When that’s the case, some things you can do to decide between the models are:

+ Possibly choose the model with the fewest parameters.

+ Examine standard errors of forecast values. Pick the model with the generally lowest standard errors for predictions of the future.

+ Compare models with regard to statistics such as the MSE (the estimate of the variance of the $w_t$), AIC, AICc, and SIC (also called BIC). Lower values of these statistics are desirable.

AIC, AICc, and SIC (or BIC) combine the estimate of the variance with values of the sample size and number of parameters in the model.

One reason that two models may seem to give about the same results is that, with the certain coefficient values, two different models can sometimes be nearly equivalent when they are each converted to an infinite order MA model. [**Every ARIMA model can be converted to an infinite order MA** – this is useful for some theoretical work, including the determination of standard errors for forecast errors.]

### Example 3-1: Lake Erie
The Lake Erie data series is n = 40 consecutive annual measurements of the level of Lake Erie in October.

#### Identifying the model
A time series plot of the data is the following:
![Lake Erie](https://pic4.zhimg.com/80/v2-d712dd5c5ed17f75745c5298ae25e00f.png)

There's a possibility of some overall trend, but it might look that way just because there seemed to be a big dip around the 15th time or so. We'll go ahead without worrying about trend.

The ACF and the PACF of the series are the following. (They start at lag 1).

![xerie ACF and PACF](https://pic4.zhimg.com/80/v2-f0d72285c06af08471dd6418af9cfb1e.png)

The PACF shows a single spike at the first lag and the ACF shows a tapering pattern. An AR(1) model is indicated.

#### Estimating the Model
We used an R script written by one of the authors of our book (Stoffer) to estimate the AR(1) model. Here's part of the output:

![Lake Erie AR Output](https://pic4.zhimg.com/80/v2-13aac46b3cf40720f404c54a19ada2c7.png)


Where the coefficients are listed, notice the heading "xmean." This is giving the estimated mean of the series based on this model, not the intercept. The model used in the software is of the form $\left(x_{t}-\mu\right)=\phi_{1}\left(x_{t-1}-\mu\right)+w_{t}$.

The estimated model can be written as  $\left(x_{t}-14.6309\right)=0.6909\left(x_{t-1}-\right.   14.6309)+w_{t}$.

This is equivalent to  $x_{t}=14.6309-(14.6309 * 0.6909)+0.6909 x_{t-1}+w_{t}=   4.522+0.6909 x_{t-1}+w_{t}$ 

The AR coefficient is statistically significant  $(z=0.6909 / 0.1094=6.315)$. It's not necessary to test the mean coefficient. We know that it's not 0.

The author's routine also gives residual diagnostics in the form of several graphs. Here's that part of the output:

![Lake Erie AR Residuals](https://pic4.zhimg.com/80/v2-35ff51a103fe3e8e6d5e2036a97c0de6.png)

#### Interpretations of the Diagnostics
The time series plot of the standardized residuals mostly indicates that there's no trend in the residuals, no outliers, and in general, no changing variance across time.

The ACF of the residuals shows no significant autocorrelations – a good result.

The Q-Q plot is a normal probability plot. It doesn't look too bad, so the assumption of normally distributed residuals looks okay.

The bottom plot gives p-values for the Ljung-Box-Pierce statistics for each lag up to 20. These statistics consider the accumulated residual autocorrelation from lag 1 up to and including the lag on the horizontal axis. The dashed blue line is at .05. All p-values are above it. That's a good result. We want non-significant values for this statistic when looking at residuals.

All in all, the fit looks good. There's not much need to continue, but just to show you how things looks when incorrect models are used, we will present another model.

Output for a Wrong Model
Suppose that we had misinterpreted the ACF and PACF of the data and had tried an MA(1) model rather than the AR(1) model.

Here's the output:
![Lake Erie MA output](https://pic4.zhimg.com/80/v2-110fbf7c6164894c70e0790a87e36b49.png)

The MA(1) coefficient is significant, but mostly this looks worse than the statistics for the right model. The estimate of the variance is 1.87, compared to 1.447 for the AR(1) model. The AIC and BIC statistics are higher for the MA(1) than for the AR(1). That's not good.

The diagnostic graphs aren't good for the MA(1). The ACF has a significant spike at lag 2 and several of the Ljung-Box-Pierce p-values are below .05. We don't want them there. So, the MA(1) isn't a good model.

![Lake Erie MA Residuals](https://pic4.zhimg.com/80/v2-ca289070b58aebcf917141e88e037eac.png)

A Model with One Too Many Coefficients:

Suppose we try a model (still the Lake Erie Data) with one AR term and one MA term. Here's some of the output:

![Lake Erie ARMA Output](https://pic4.zhimg.com/80/v2-a4532b769e82d0534ef825a8e4876199.png)

> Note!

> The MA(1) coefficient is not significant (z = -0.0909/.1969=-0.4617 is less than 1.96 in absolute value). The MA(1) term could be dropped so that takes us back to the AR(1). Also, the estimate of the variance is barely better than the estimate for the AR(1) model and the AIC and BIC statistics are higher for the ARMA(1,1) than for the AR(1).

### Example 3-2: Parameter Redundancy

Suppose that the model for your data is white noise. If this is true for every  t , then it is true for  t-1  as well, in other words:

$$
\begin{aligned}
x_{t} & =w_{t} \\
x_{t-1} & =w_{t-1}
\end{aligned}
$$

Let's multiply both sides of the second equation by 0.5 :

$$
0.5 x_{t-1}=0.5 w_{t-1}
$$

Next, we will move both terms over to one side:

$$
0=-0.5 x_{t-1}+0.5 w_{t-1}
$$

Because the data is white noise, $x_{t}=w_{t}$, so we can add  $x_{t}$  to the left side and  $w_{t}$  to the right side:

$$
x_{t}=-0.5 x_{t-1}+w_{t}+0.5 w_{t-1}
$$

This is an ARMA(1, 1). The problem is that we know it is white noise because of the original equations. If we looked at the ACF what would we see? You would see the ACF corresponding to white noise, a spike at zero and then nothing else. This also means if we take the white noise process and you try to fit in an ARMA(1, 1), R will do it and will come up with coefficients that looks something like what we have above. This is one of the reasons why we need to look at the ACF and the PACF plots and other diagnostics. We prefer a model with the fewest parameters. This example also says that for certain parameter values, ARMA models can appear very similar to one another.

### R Code for Example 3-1
The session for creating Example 1 then proceeds as follows:
```r
xerie = scan("eriedata.dat") #reads the data
xerie = ts (xerie) # makes sure xerie is a time series object
plot (xerie, type = "b") # plots xerie
acf2 (xerie) # author’s routine for graphing both the ACF and the PACF
sarima (xerie, 1, 0, 0) # this is the AR(1) model estimated with the author’s routine
sarima (xerie, 0, 0, 1) # this is the incorrect MA(1) model
sarima (xerie, 1, 0, 1) # this is the over-parameterized ARMA(1,1) model 
```

Forecasting:
```r
sarima.for(xerie, 4, 1, 0, 0) # four forecasts from an AR(1) model for the erie data 
```

## Diagnostics
### Analyzing possible statistical significance of autocorrelation values
The Ljung-Box statistic, also called the modified Box-Pierce statistic, is a function of the accumulated sample autocorrelations,  $r_{j}$, up to any specified time lag  $m$. As a function of $m$, it is determined as:

$$
Q(m)=n(n+2) \sum_{j=1}^{m} \frac{r_{j}^{2}}{n-j}
$$

where  $n =$  number of usable data points after any differencing operations. 

As an example,

$$
Q(3)=n(n+2)\left(\frac{r_{1}^{2}}{n-1}+\frac{r_{2}^{2}}{n-2}+\frac{r_{3}^{2}}{n-3}\right)
$$

#### Use of the Statistic
This statistic can be used to examine residuals from a time series model in order to see if all underlying population autocorrelations for the errors may be 0 (up to a specified point).

For nearly all models that we consider in this course, the residuals are assumed to be "white noise," meaning that they are identically, independently distributed (from each other). Thus, as we saw last week, the ideal ACF for residuals is that all autocorrelations are 0 . This means that  $Q(m)$  should be 0 for any lag  $m$. A significant  $Q(m)$  for residuals indicates a possible problem with the model.

(Remember  $Q(m)$  measures accumulated autocorrelation up to lag  $m$.)

#### Distribution of $Q(m)$
There are two cases:
1. When the  $r_{j}$  are sample autocorrelations for residuals from a time series model, the null hypothesis distribution of  $Q(m)$  is approximately a  $\chi^{2}$  distribution with  $\mathrm{df}=m-p-q-1$, where  $p+q+1$  is the number of coefficients in the model, including a constant.

> Note!

> $m$  is the lag to which we're accumulating, so in essence, the statistic is not defined until  $m>p+q+1$.

2. When no model has been used, so that the ACF is for raw data, the null distribution of  $Q(m)$  is approximately a  $\chi^{2}$  distribution with  $\mathrm{df}=   m$.

#### p-Value Determination
In both cases, a p-value is calculated as the probability past $Q(m)$ in the relevant distribution. A small p-value (for instance, p-value < .05) indicates the possibility of non-zero autocorrelation within the first $m$ lags.

### Example 3-3
Below there is Minitab output for the Lake Erie level data. A useful model is an AR(1) with a constant. So  $p+q+1=1+0+1=2$.

Final Estimates of Parameters

| **Type**     | **Coef** | **SE Coef** | **T** | **P ** |
|--------------|----------|-------------|-------|--------|
| **AR 1**     | 0.7078   | 0.1161      | 6.10  | 0.000  |
| **Constant** | 4.2761   | 0.1953      | 21.89 | 0.000  |
| **Mean**     | 14.6349  | 0.6684      |       |        |

Modified Box-Pierce (Ljung-Box) Chi-Square statistic
| ** **          | **null** | **null** | **null** | **null** |
|----------------|----------|----------|----------|----------|
| **Lag**        | 12       | 24       | 36       | 48       |
| **Chi-Square** | 9.4      | 23.2     | 30.0     | *        |
| **DF**         | 10       | 22       | 34       | *        |

Minitab gives  $\mathrm{p} -values$ for accumulated lags that are multiples of 12. The  $\mathrm{R}  sarima$ command will give a graph that shows  $p -values$ of the LjungBox-Pierce tests for each lag (in steps of 1) up to a certain lag, usually up to lag 20 for nonseasonal models.

#### Interpretation of the Box-Pierce Results

Notice that the  p -values for the modified Box-Pierce all are well above .05 , indicating "non-significance." This is a desirable result. Remember that there only 40 data values, so there's not much data contributing to correlations at high lags. Thus, the results for  $m=24$  and  $m=36$  may not be meaningful.

## Forecasting with ARIMA Models
In an ARIMA model, we express $x_t$ as a function of past value(s) of x and/or past errors (as well as a present time error). When we forecast a value past the end of the series, we might need values from the observed series on the right side of the equation or we might, in theory, need values that aren’t yet observed.

### Example 3-4
Consider the AR(2) model $x_{t}=\delta+\phi_{1} x_{t-1}+\phi_{2} x_{t-2}+w_{t}$. In this model, $x_{t}$ is a linear function of the values of $x$ at the previous two times. Suppose that we have observed $n$ data values and wish to use the observed data and estimated AR(2) model to forecast the value of $x_{n+1}$ and $x_{n+2}$, the values of the series at the next two times past the end of the series. The equations for these two values are

$$
x_{n+1}=\delta+\phi_{1} x_{n}+\phi_{2} x_{n-1}+w_{n+1} \\
$$
$$
x_{n+2}=\delta+\phi_{1} x_{n+1}+\phi_{2} x_{n}+w_{n+2}
$$

To use the first of these equations, we simply use the observed values of  $x_{n}$ and $x_{n-1}$ and replace $w_{n+1}$ by its expected value of 0 (the assumed mean for the errors).

The second equation for forecasting the value at time $n+2$ presents a problem. It requires the unobserved value of $x_{n+1}$(one time past the end of the series). The solution is to use the forecasted value of  $x_{n+1}$(the result of the first equation).

In general, the forecasting procedure, assuming a sample size of $n$, is as follows:
- For any $w_{j}$ with  $1 \leq \mathrm{j} \leq \mathrm{n}$, use the sample residual for time point $j$ 
- For any $w_{j}$ with  $\mathrm{j}>\mathrm{n}$, use 0 as the value of  $w_{j}$
- For any  $x_{j}$  with  $1 \leq \mathrm{j} \leq \mathrm{n}$, use the observed value of $x_{j}$ 
- For any  $x_{j}$  with  $\mathrm{j}>\mathrm{n}$, use the forecasted value of  $x_{j}$

### Notation
Our authors use the notation  $x_{n+m}^{n}$  to represent a forecast  $m$  times past the end of the observed series. The "superscript" is to be read as "given data up to time  $n$." Other authors use the notation  $x_{n}(m)$  to denote a forecast  $m$  times past time  $n$.

To understand the formula for the standard error of the forecast error, we first need to define the concept of psi-weights.

### Psi-weight representation of an ARIMA model
Any ARIMA model can be converted to an infinite order MA model:

$$
\begin{aligned}
x_{t}-\mu & =w_{t}+\Psi_{1} w_{t-1}+\Psi_{2} w_{t-2}+\cdots+\Psi_{k} w_{t-k}+\ldots \\
& =\sum_{j=0}^{\infty} \Psi_{j} w_{t-j} \text { where } \Psi_{0} \equiv 1
\end{aligned}
$$

An important constraint so that the model doesn't "explode" as we go back in time is

$$
\sum_{j=0}^{\infty}\left|\Psi_{j}\right|<\infty
$$

The process of finding the "psi-weight" representation can involve a few algebraic tricks. Fortunately,  $\mathrm{R}$  has a routine, `ARMAtoMA`, that will do it for us. To illustrate how psi-weights may be determined algebraically, we'll consider a simple example.

### Example 3-5
Suppose that an AR(1) model is $x_{t}=40+0.6 x_{t-1}+w_{t}$

For an AR(1) model, the mean $\mu=\delta /\left(1-\phi_{1}\right)$  so in this case,  $\mu=40 /(1-.6)=100$. We'll define  $z_{t}=x_{t}-100$  and rewrite the model as  $z_{t}=0.6 z_{t-1}+w_{t}$. 

To find the psi-weight expression, we'll continually substitute for the  $z$  on the right side in order to make the expression become one that only involves  $w$  values.

$$
z_{t}=0.6 z_{t-1}+w_{t}, \text { so } z_{t-1}=0.6 z_{t-2}+w_{t-1}
$$

Substitute the right side of the second expression for  $z_{t-1}$  in the first expression.
This gives

$$
z_{t}=0.6\left(0.6 z_{t-2}+w_{t-1}\right)+w_{t}=0.36 z_{t-2}+0.6 w_{t-1}+w_{t}
$$

Next, note that  $z_{t-2}=0.6 z_{t-3}+w_{t-2}$.

Substituting this into the equation gives

$$
z_{t}=0.216 z_{t-3}+0.36 w_{t-2}+0.6 w_{t-1}+w_{t}
$$

If you keep going, you'll soon see that the pattern leads to

$$
z_{t}=x_{t}-100=\sum_{j=0}^{\infty}(0.6)^{j} w_{t-j}
$$

Thus the psi-weights for this model are given by  $\psi_{j}=(0.6)^{j}$  for  $j=0,1, \ldots, \infty$.


In  $\mathrm{R}$, the command `ARMAtomA  (ar=.6, \mathrm{ma}=0, 12)`  gives the first 12 psi-weights. This will give the psi-weights  $\psi_{1}$  to  $\psi_{12}$  in scientific notation.

For the AR(1) with AR coefficient = 0.6  they are:

![Example 3-5 ARMAtoMA](https://pic4.zhimg.com/80/v2-00c587cb707f5ec18e780e1a19922ff1.png)

Remember that  $\psi_{0} \equiv 1$. R doesn't give this value. It's listing starts with  $\psi_{1}$, which equals 0.6 in this case.

**MA Models**: The psi-weights are easy for an MA model because the model already is written in terms of the errors. The psi-weights  = 0  for lags past the order of the MA model and equal the coefficient values for lags of the errors that are in the model. Remember that we always have  $\psi_{0}=1$.

### Standard error of the forecast error for a forecast using an ARIMA model
Without proof, we'll state a result:
The variance of the difference between the forecasted value at time  $n+m$  and the (unobserved) value at time  $n+m$  is

$$
\text { Variance of }\left(x_{n+m}^{n}-x_{n+m}\right)=\sigma_{w}^{2} \sum_{j=0}^{m-1} \Psi_{j}^{2} .
$$

Thus the estimated **standard deviation of the forecast error** at time  n+m  is

$$
\text { Standard error of }\left(x_{n+m}^{n}-x_{n+m}\right)=\sqrt{\widehat{\sigma}_{w}^{2} \sum_{j=0}^{m-1} \Psi_{j}^{2}} \text {. }
$$

> Note!

> The summation of squared psi-weights begins with  $\left(\Psi_{0}\right)^{2}=1$  and that the summation goes to  $m-1$, one less than the number of times ahead for which we're forecasting.

When forecasting  $m=1$  time past the end of the series, the standard error of the forecast error is

$$
\text { Standard error of }\left(x_{n+1}^{n}-x_{n+1}\right)=\sqrt{\widehat{\sigma}_{w}^{2}(1)}
$$

When forecasting the value  m=2  times past the end of the series, the standard error of the forecast error is

$$
\text { Standard error of }\left(x_{n+2}^{n}-x_{n+2}\right)=\sqrt{\widehat{\sigma}_{w}^{2}\left(1+\Psi_{1}^{2}\right)} \text {. }
$$

Notice that the variance will not be too big when  $m=1$. But, as you predict out farther in the future, the variance will increase. When  $m$  is very large, we will get the total variance. In other words, if you are trying to predict very far out, we will get the variance of the entire time series; as if you haven't even looked at what was going on previously.

95\% Prediction Interval for  $x_{n+m}$

With the assumption of normally distributed errors, a  95 \%  prediction interval for  $x_{n+m}$, the future value of the series at time  $n+m$, is

$$
x_{n+m}^{n} \pm 1.96 \sqrt{\widehat{\sigma}_{w}^{2} \sum_{j=0}^{m-1} \Psi_{j}^{2}}.
$$

### Example 3-6
Suppose that an AR(1) model is estimated to be  $x_{t}=40+0.6 x_{t-1}+w_{t}$. This is the same model used earlier in this handout, so the psi-weights we got there apply.

Suppose that we have  $n=100$  observations,  $\widehat{\sigma}_{w}^{2}=4$  and  $x_{100}=80$. We wish to forecast the values at times 101 and 102, and create prediction intervals for both forecasts.

First we forecast time 101.
$$
\begin{array}{l}
x_{101}=40+0.6 x_{100}+w_{101} \\
x_{100}^{100}=40+0.6(80)+0=88
\end{array}
$$

The standard error of the forecast error at time 101 is
$$
\sqrt{\widehat{\sigma}_{w}^{2} \sum_{j=0}^{1-1} \psi_{j}^{2}}=\sqrt{4(1)}=2 .
$$

The  $95 \%$  prediction interval for the value at time 101 is  $88 \pm 2(1.96)$, which is 84.08 to 91.96 . We are therefore  $95 \%$  confident that the observation at time 101 will be between 84.08 and 91.96.

The forecast for time 102 is

$$
x_{102}^{100}=40+0.6(88)+0=92.8
$$

> Note!

> We used the forecasted value for time 101 in the AR(1) equation.
The relevant standard error is
$$
\sqrt{\widehat{\sigma}_{w}^{2} \sum_{j=0}^{2-1} \psi_{j}^{2}}=\sqrt{4\left(1+0.6^{2}\right)}=2.332
$$

A $ 95 \%$  prediction interval for the value at time 102 is  $92.8 \pm(1.96)(2.332)$.

### Example 3-7
In the homework for Lesson 2, problem 5 asked you to suggest a model for a time series of stride lengths measured every 30 seconds for a runner on a treadmill. 

From R, the estimated coefficients for an AR(2) model and the estimated variance are as follows for a similar data set with $n = 90$ observations:

![Coefficients](https://pic4.zhimg.com/80/v2-7de871ade285c1edbec7086c65dfa68b.png)

The command `sarima.for(stridelength, 6, 2, 0, 0) # 6 forecasts with an AR(2) model for stridelength` will give forecasts and standard errors of prediction errors for the next six times past the end of the series. Here’s the output (slightly edited to fit here):

![Errors](https://pic4.zhimg.com/80/v2-5d19289c867f2bb3f095473200e66787.png)

The forecasts are given in the first batch of values under $pred and the standard errors of the forecast errors are given in the last line in the batch of results under $se.

The procedure also gave this graph, which shows the series followed by the forecasts as a red line and the upper and lower prediction limits as blue dashed lines:

![Forecasts and Limits](https://pic4.zhimg.com/80/v2-5e6a0da700a1560ae7627daecdde1ed1.png)

### Psi-Weights for the Estimated  A R(2)  for the Stride Length Data
If we wanted to verify the standard error calculations for the six forecasts past the end of the series, we would need to know the psiweights. To get them, we need to supply the estimated AR coefficients for the AR(2) model to the `ARMAtoMA `command.

The  `\mathrm{R}`  command in this case is `ARMAtoMA (ar = list(1.148,-0.3359) , ma = 0, 5)`

This will give the psi-weights  $\psi_{1}$  to  $\psi_{5}$  in scientific notation. The answer provided by  \mathrm{R}  is:
*[1] 1.148000e+00 9.820040e-01 7.417274e-01 5.216479e-01 3.497056e-01*

(Remember that  $\psi_{0} \equiv 1$  in all cases)

The output for estimating the  \mathrm{AR}(2)  included this estimate of the error variance:

*sigma^2 estimated as 11.47*

As an example, the standard error of the forecast error for 3 times past the end of the series is

$$
\sqrt{\widehat{\sigma}_{w}^{2} \sum_{j=0}^{3-1} \psi_{j}^{2}}=\sqrt{11.47\left(1+1.148^{2}+0.982^{2}\right)}=6.1357
$$

which, except for round off error, matches the value of 6.135493 given as the third standard error in the sarima.for output above.

#### Where will the Forecasts End Up?
For a stationary series and model, the forecasts of future values will eventually converge to the mean and then stay there. Note below what happened with the stride length forecasts, when we asked for 30 forecasts past the end of the series. [Command was `sarima.for (stridelength, 30, 2, 0, 0)`]. The forecast got to 48.74753 and then stayed there.

![Forecasts](https://pic4.zhimg.com/80/v2-62a1de1be88037732829b982cd4920cf.png)

The graph showing the series and the six prediction intervals is the following

![Six Prediction](https://pic4.zhimg.com/80/v2-41fcc78f1387ff6db8f36d494f32bf46.png)