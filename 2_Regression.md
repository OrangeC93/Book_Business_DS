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
