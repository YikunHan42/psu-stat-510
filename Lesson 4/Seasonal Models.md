#! https://zhuanlan.zhihu.com/p/629604549
# Seasonal Models
## Seasonal ARIMA models
Seasonality in a time series is a regular pattern of changes that repeats over  S  time periods, where  S  defines the number of time periods until the pattern repeats again.

For example, there is seasonality in monthly data for which high values tend always to occur in some particular months and low values tend always to occur in other particular months. In this case,  S=12  (months per year) is the span of the periodic seasonal behavior. For quarterly data,  S=4  time periods per year.

In a seasonal ARIMA model, seasonal AR and MA terms predict  $x_{t}$  using data values and errors at times with lags that are multiples of  S  (the span of the seasonality).
- With monthly data (and  S=12), a seasonal first order autoregressive model would use  $x_{t-12}$  to predict  $x_{t}$. For instance, if we were selling cooling fans we might predict this August's sales using last August's sales. (This relationship of predicting using last year's data would hold for any month of the year.)
- A seasonal second order autoregressive model would use  $x_{t-12}$  and  $x_{t-24}$  to predict  $x_{t}$. Here we would predict this August's values from the past two Augusts.
- A seasonal first order MA(1) model (with  S=12) would use  $w_{t-12}$  as a predictor. A seasonal second order MA(2) model would use  $w_{t-12}$  and  $w_{t-24}$

### Differencing
Almost by definition, it may be necessary to examine differenced data when we have seasonality. Seasonality usually causes the series to be nonstationary because the average values at some particular times within the seasonal span (months, for example) may be different than the average values at other times. For instance, our sales of cooling fans will always be higher in the summer months.

#### Seasonal differencing
Seasonal differencing is defined as a difference between a value and a value with lag that is a multiple of  $S$.
- With  S=12 , which may occur with monthly data, a seasonal difference is  $\left(1-B^{12}\right) x_{t}=x_{t}-x_{t-12}$.
The differences (from the previous year) may be about the same for each month of the year giving us a stationary series.
- With  S=4 , which may occur with quarterly data, a seasonal difference is  $\left(1-B^{4}\right) x_{t}=x_{t}-x_{t-4}$.

Seasonal differencing removes seasonal trend and can also get rid of a seasonal random walk type of nonstationarity.

#### Non-seasonal differencing
If trend is present in the data, we may also need non-seasonal differencing. Often (not always) a first difference (non-seasonal) will "detrend" the data. That is, we use  $(1-B) x_{t}=x_{t}-x_{t-1}$  in the presence of trend.

#### Differencing for Trend and Seasonality
When both trend and seasonality are present, we may need to apply both a non-seasonal first difference and a seasonal difference.

That is, we may need to examine the ACF and PACF of  $\left(1-B^{12}\right)(1-B) x_{t}=\left(x_{t}-x_{t-1}\right)-\left(x_{t-12}-x_{t-13}\right)$.

Removing trend doesn't mean that we have removed the dependency. We may have removed the mean,  $\mu_{t}$, part of which may include a periodic component. In some ways we are breaking the dependency down into recent things that have happened and long-range things that have happened.

#### Non-seasonal behavior will still matter...
With seasonal data, it is likely that short run non-seasonal components will still contribute to the model. In the monthly sales of cooling fans mentioned above, for instance, sales in the previous month or two, along with the sales from the same month a year ago, may help predict this month's sales.

We'll have to look at the ACF and PACF behavior over the first few lags (less than S) to assess what non-seasonal terms might work in the model.

### Seasonal ARIMA Model
The seasonal ARIMA model incorporates both non-seasonal and seasonal factors in a multiplicative model. One shorthand notation for the model is

$$
\text { ARIMA }(p, d, q) \times(P, D, Q) S
$$

with $p$ =  non-seasonal AR order,  $d$ =  non-seasonal differencing,  $q$ =  non-seasonal MA order,  $P$ =  seasonal AR order,  $D$ =  seasonal differencing,  $Q$ =  seasonal MA order, and  $S$ =  time span of repeating seasonal pattern.

Without differencing operations, the model could be written more formally as

$$
\text { (1) } \Phi\left(B^{S}\right) \phi(B)\left(x_{t}-\mu\right)=\Theta\left(B^{S}\right) \theta(B) w_{t}
$$

The non-seasonal components are:
- AR:  $\phi(B)=1-\phi_{1} B-\ldots-\phi_{p} B^{p}$ 
- MA:  $\theta(B)=1+\theta_{1} B+\ldots+\theta_{q} B^{q}$

The seasonal components are:
- Seasonal AR:  $\Phi\left(B^{S}\right)=1-\Phi_{1} B^{S}-\ldots-\Phi_{P} B^{P S}$ 
- Seasonal MA:  $\Theta\left(B^{S}\right)=1+\Theta_{1} B^{S}+\ldots+\Theta_{Q} B^{Q S}$

> Note!
> On the left side of equation (1) the seasonal and non-seasonal AR components multiply each other, and on the right side of equation (1) the seasonal and non-seasonal MA components multiply each other.

### Example 4-1: ARIMA  $(0,0,1) \times(0,0,1)_{12}$

The model includes a non-seasonal MA(1) term, a seasonal MA(1) term, no differencing, no AR terms and the seasonal period is  $S=12$.

The non-seasonal MA(1) polynomial is  $\theta(B)=1+\theta_{1} B$.

The seasonal MA(1) polynomial is  $\Theta\left(B^{12}\right)=1+\Theta_{1} B^{12}$.

The model is  $\left(x_{t}-\mu\right)=\Theta\left(B^{12}\right) \theta(B) w_{t}=\left(1+\Theta_{1} B^{12}\right)\left(1+\theta_{1} B\right) w_{t}$.

When we multiply the two polynomials on the right side, we get

$$
\begin{array}{c}
\left(x_{t}-\mu\right)=\left(1+\theta_{1} B+\Theta_{1} B^{12}+\theta_{1} \Theta_{1} B^{13}\right) w_{t} \\
=w_{t}+\theta_{1} w_{t-1}+\Theta_{1} w_{t-12}+\theta_{1} \Theta_{1} w_{t-13} .
\end{array}
$$

Thus the model has MA terms at lags 1, 12, and 13. This leads many to think that the identifying ACF for the model will have non-zero autocorrelations only at lags 1, 12, and 13. There's a slight surprise here. **There will also be a non-zero autocorrelation at lag 11**. We supply a proof in the Appendix below.

We simulated  $n=1000$  values from an ARIMA  $(0,0,1) \times(0,0,1)_{12}$. The non-seasonal MA (1) coefficient was  $\theta_{1}=0.7$. The seasonal MA( 1 ) coefficient was  $\Theta_{1}=0.6$. The sample ACF for the simulated series was as follows:

![Example 4-1](https://pic4.zhimg.com/80/v2-149f53db3a14e9ae81139ab462d3561e.png)

> Note!
> The spikes at lags 1,11 , and 12 in the ACF. This is characteristic of the ACF for the ARIMA  $(0,0,1) \times(0,0,1)_{12}$. Because this model has nonseasonal and seasonal MA terms, the PACF tapers nonseasonally, following  $\operatorname{lag}$ 1 , and tapers seasonally, that is near  $\mathrm{S}=12$, and again near lag  $2*S=24$.

### Example 4-2: ARIMA  $(1,0,0) \times(1,0,0)_{12}$
The model includes a non-seasonal AR(1) term, a seasonal AR(1) term, no differencing, no MA terms and the seasonal period is  $S=12$.

The non-seasonal AR(1) polynomial is  $\phi(B)=1-\phi_{1} B$.

The seasonal AR(1) polynomial is  $\Phi\left(B^{12}\right)=1-\Phi_{1} B^{12}$.

The model is  $\left(1-\phi_{1} B\right)\left(1-\Phi_{1} B^{12}\right)\left(x_{t}-\mu\right)=w_{t}$.

If we let  $z_{t}=x_{t}-\mu$  (for simplicity), multiply the two AR components and push all but  $z_{t}$  to the right side we get  $z_{t}=\phi_{1} z_{t-1}+\Phi_{1} z_{t-12}+\left(-\phi_{1} \Phi_{1}\right) z_{t-13}+w_{t}$

This is an AR model with predictors at lags 1,12 and 13.

R can be used to determine and plot the PACF for this model, with  $\phi_{1}=.6$  and  $\Phi_{1}=.5$. That PACF (partial autocorrelation function) is:

![Eaxmple 4-2](https://pic4.zhimg.com/80/v2-b9ade81df1c0a46f5a5692745667582b.png)

It's not quite what you might expect for an AR model, but it almost is. There are distinct spikes at lags 1, 12, and 13 with a bit of action coming before lag 12. Then, it cuts off after lag 13.

```r
thepacf=ARMAacf (ar = c(.6,0,0,0,0,0,0,0,0,0,0,.5,-.30),lag.max=30,pacf=T)
plot (thepacf,type="h")
```

### Identifying a Seasonal Model
**Step 1: Do a time series plot of the data.**

Examine it for features such as trend and seasonality. You'll know that you've gathered seasonal data (months, quarters, etc.,) so look at the pattern across those time units (months, etc.) to see if there is indeed a seasonal pattern.

**Step 2: Do any necessary differencing.**

If there is seasonality and no trend, then take a difference of lag $S$. For instance, take a 12th difference for monthly data with seasonality. Seasonality will appear in the ACF by tapering slowly at multiples of S. View the TS plot and ACF/PACF plots for an example of data that requires a seasonal difference. 

![TS plot and ACF/PACF plots](https://pic4.zhimg.com/80/v2-c9a30bcbe7c4c727fa1d1341decb1d45.png)

Note that the TS plot shows a clear seasonal pattern that repeats over 12 time points. Seasonal differences are supported in the ACF/PACF of the original data because the first seasonal lag in the ACF is close to 1 and decays slowly over multiples of  S & = 12 . Once seasonal differences are taken, the ACF/PACF  plots of twelfth differences support a seasonal AR(1) pattern.

![ACF/PACF plots](https://pic4.zhimg.com/80/v2-54b6244036dd536094fbb44443522c16.png)

If there is linear trend and no obvious seasonality, then take a first difference. If there is a curved trend, consider a transformation of the data before differencing.

If there is both trend and seasonality, apply a seasonal difference to the data and then re-evaluate the trend. If a trend remains, then take first differences. For instance, if the series is called  $x$, the commands in  $\mathrm{R}$  would be:

```r
diff12=diff(x, 12)
plot(diff12)
acf2(diff12)
diff1and12 = diff(diff12, 1) 
```

If there is neither obvious trend nor seasonality, don't take any differences.

**Step 3: Examine the ACF and PACF of the differenced data (if differencing is necessary).**

We're using this information to determine possible models. This can be tricky going involving some (educated) guessing. Some basic guidance:
- **Non-seasonal terms**: Examine the early lags  $(1,2,3, \ldots)$  to judge non-seasonal terms. Spikes in the ACF (at low lags) with a tapering PACF indicate non-seasonal MA terms. Spikes in the PACF (at low lags) with a tapering ACF indicate possible non-seasonal AR terms.
- Seasonal terms: Examine the patterns across lags that are multiples of  $S$. For example, for monthly data, look at lags 12, 24, 36, and so on (probably won't need to look at much more than the first two or three seasonal multiples). Judge the ACF and PACF at the seasonal lags in the same way you do for the earlier lags.

**Step 4: Estimate the model(s) that might be reasonable on the basis of step 3.**

Don't forget to include any differencing that you did before looking at the ACF and PACF. In the software, specify the original series as the data and then indicate the desired differencing when specifying parameters in the `arima` command that you're using.

**Step 5: Examine the residuals (with ACF, Box-Pierce, and any other means) to see if the model seems good.**

Compare AIC or BIC values to choose among several models.

If things don't look good here, it's back to Step 3 (or maybe even Step 2).

### Example 4-3
The data series are a monthly series of a measure of the flow of the Colorado River, at a particular site, for $n = 600$ consecutive months.

**Step 1**
A time series plot is
![Colorado River Time Series](https://pic4.zhimg.com/80/v2-7e7c4d70970f5a6893aaea8552d3afcc.png)

With so many data points, it's difficult to judge whether there is seasonality. If it was your job to work on data like this, you probably would know that river flow is seasonal – perhaps likely to be higher in the late spring and early summer, due to snow runoff.

Without this knowledge, we might determine means by month of the year. Below is a plot of means for the 12 months of the year. It's clear that there are monthly differences (seasonality).

![Colorado River Monthly Means](https://pic4.zhimg.com/80/v2-8902d16c4464a81adc2bef867fdbac62.png)

Looking back at the time series plot, it's hard to judge whether there's any long run trend. If there is, it's slight.

**Steps 2 and 3**

We might try the idea that there is seasonality, but no trend. To do this, we can create a variable that gives the 12th differences (seasonal differences), calculated as $x_t-x_{t-12}$
. Then, we look at the ACF and the PACF for the $12^{th}$ difference series (not the original data). Here they are:

![Colorado River ACF and PACF](https://pic4.zhimg.com/80/v2-dde9e1833f9c46cc7ef9f910f42eaad2.png)

- **Non-seasonal behavior**: The PACF shows a clear spike at lag 1 and not much else until about lag 11. This is accompanied by a tapering pattern in the early lags of the ACF. A non-seasonal AR(1) may be a useful part of the model.
- **Seasonal behavior**: We look at what's going on around lags 12, 24, and so on. In the ACF, there's a cluster of (negative) spikes around lag 12 and then not much else. The PACF tapers in multiples of  S ; that is the PACF has significant lags at  12,24,36  and so on. This is similar to what we saw for a seasonal MA(1) component in Example 1 of this lesson.

Remembering that we're looking at  $12^{\text {th }}$  differences, the model we might try for the original series is ARIMA  $(1,0,0) \times(0,1,1)_{12}$.

**Step 4**

R results for the ARIMA  $(1,0,0) \times(0,1,1)_{12}$ :

![R Results](https://pic4.zhimg.com/80/v2-f16375fa81575a9fdb5ab19fa8be2d42.png)

**Step 5 (diagnostics)**

The normality and Box-Pierce test results are shown in Lesson 4.2.  Besides normality, things look good.  The Box-Pierce statistics are all non-significant and the estimated ARIMA coefficients are statistically significant.

The ACF of the residuals looks good too:
![ACF](https://pic4.zhimg.com/80/v2-a832771dcd7d92b8e850cb9fc6b29b0f.png)

What doesn't look perfect is a plot of residuals versus fits. There's non-constant variance.

![Residuals Versus Fits](https://pic4.zhimg.com/80/v2-ca48ccd954aa437b72c6eb748371389e.png)

We've got three choices for what to do about the non-constant variance: (1) ignore it, (2) go back to step 1 and try a variance stabilizing transformation like log or square root, or (3) use an ARCH model that includes a component for changing variances. We'll get to ARCH models later in the course.

### Appendix (Optional reading)
In Example 4-1, we promised a proof that  $\rho_{11} \neq 0$  for ARIMA  $(0,0,1) \times(0,0,1)_{12}$.

A correlation is defined as Covariance/ product of standard deviations.

The covariance between  $x_{t}$  and  $x_{t-11}=E\left(x_{t}-\mu\right)\left(x_{t-11}-\mu\right)$.

For the model in Example 1,

$$
x_{t}-\mu=w_{t}+\theta_{1} w_{t-1}+\Theta_{1} w_{t-12}+\theta_{1} \Theta_{1} w_{t-13} \\
x_{t-11}-\mu=w_{t-11}+\theta_{1} w_{t-12}+\Theta_{1} w_{t-23}+\theta_{1} \Theta_{1} w_{t-24}
$$

The covariance between  $x_{t}$  and  x_
${t-11}$ 

$$
(2)  \mathrm{E}\left(w_{t}+\theta_{1} w_{t-1}+\Theta_{1} w_{t-12}+\theta_{1} \Theta_{1} w_{t-13}\right)\left(w_{t-11}+\theta_{1} w_{t-12}+\Theta_{1} w_{t-23}+\theta_{1} \Theta_{1} w_{t-24}\right)
$$

The w's are independent errors. The expected value of any product involving $\omega$'s with different subscripts will be 0 . A covariance between $\omega$s with the same subscripts will be the variance of  $\omega$.

If you inspect all possible products in expression 2 , there will be one product with matching subscripts. They have lag  $t-12$. Thus this expected value (covariance) will be different from 0.

This shows that the lag 11 autocorrelation will be different from 0 . If you look at the more general problem, you can find that only lags 1 , 11,12 , and 13 have non-zero autocorrelations for the  $\operatorname{ARIMA}(0,0,1) \times(0,0,1)_{12}$.

A seasonal ARIMA model incorporates both non-seasonal and seasonal factors in a multiplicative fashion.

## Identifying Seasonal Models and R Code
Example 3 described the analysis of monthly flow data for a Colorado River location. An ARIMA $(1,0,0)×(0,1,1)_{12}$ was identified and estimated. In the first part of this lesson, you'll see the R code and output for that analysis.

### Example 4-3: Revisited
R code for the Colorado River Analysis

The data are in the file coloradoflow.dat. We used scripts in the library, so the first two lines of code are:

```r
library(astsa)
flow <- ts(scan("coloradoflow.dat"))
```

The `acf2` command asks for information about 48 lags. On the basis of the ACF and PACF of the  $12^{\text {th }}$  differences, we identified an ARIMA  $(1,0,0) \times(0,1,1)_{12}$  model as a possibility. The command for fitting this model is

```r
sarima(flow, 1,0,0,0,1,1,12)
```

The parameters of the command just given are the data series, the non-seasonal specification of AR, differencing, and MA, and then the seasonal specification of seasonal AR, seasonal differencing, seasonal MA, and period or span for the seasonality.

Output from the sarima command is:
![Output](https://pic4.zhimg.com/80/v2-dc5e1d64791d311d25e0effd2bcb6d3b.png)

The output included these residual plots. The only difficulty we see is in the normal probability plot. The extreme standardized sample residuals (on both ends) are larger than they would be for normally distributed data. This could be related to the non-constant variance.

![Standardized Residuals](https://pic4.zhimg.com/80/v2-63eb42b52d04ac26cd4c70cad88aca38.png)

We didn't generate any forecasts, but if we were to use R to generate forecasts for the next 24 months in R, one command that could be used is

```r
sarima.for(flow, 24, 1,0,0,0,1,1,12)
```

> Note!
> The order of parameters in the command is the name of the data series, the number of times for which we want forecasts, followed by the parameters of the ARIMA model.

Partial output for the forecast command follows (We skipped giving the standard errors.)

![Output](https://pic4.zhimg.com/80/v2-bac652fecda09ad42e6df57d8edb3d32.png)

![Time Series](https://pic4.zhimg.com/80/v2-94c9d699b35949b799febe0a7df95178.png)

Note that the lower limits of some prediction intervals are negative - impossible for the flow of a river. In practice, we might truncate these lower limits to 0 when presenting them.

If you were to use R's native commands to do the fit and forecasts, the commands might be:

```r
themodel = arima(flow, order = c(1,0,0), seasonal = list(order = c(0,1,1), period = 12))
themodel
predict(themodel, n.ahead=24) 
```

The first command does the arima and stores results in an “object” called “themodel.” The second command, which is simply themodel, lists the results and the final command generates forecasts for 24 times ahead.

We presented a graph of the monthly means to make the case that the data are seasonal. The R commands are:

```r
flowm = matrix(flow, ncol=12,byrow=TRUE)
col.means=apply(flowm,2,mean)
plot(col.means,type="b", main="Monthly Means Plot for Flow", xlab="Month", ylab="Mean")
```

![Monthly Means](https://pic4.zhimg.com/80/v2-8902d16c4464a81adc2bef867fdbac62.png)

### Example 4-4: Beer Production in Australia
This plot for quarterly beer production in Australia was given.

![Time Series](https://pic4.zhimg.com/80/v2-8d3f4a1f8688ae8867fab25638a41c05.png)

There is an upward trend and seasonality. The regular pattern of ups and downs is an indicator of seasonality. If you count points, you'll see that the  $4^{\text {th }}$  quarter of a year is the high point, the  $2^{\text {nd }}$  and  $3^{\text {rd }}$  quarters are low points, and the  $1^{\text {st }}$  quarter value falls in between.

With trend and quarterly seasonality we will likely need both a first and a fourth difference. The plot above was produced in Minitab, but we'll use R for the rest of this example. The commands for creating the differences and graphing the ACF and PACF of these differences are:

```r
diff4 = diff(beer, 4)
diff1and4 = diff(diff4,1)
acf2(diff1and4,24)
```

> Note!
> When we read the data, we called the series “beer” and also loaded the astsa library.

Let's examine the TS plot and ACF/PACF of seasonal differences, diff4, to determine whether or not first differences are also necessary.

![TS and ACF/PACF](https://pic4.zhimg.com/80/v2-ac4d5c1816e1cb4515c2dd7b083391b2.png)

The TS plot still shows that the upward trend remains, however the ACF/PACF do not suggest the need to difference further. In this case, we could detrend by subtracting an estimated linear trend from each observation as follows:

```r
The TS plot still shows that the upward trend remains, however the ACF/PACF do not suggest the need to difference further. In this case, we could detrend by subtracting an estimated linear trend from each observation as follows:
```

The ACF and PACF of the detrended seasonally differenced data follow. The interpretation:

- **Non-seasonal:** Looking at just the first 2 or 3 lags, either a MA(1) or AR(1) might work based on the similar single spike in the ACF and PACF, if at all. Both terms are also possible with an ARMA(1,1), but with both cutting off immediately, this is less likely than a single order model. With  S=4 , the nonseasonal aspect can sometimes be difficult to interpret in such a narrow window.
- **Seasonal:** Look at lags that are multiples of 4 (we have quarterly data). Not much is going on there, although there is a (barely) significant spike in the ACF at lag 4 and a somewhat confusing spike at lag 9 (in ACF). Nothing significant is happening at the higher lags. Maybe a seasonal MA(1) or MA(2) might work.

![dtb ACF/PACF](https://pic4.zhimg.com/80/v2-fe81ef8223bd3839371e1c0a805a9d50.png)

We tried a few models. An initial guess was ARIMA  $(0,0,1) \times(0,1,1)_{4}$, which wasn't a bad guess. We also tried ARIMA  $(1,0,0) \times(0,1,1)_{4}$, $\operatorname{ARIMA}(1,0,1) \times(0,1,1)_{4}$, and  $\operatorname{ARIMA}(1,0,0) \times(0,1,2)_{4}$

The commands for the 4 models are:
```r
sarima (dtb, 0,0,1,0,0,1,4)
sarima (dtb, 1,0,0,0,0,1,4)
sarima (dtb, 0,0,0,0,0,1,4)
sarima (dtb, 1,0,0,0,0,2,4)
```

A summary of the results:
![Summary](https://pic4.zhimg.com/80/v2-abd483dca12fe53743635bf8613db39a.png)

All models are quite close, though the second model is the best in terms of AIC and residual autocorrelation and might be worth keeping the marginally significant AR(1) term. The fourth model surprisingly comes through with a slightly lower MSE than the second, though the AIC and simplicity of the second model seems the best choice.

> Note!
> There are a number of tests to help assess the need for differencing because it is not always obvious. The augmented Dickey-Fuller (ADF) test and the KPSS test are two common choices. The ADF tests the null hypothesis that the data are not stationary, more specifically that the process contains a unit root. When we fail to reject the null, then this suggests the need to difference. The KPSS test reverses the null and alternative hypothesis of the ADF test. If the KPSS test rejects, then the alternative would be to conclude that the data are not stationary and to take first differences. Both tests are available in the R package tseries.

> Note!
> You may wish to examine this model with first and fourth differences. Differencing twice usually removes any drift from the model and so sarima does not fit a constant when d=1 and D=1. In this case you may difference within the sarima command, e.g. sarima(x,1,1,1,0,1,1,S). However there are cases, when drift remains after differencing twice and so you must difference outside of the sarima command to fit a constant. It seems the safest choice is to difference outside of the sarima command to first verify that the drift is gone.