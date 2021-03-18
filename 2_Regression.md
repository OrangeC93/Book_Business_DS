## Regression
Linear regression: ...

Log(variable), each unit increase in x leads y to be multiplied by the factor <img src="https://render.githubusercontent.com/render/math?math=e^{\beta}">.
- When variables change is usually expressed in percentage rather than absolute terms
- When variables are strictly non negative
- When each other two variables that both move multiplicatively


**Example1: a simple model**
![image](/pic/example1.png)
- Log price effect: sales drop by about 3% for every 1% price hike
- At the same price, Tropicana sells more than Minute Maid, which in turn sells more than Dominick’s. This makes sense: Tropicana is a luxury product that is preferable at the same price
![image](/pic/price_sale1.png)
- All of the lines in have the same slope. In economic terms, the model assumes that consumers of the three brands have the same price sensitivity, which is unrealistic

**Example2: add log price interact with brand**
- We want to know whether one variable modulates the effect of another.
![image](/pic/example2.png)
We can find  the elasticities for each brands by adding the log(price):brand interaction terms to this main slope.
- We see that Tropicana customers are less sensitive than the others: −2.7 versus around −3.3. 
![image](/pic/result2.png)

**Example3: confounding between advertisement and brand effects**
![image](/pic/example3.png)
- Look at the role of advertising in the relationship between sales and prices
![image](/pic/result3.png)
We see that being featured always leads to more price sensitivity. Minute Maid and Tropicana elasticities drop from −2 to below −3.5 with ads, while Dominick’s drops from −2.8 to −3.2. Why does this happen?
- One possible explanation is that advertisement increases the population of consumers who are considering your brand. In particular, it can increase your market beyond brand loyalists, to include people who will be more price sensitive than those who reflexively buy your orange juice every week. Indeed, if you observe increased price sensitivity, it can be an indicator that your marketing efforts are expanding your consumer base.

Why example2 and example3 resutls are different
- The answer is that the overly simple model in example2 led to a **confounding between advertisement and brand effects**.

- The model in Equation3 corrects this by including feat in the regression. This phenomenon, where variable effects can get confounded if you don’t control for them correctly, will play an important role in the later discussions of causal and structural inference.

## Logistic regression
## Deviance and likelihood
![image](/pic/summary.png)
- Residual deviance D: fitted and actual
- Null deviance D0: fitted and average actual
- R squre: 1- D/D0, measures how much response variability you are able to model as a function of the regression inputs
- Dispersion parameter, measures variability around the fitted conditional means, R has estimated the dispersion parameter by taking the variance of your fitted residuals. In logistic regression, there is no corresponding “error term,” so R just outputs Dispersion parameter for binomial family taken to be 1. If you don’t see this, then you might have forgotten to put “type=binomial.”
- Residual degrees of freedom defined as the number of observations minus the number of parameters.


## Regression uncertainty
**Usual way**: standard error by the software, which is okay for many purpose as long as the model is close to representing the true data generating process, but it will be wrong if there're [heteroskedastic errors](https://www.zhihu.com/question/278182454)

**Nonparametric method**(methods that account for the possibility that the stated model is not quite a perfect fit)  
- Bootstrap, a strategy in which you resample the data (with replacement) and use the uncertainty across samples as an estimate of actual sampling variance. You can use a bootstrap to nonparametrically quantify the uncertainty associated with regression coefficients. 
- For example, compared parametric and nonparametric uncertainty estimates for the coefficient on an indicator for broadband access when predicting online spending.

**OLS - sandwich variance estimators**: error variance, ∑, “sandwiched” by the projection X(X′X)–1.
![image](/pic/sandwich.png)

**A more general model**:
- You can use Equation 2.29 to quantify uncertainty in the common heteroskedastic scenario where each observation has a different error variance. That is, where ∑ = diag(σ2) = diag ([σ21, σ22, . . ., σ2n]) with a different diagonal entry for each observation. The **heteroskedastic consistent12 (HC) standard errors** are constructed by replacing ∑ in Equation 2.29 with an estimate that allows for such heteroskedasticity.
- AER package can be used to obtain HC standard errors, we need to take the square root of the diagonal variance estimate, for example wind sqrt (0.8) = 91288 which is much larger than the standard error of 0.64 from R output, the reason for this heteroskedasticity is that much larger residuals occurring on low wind days. 
![image](/pic/aer_hc.png)

**HC and Bootstrap**
Here is also an interesting relationship between HC standard errors and the bootstrap. It turns out that, for OLS, the parameter variance estimates you get from the nonparametric bootstrap are actually approximated by the HC procedure. 
![image](/pic/hc_bootstrap_vanilla.png)


These techniques are commonly used when estimating treatment effects in randomized controlled trials.

## Space and Time
Exmaple: airline passenger regression

**log(yt) = α + βtt + βmt + εt**
- Including time as a continuous variable t (i.e., date 1, 2, 3, . . . ) and with month mt denoting the calendar month at time t.
![image](/pic/ts.png)

- De-trending is easy. If your data include dates, then you should create indicator variables for, say, each year, month, and day. For example: if you are going to include an effect for may-1981, then you should also include broader effects for may and for 1981.
- Another issue: autocorrelation


**autocorrelation**
Phenomenon: If we consider the residuals for the passenger regression, like when residuals are high in one month, they are often high the next month. The errors appear correlated in time, such that εt is related to εt–1. That relationship violates our basic independence assumption,

Define autocorrelation function ACF that tracks lag-l correlations:
- acf(l) = cor(εt, εt-1)
- 

![image](/pic/acf_airline.png)
- There's significant dependence, the correlation between yt and yt-1 is around 0.8, which is pretty huge. 
- Note that acf (0) = 1 because this is the correlation between yt and itself. The dashed lines are heuristics for “big” correlations.

**random walk**
- This is called a random walk model for yt : the expectation of what will happen is always what happened most recently.
![image](/pic/random_work.png)
- Random walks are one version of a general autoregressive (AR) model.

**AR(1)**
![image](/pic/AR1.png)
- This is simply yt regressed onto lagged yt–1. 
- The random walk corresponds to β1 = 1, and nonzero β0 is then referred to as drift. 

To fit the AR(1) model, you need to create lagged versions of your response and then include them in the regressions. This usually requires removing the first observation from your training data.
![image](/pic/ariline_AR1.png)

**coefficient**
The AR(1) model is simple but hugely powerful. If you have any suspicion of autocorrelation, it is a good move to include lagged response as a covariate. The coefficient on this lag gives you important information about the time-series properties:
- if |β1| = 1, random walk
![image](/pic/random_walk1.png)
  - In a random walk, the series just wanders around, and the **autocorrelation stays high** for a long time (see Figures 2.18 and 2.19). More precisely, the series is nonstationary: it has no average level that it wants to be near but rather diverges off into space.
  - This property is implied by the series being a random walk: the differences between yt and yt–1 are independent. If you have a random walk, you should perform this returns transformation to obtain something that is easier to model.
  - However, when we switch from prices to returns, (yt – yt–1)/yt–1, we get data that looks more like white noise.


- if |β1| > 1, series explodes
- if |β1| < 1, values are mean reverting
  - These series are called stationary because yt is always pulled back toward the mean. These are the most common, and most useful, type of AR series. The past matters in a stationary series, but with limited horizon and the autocorrelation drops off rapidly
![image](/pic/values_revert.png)
