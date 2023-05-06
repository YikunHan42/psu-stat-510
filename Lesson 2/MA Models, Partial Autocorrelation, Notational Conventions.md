# MA Models, Partial Autocorrelation, Notational Conventions
## Moving Average Models (MA models)

ARIMA include *autoregressive* and/or *moving average* terms.

A moving average term in a time series model is a past error (multiplied by a coefficient).

Let  $w_{t} \stackrel{i i d}{\sim} N\left(0, \sigma_{w}^{2}\right)$.

The  $1^{\text {st }}$  order moving average model, denoted by  M A(1)  is:

$$x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}$$

The  $2^{\text {nd }}$  order moving average model, denoted by  M A(2)  is:

$$x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}+\theta_{2} w_{t-2}$$

The  $q^{\text {th }}$  order moving average model, denoted by MA(q) is:

$$x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}+\theta_{2} w_{t-2}+\cdots+\theta_{q} w_{t-q}$$

> Note!

> Many textbooks and software programs define the model with negative signs before the $\theta$
 terms. This doesn’t change the general theoretical properties of the model, although it does flip the algebraic signs of estimated coefficient values and (unsquared) $\theta$
 terms in formulas for ACFs and variances. You need to check your software to verify whether negative or positive signs have been used in order to correctly write the estimated model. R uses positive signs in its underlying model, as we do here.

### Theoretical Properties of a Time Series with an MA(1) Model

- Mean is  $E\left(x_{t}\right)=\mu$ 
- Variance is  $\operatorname{Var}\left(x_{t}\right)=\sigma_{w}^{2}\left(1+\theta_{1}^{2}\right)$ 
- Autocorrelation function (ACF) is:
$$
\rho_{1}=\frac{\theta_{1}}{1+\theta_{1}^{2}}, \text { and } \rho_{h}=0 \text { for } h \geq 2
$$
> Note!

> That the only nonzero value in the theoretical ACF is for lag 1. All other autocorrelations are 0. Thus a sample ACF with a significant autocorrelation only at lag 1 is an indicator of a possible MA(1) model.

### Example 2-1
Suppose that an MA(1) model is  $x_{t}=10+w_{t}+.7 w_{t-1}$ , where  $w_{t} \stackrel{i i d}{\sim} N(0,1)$ . Thus the coefficient  $\theta_{1}=0.7$ . The theoretical ACF is given by:

$$
\rho_{1}=\frac{0.7}{1+0.7^{2}}=0.4698, \text { and } \rho_{h}=0 \text { for all lags } h \geq 2
$$

![ACF for MA(1) with theta1=0.7](https://pic4.zhimg.com/80/v2-28f7ed16164f56d02293c6726a87de8f.png)

The plot just shown is the theoretical ACF for an MA(1) with  $\theta_{1}=0.7$ . In practice, a sample won't usually provide such a clear pattern. Using  R, we simulated  $\mathrm{n}=100$  sample values using the model  $x_{t}=10+w_{t}+.7 w_{t-1}$  where  $w_{t}{ }^{i i d} \sim N(0,1)$ . For this simulation, a time series plot of the sample data follows. We can't tell much from this plot.

![Simulated MA(1) data](https://pic4.zhimg.com/80/v2-9a9902f55976a491a206ed645e5a6d8c.png)

The sample ACF for the simulated data follows. We see a “spike” at lag 1 followed by generally non-significant values for lags past 1. Note that the sample ACF does not match the theoretical pattern of the underlying MA(1), which is that all autocorrelations for lags past 1 will be 0. A different sample would have a slightly different sample ACF shown below, but would likely have the same broad features.

![ACF for simulated sample data](https://pic4.zhimg.com/80/v2-10fe2b4aa2a8ef7cc06aaaa767b57819.png)

### Theoretical Properties of a Time Series with an MA(2) Model

For the  MA(2)  model, theoretical properties are the following:
- Mean is  $E\left(x_{t}\right)=\mu$ 
- Variance is  $\operatorname{Var}\left(x_{t}\right)=\sigma_{w}^{2}\left(1+\theta_{1}^{2}+\theta_{2}^{2}\right)$
- Autocorrelation function (ACF) is:

$$
\rho_{1}=\frac{\theta_{1}+\theta_{1} \theta_{2}}{1+\theta_{1}^{2}+\theta_{2}^{2}}, \rho_{2}=\frac{\theta_{2}}{1+\theta_{1}^{2}+\theta_{2}^{2}}, \text { and } \rho_{h}=0 \text { for } h \geq 3
$$

> Note!

> The only nonzero values in the theoretical ACF are for lags 1 and 2. Autocorrelations for higher lags are 0. So, a sample ACF with significant autocorrelations at lags 1 and 2, but non-significant autocorrelations for higher lags indicates a possible MA(2) model.

### Example 2-2
Consider the MA(2) model  $x_{t}=10+w_{t}+.5 w_{t-1}+.3 w_{t-2}$ , where  $w_{t} \stackrel{i i d}{\sim} N(0,1)$ . The coefficients are  $\theta_{1}=0.5$  and  $\theta_{2}=0.3$ . Because this is an MA(2), the theoretical ACF will have nonzero values only at lags 1 and 2 .
Values of the two nonzero autocorrelations are:

$$
\rho_{1}=\frac{0.5+0.5 \times 0.3}{1+0.5^{2}+0.3^{2}}=0.4851 \text { and } \rho_{2}=\frac{0.3}{1+0.5^{2}+0.3^{2}}=0.2239
$$

A plot of the theoretical ACF follows:
![ACF for MA(2)](https://pic4.zhimg.com/80/v2-e9ede675be49fa3a0e7d49d412c3380d.png)

As nearly always is the case, sample data won't behave quite so perfectly as theory. We simulated  $n=150$  sample values for the model  $x_{t}=10+w_{t}+.5 w_{t-1}+.3 w_{t-2}$ , where  $w_{t} \stackrel{i i d}{\sim} N(0,1)$ . The time series plot of the data follows. **As with the time series plot for the MA(1) sample data, you can't tell much from it.**

![Simulated MA(2) Series](https://pic4.zhimg.com/80/v2-c004f214adc0ca3327cfcabee8484d76.png)

The sample ACF for the simulated data follows. The pattern is typical for situations where an MA(2) model may be useful. **There are two statistically significant “spikes” at lags 1 and 2 followed by non-significant values for other lags.** Note that due to sampling error, the sample ACF did not match the theoretical pattern exactly.

![ACF for simulated MA(2) Data](https://pic4.zhimg.com/80/v2-9276aedaf976a71f74bdd9519d218424.png)

### ACF for General MA(q) Models
A property of  \mathrm{MA}(\mathrm{q})  models in general is that there are nonzero autocorrelations for the first  q  lags and autocorrelations  =0  for all lags  >  q.

**Non-uniqueness of connection between values of  $\theta_{1}$  and  $\rho_{1}$  in MA(1) Model.**

In the MA(1)  model, for any value of  $\theta_{1}$ , the reciprocal  $1 / \theta_{1}$  gives the same value for:

$$
\rho_{1}=\frac{\theta_{1}}{1+\theta_{1}^{2}}
$$

As an example, use +0.5 for  $\theta_{1}$ , and then use  $1 /(0.5)=2$  for  $\theta_{1}$. You'll get  $\rho_{1}=0.4$ in both instances.

To satisfy a theoretical restriction called **invertibility**, we restrict MA(1) models to have values with absolute value less than 1 . In the example just given,  $\theta_{1}=0.5$  will be an allowable parameter value, whereas  $\theta_{1}=1 / 0.5=2$  will not.

### Invertibility of MA models
An MA model is said to be invertible if it is algebraically equivalent to a converging infinite order AR model. By converging, we mean that the AR coefficients decrease to 0 as we move back in time.

>Advanced Theory Note!

>For a MA(q) model with a specified ACF, there is only one invertible model. The necessary condition for invertibility is that the  $\theta$  coefficients have values such that the equation  $1-\theta_{1} y-\ldots-\theta_{q} y^{q}=0$  has solutions for  $y$  that fall outside the unit circle.

> **connection between $\omega$ and $x$**

### R Code for the Examples
For the pdf version, view on [output](https://github.com/YikunHan42/psu-stat-510/blob/main/Lesson%202/MA-Models%2C-Partial-Autocorrelation%2C-Notational-Conventions.pdf)

Example 1
```r
acfma1=ARMAacf(ma=c(0.7), lag.max=10) # 10 lags of ACF for MA(1) with theta1 = 0.7
lags=0:10 #creates a variable named lags that ranges from 0 to 10.
plot(lags,acfma1,xlim=c(1,10), ylab="r",type="h", main = "ACF for MA(1) with theta1 = 0.7")
abline(h=0) #adds a horizontal axis to the plot 
```

simultion and plots
```r
xc=arima.sim(n=150, list(ma=c(0.7))) #Simulates n = 150 values from MA(1)
x=xc+10 # adds 10 to make mean = 10. Simulation defaults to mean = 0.
plot(x,type="b", main="Simulated MA(1) data")
acf(x, xlim=c(1,10), main="ACF for simulated sample data") 
```

Example 2
```r
acfma2=ARMAacf(ma=c(0.5,0.3), lag.max=10)
acfma2
lags=0:10
plot(lags,acfma2,xlim=c(1,10), ylab="r",type="h", main = "ACF for MA(2) with theta1 = 0.5,theta2=0.3")
abline(h=0)
xc=arima.sim(n=150, list(ma=c(0.5, 0.3)))
x=xc+10
plot(x, type="b", main = "Simulated MA(2) Series")
acf(x, xlim=c(1,10), main="ACF for simulated MA(2) Data")
```

### Appendix: Proof of Properties of MA(1)
The  *$1^{\text {st }}$  order moving average model*, denoted by MA(1) is  $x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}$ , where  $w_{t} \stackrel{i i d}{\sim} N\left(0, \sigma_{w}^{2}\right)$

+ Mean:  $E\left(x_{t}\right)=E\left(\mu+w_{t}+\theta_{1} w_{t-1}\right)=\mu+0+\left(\theta_{1}\right)(0)=\mu$ 
+ Variance:  $$\operatorname{Var}\left(x_{t}\right)=\operatorname{Var}\left(\mu+w_{t}+\theta_{1} w_{t-1}\right)=0+\operatorname{Var}\left(w_{t}\right)+\operatorname{Var}\left(\theta_{1} w_{t-1}\right)=\sigma_{w}^{2}+\theta_{1}^{2} \sigma_{w}^{2}=\left(1+\theta_{1}^{2}\right) \sigma_{w}^{2} $$
ACF: Consider the covariance between  $x_{t}$  and  $x_{t-h}$ . This is  $E\left(x_{t}-\mu\right)\left(x_{t-h}-\mu\right)$ , which equals

$$
E\left[\left(w_{t}+\theta_{1} w_{t-1}\right)\left(w_{t-h}+\theta_{1} w_{t-h-1}\right)\right]=E\left[w_{t} w_{t-h}+\theta_{1} w_{t-1} w_{t-h}+\theta_{1} w_{t} w_{t-h-1}+\theta_{1}^{2} w_{t-1} w_{t-h-1}\right]
$$

When  $h=1$ , the previous expression  =$\theta_{1} \sigma_{w}^{2}$ . For any  h \geq 2 , the previous expression  =0.
For a time series,

$$
\rho_{h}=\frac{\text { Covariance for lag } \mathrm{h}}{\text { Variance }}
$$

Apply this result to get the ACF given above.

### Invertibility Restriction
The MA(1) model can be written as  $x_{t}-\mu=w_{t}+\theta_{1} w_{t-1}$.

If we let  $z_{t}=x_{t}-\mu$, then the MA(1) model is
$$
z_{t}=w_{t}+\theta_{1} w_{t-1} \quad (1)
$$

At time  $t-1$ , the model is  $z_{t-1}=w_{t-1}+\theta_{1} w_{t-2}$  which can be reshuffled to

$$
w_{t-1}=z_{t-1}-\theta_{1} w_{t-2} \quad (2)
$$
We then substitute relationship (2) for  $w_{t-1}$  in equation (1)
$$
z_{t}=w_{t}+\theta_{1}\left(z_{t-1}-\theta_{1} w_{t-2}\right)=w_{t}+\theta_{1} z_{t-1}-\theta^{2} w_{t-2} \quad (3)
$$

At time  $t-2$ , equation (2) becomes
$$
w_{t-2}=z_{t-2}-\theta_{1} w_{t-3} \quad(4)
$$

We then substitute relationship (4) for  $w_{t-2}$  in equation (3)

$$
z_{t}=w_{t}+\theta_{1} z_{t-1}-\theta_{1}^{2} w_{t-2}=w_{t}+\theta_{1} z_{t-1}-\theta_{1}^{2}\left(z_{t-2}-\theta_{1} w_{t-3}\right)=w_{t}+\theta_{1} z_{t-1}-\theta_{1}^{2} z_{t-2}+\theta_{1}^{3} w_{t-3}
$$

If we were to continue (infinitely), we would get the infinite order AR model

$$
z_{t}=w_{t}+\theta_{1} z_{t-1}-\theta_{1}^{2} z_{t-2}+\theta_{1}^{3} z_{t-3}-\theta_{1}^{4} z_{t-4}+\ldots
$$

> Note!

> However, that if  $\left|\theta_{1}\right| \geq 1$, the coefficients multiplying the lags of  z  will increase (infinitely) in size as we move back in time. To prevent this, we need  $\left|\theta_{1}\right|<1$. This is the condition for an invertible MA(1) model.

### Infinite Order MA model
An AR(1) model can be converted to an infinite order MA model:

$$
x_{t}-\mu=w_{t}+\phi_{1} w_{t-1}+\phi_{1}^{2} w_{t-2}+\cdots+\phi_{1}^{k} w_{t-k}+\cdots=\sum_{j=0}^{\infty} \phi_{1}^{j} w_{t-j}
$$

This summation of past white noise terms is known as the **causal representation** of an AR(1). In other words,  $x_{t}$  is a special type of MA with an infinite number of terms going back in time. This is called an infinite order MA or MA(  $\infty$) . A finite order MA is an infinite order AR and any finite order AR is an infinite order MA.

A requirement for a stationary  $\operatorname{AR}(1)$  is that  $\left|\phi_{1}\right|<1$. Let's calculate the  \operatorname{Var}\left(x_{t}\right)  using the causal representation.

$$
\operatorname{Var}\left(x_{t}\right)=\operatorname{Var}\left(\sum_{j=0}^{\infty} \phi_{1}^{j} w_{t-j}=\sum_{j=0}^{\infty} \operatorname{Var}\left(\phi_{1}^{j} w_{t-j}\right)=\sum_{j=0}^{\infty} \phi_{1}^{2 j} \sigma_{w}^{2}=\sigma_{w}^{2} \sum_{j=0}^{\infty} \phi_{1}^{2 j}=\frac{\sigma_{w}^{2}}{1-\phi_{1}^{2}}\right)
$$

This last step uses a basic fact about geometric series that requires  $\left|\phi_{1}\right|<1$; otherwise the series diverges.

## Partial Autocorrelation Function (PACF)
In general, a partial correlation is a conditional correlation.

In regression, this partial correlation could be found by correlating the residuals from two different regressions:
1. Regression in which we predict  y  from  $x_{1}$  and  $x_{2}$,
2. regression in which we predict  $x_{3}$  from  $x_{1}$  and  $x_{2}$. Basically, we correlate the "parts" of  $y$  and  $x_{3}$  that are not predicted by  $x_{1}$  and  $x_{2}$.

More formally, we can define the partial correlation just described as

$$
\frac{\text { Covariance }\left(y, x_{3} \mid x_{1}, x_{2}\right)}{\sqrt{\text { Variance }\left(y \mid x_{1}, x_{2}\right) \operatorname{Variance}\left(x_{3} \mid x_{1}, x_{2}\right)}}
$$

> Note!

> That this is also how the parameters of a regression model are interpreted. Think about the difference between interpreting the regression models:

$$
y=\beta_{0}+\beta_{1} x^{2} \text { and } y=\beta_{0}+\beta_{1} x+\beta_{2} x^{2}
$$

In the first model,  $\beta_{1}$ can be interpreted as the linear dependency between  $x^{2}$  and  $y$. In the second model,  $\beta_{2}$ would be interpreted as the linear dependency between $x^{2}$  and $y$ WITH the dependency between $x$  and $y$ already accounted for.

For a time series, the partial autocorrelation between  $x_{t}$  and  $x_{t-h}$  is defined as the conditional correlation between  $x_{t}$  and  $x_{t-h}$, conditional on  $x_{t-h+1}$,$ \ldots$, $x_{t-1}$, the set of observations that come between the time points  $t$  and  $t-h$.

- The  $1^{\text {st }}$ order partial autocorrelation will be defined to equal the  $1 \mathrm{st}$  order autocorrelation.
- The  $2^{\text {nd }}$  order (lag) partial autocorrelation is

$$
\frac{\operatorname{Covariance}\left(x_{t}, x_{t-2} \mid x_{t-1}\right)}{\sqrt{\operatorname{Variance}\left(x_{t} \mid x_{t-1}\right) \operatorname{Variance}\left(x_{t-2} \mid x_{t-1}\right)}}
$$

This is the correlation between values two time periods apart conditional on knowledge of the value in between. (By the way, the two variances in the denominator will equal each other in a stationary series.)

- The  $3^{\text {rd }}$  order (lag) partial autocorrelation is

$$
\frac{\text { Covariance }\left(x_{t}, x_{t-3} \mid x_{t-1}, x_{t-2}\right)}{\sqrt{\operatorname{Variance}\left(x_{t} \mid x_{t-1}, x_{t-2}\right) \operatorname{Variance}\left(x_{t-3} \mid x_{t-1}, x_{t-2}\right)}}
$$

And, so on, for any lag.

Typically, matrix manipulations having to do with the covariance matrix of a multivariate distribution are used to determine estimates of the partial autocorrelations.

### Some Useful Facts About PACF and ACF Patterns

**Identification of an AR model is often best done with the PACF.**

For an AR model, the theoretical PACF “shuts off” past the order of the model. The phrase “shuts off” means that in theory **the partial autocorrelations are equal to 0** beyond that point. Put another way, the number of non-zero partial autocorrelations gives the order of the AR model. By the “order of the model” we mean the most extreme lag of x that is used as a predictor.

Example: Following is the sample PACF(Lesson 1.2) for this series. Note that the first lag value is statistically significant, whereas partial autocorrelations for all other lags are not statistically significant. This suggests a possible AR(1) model for these data.

![Partial Autocorrelation Function for quakes](https://pic4.zhimg.com/80/v2-d6af772fbb551cc81eaad246cd168ee6.png)

**Identification of an MA model is often best done with the ACF rather than the PACF.**

For an MA model, the theoretical PACF does not shut off, but instead tapers toward 0 in some manner. A clearer pattern for an MA model is in the ACF. The ACF will have non-zero autocorrelations only at lags involved in the model.

Note that the first lag autocorrelation(MA(1) series) is statistically significant whereas all subsequent autocorrelations are not. This suggests a possible MA(1) model for the data.

![Autocorrelation Function for x](https://pic4.zhimg.com/80/v2-bea414ac92af0c82923afb8c084b0336.png)

For $x_t=10+\omega_t+0.7\omega_{t-1}$, the pattern gradually tapers to 0.

![Theoretical PACF of MA(1) with theta=0.7](https://pic4.zhimg.com/80/v2-66210fa4e268309437e2a0b80633d902.png)

The PACF just shown was created in R with these two commands:
```r
ma1pacf = ARMAacf(ma = c(.7),lag.max = 36, pacf=TRUE)
plot(ma1pacf,type="h", main = "Theoretical PACF of MA(1) with theta = 0.7") 
```

## Notational Conventions
Time series models (in the time domain) involve lagged terms and may involve differenced data to account for trend. There are useful notations used for each.

**Backshift Operator**

Using B before either a value of the series  x_{t}  or an error term  w_{t}  means to move that element back one time. For instance,

$$
B x_{t}=x_{t-1}
$$

A "power" of B means to repeatedly apply the backshift in order to move back a number of time periods that equals the "power." As an example,

$$
B^{2} x_{t}=x_{t-2}
$$

 $x_{t-2}$  represents  $x_{t}$  two units back in time.  $B^{k} x_{t}=x_{t-k}$  represents  $x_{t}$ $\mathrm{k}$  units back in time. The backshift operator B doesn't operate on coefficients because they are fixed quantities that do not move in time. For example,  $B \theta_{1}=\theta_{1}$.

 ### AR Models and the AR Polynomial
 AR models can be written compactly using an "AR polynomial" involving coefficients and backshift operators. Let  p=  the maximum order (lag) of the AR terms in the model. The general form for an AR polynomial is

$$
\Phi(B)=1-\phi_{1} B-\cdots-\phi_{p} B^{p}
$$

Using the AR polynomial one way to write an AR model is

$$
\Phi(B) x_{t}=\delta+w_{t}
$$

### Examples 2-3

Consider the AR(1) model  $x_{t}=\delta+\phi_{1} x_{t-1}+w_{t}$  where  $w_{t} \stackrel{i i d}{\sim} N\left(0, \sigma_{w}^{2}\right)$. For an AR(1), the maximum lag = 1 so the AR polynomial is

$$
\Phi(B)=1-\phi_{1} B
$$

and the model can be written

$$
\left(1-\phi_{1} B\right) x_{t}=\delta+w_{t}
$$

To check that this works, we can multiply out the left side to get

$$
x_{t}-\phi_{1} x_{t-1}=\delta+w_{t}
$$

Then, swing the  $-\phi_{1} x_{t-1}$  over to the right side and we get

$$
x_{t}=\delta+\phi_{1} x_{t-1}+w_{t}
$$

An AR(2) model is  $x_{t}=\delta+\phi_{1} x_{t-1}+\phi_{2} x_{t-2}+w_{t}$. That is,  $x_{t}$  is a linear function of the values of  x  at the previous two lags. The AR polynomial for an AR(2) model is

$$
\Phi(B)=1-\phi_{1} B-\phi_{2} B^{2}
$$

The AR(2) model could be written as  $\left(1-\phi_{1} B-\phi_{2} B^{2}\right) x_{t}=\delta+w_{t}$ , or as  $\Phi(B) x_{t}=\delta+w_{t}$  with an additional explanation that

$$
\Phi(B)=1-\phi_{1} B-\phi_{2} B^{2}
$$

An AR(p) model is  $x_{t}=\delta+\phi_{1} x_{t-1}+\phi_{2} x_{t-2}+\ldots+\phi_{p} x_{t-p}+w_{t}$ , where  $\phi_{1}, \phi_{2}, \ldots, \phi_{p}$  are constants and may be greater than 1 . (Recall that  $\left|\phi_{1}\right|<1$  for an AR(1) model.) Here  $x_{t}$  is a linear function of the values of  $x$  at the previous  $p$  lags.

A shorthand notation for the AR polynomial is  $\Phi(B)$  and a general AR model might be written as  $\Phi(B) x_{t}=\delta+w_{t}$. Of course, you would have to specify the order of the model somewhere on the side.

### MA Models
A MA(1) model  $x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}$  could be written as  $x_{t}=\mu+\left(1+\theta_{1} B\right) w_{t}$. A factor such as  $1+\theta_{1} B$  is called the MA polynomial, and it is denoted as  $\Theta(B)$.

A MA(2) model is defined as  $x_{t}=\mu+w_{t}+\theta_{1} w_{t-1}+\theta_{2} w_{t-2}$  and could be written as  $x_{t}=\mu+\left(1+\theta_{1} B+\theta_{2} B^{2}\right) w_{t}$. Here, the MA polynomial is  $\Theta(B)=\left(1+\theta_{1} B+\theta_{2} B^{2}\right)$

In general, the MA polynomial is  $\Theta(B)=\left(1+\theta_{1} B+\cdots+\theta_{q} B^{q}\right)$, where  $q$=  maximum order (lag) for MA terms in the model.

In general, we can write an MA model as  $x_{t}-\mu=\Theta(B) w_{t}$.

### Models with Both AR and MA Terms
A model that involves both AR and MA terms might be written  $\Phi(B)\left(x_{t}-\mu\right)=\Theta(B) w_{t}$  or possibly even

$$
\left(x_{t}-\mu\right)=\frac{\Theta(B)}{\Phi(B)} w_{t}
$$

> Note!

> Many textbooks and software programs define the MA polynomial with negative signs rather than positive signs as above. This doesn't change the properties of the model, or with a sample, the overall fit of the model. It only changes the algebraic signs of the MA coefficients. Always check to see how your software is defining the MA polynomial. For example is the MA(1) polynomial  $1+\theta_{1}B$  or  $1-\theta_{1} B$?

### Differencing
Often differencing is used to account for nonstationarity that occurs in the form of trend and/or seasonality.
The difference  $x_{t}-x_{t-1}$  can be expressed as $(1-B)_{X_{t}}$.
An alternative notation for a difference is

$$
\nabla=1-B
$$

Thus

$$
\nabla x_{t}=(1-B) x_{t}=x_{t}-x_{t-1}
$$

A subscript defines a difference of a lag equal to the subscript. For instance,

$$
\nabla_{12} x_{t}=x_{t}-x_{t-12}
$$

This type of difference is often used with monthly data that exhibits seasonality. The idea is that differences from the previous year may be, on average, about the same for each month of a year.

A superscript says to repeat the differencing the specified number of times. As an example,

$$
\nabla^{2} x_{t}=(1-B)^{2} x_{t}=\left(1-2 B+B^{2}\right) x_{t}=x_{t}-2 x_{t-1}+x_{t-2}
$$

In words, this is a first difference of the first differences.