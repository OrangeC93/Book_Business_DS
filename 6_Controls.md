## Conditional Ignorability and Linear Treatment Effects

Conditional ignorability (CI) implies that you observe all of the variables that influence both the treatment and the response. It can be expressed as (6.1)
- Which means: all of the potential outcomes yi(d) are independent from di given the controls xi. That is, after conditioning on all of the information in x, the potential outcomes across all treatment levels are unrelated to the treatment status you have actually been assigned.
- We can control for the factors in x simply by including them in the regression models, but things get difficult when x has high dimension or if we don't know the form of its influence on d and y.

OLS doing(don't understand): eg. sales, brand, price 
- OLS regression coefficients represent the partial effect for each input after its correlation with the other inputs has been removed. 
- What OLS is doing: it is finding the coefficients on the part of each input that is independent from the other inputs.

**LTE (linear treatment effect) model**:
![image](/pic/LTE_model.png)
- d treatment status varaible, y response of interest, x is all the variables that can influence both d and y, potential confounders
  - The first line: a reduced-form model where βj represents average change in y with xj but without any worry about causation.
  - The second line stats that x contains all of the variables that influence both d and y, such that the conditional ignorability assumption holds and γ can be interpreted causally.
- The model can be extended in a number of ways, for example by having γ change with x in a heterogeneous treatment effect specification or replacing x′β and x′τ with flexible functions l(x) and m(x) in a “partially linear” treatment effects model.


Problems start arising only when you have so many variables to control for that the assumption of n >> p no longer applies.

## High-Dimensional Confounder Adjustment
Example: easier access to abortion causes decreased crime.

- yst is the logged murder rate in state s in year t.
- dst is an effective abortion rate
- xst: prison, police, ur, inc, pov, AFDC, gun, beer
- In addition: add a fixed state effects as and a linear trend t&t

**Fit a regression model**
![image](/pic/ORIG1.png)
- One more abortion per ten live births (the units of d) leads to a 19%(1-exp(-2.08)) reduction in the per capita murder rate.
- Think about the alternative story: cell phone and murder rate (any variable with the shape of change in Figure 6.1 shows up as having a significant effect on murder)
  - To control for such variables, you need control for more flexible time trends; (1)you can do this by including different effects for each year, αt.(2) It is also good practice to allow confounder effects to interact with each other
  - The sigfinicance disappear and direction is totally changes, **but really** what is going on is that we’ve added so many variables to the model that n ≈ p, just don’t have enough data to get a decent estimate.

This example illustrates that whenever your analysis is premised on conditional ignorability, you are always susceptible to others introducing additional controls until the model is nearly saturated and you can’t measure anything. This is the weakness of using OLS—a low-dimensional method—as your regression tool.

**LTE framework**
![image](/pic/LTE_lasso_regression.png)

**Back to the example**: AIC lasso selects d as highly predictable given x, then use the predicted d to figure out the effect on y.
![image](/pic/ps_1.png)
![image](/pic/ps_2.png)
Thus, the LTE lasso procedure finds no evidence for effect of abortion on murder.

**PROPENSITY MODELS ARE AN ADAPTATION OF THE LTE FRAMEWORK FOR BINARY TREATMENTS**.
- In this setting, the second line of the LTE system in Equation 6.2 is replaced with a binomial distribution on the treatment equation
- Matching is a pain to implement because the matching algorithms are computationally expensive for large n, and it is unstable because results are highly sensitive to how you choose the matching criterion
- However we can use such explanations to help build intuition around your results even if you are actually making use of the regression techniques we advocate in this section

## Sample-Spliting
The LTE lasso and realted approaches can give the point estimates of the treatment effects, which don't come with the nice inferential properties. However, sample splitting is a good algorithm for inferenfence.

To do inference via sample splitting
- Firstly: fit a regression model to half of the data:
![image](/pic/sample_splitting1.png)
- Then figure out which columns of x have a nonzero coefficient, and fit an unpenalized (i.e., MLE) logistic regression on only those variables using the second half of the data.
![image](/pic/sample_splitting2.png)
![image](/pic/sample_splitting3.png)

Sample splitting is a great method for dealing with high-dimensional controls because the effects of these controls are nuisance functions. They are not the primary object of interest; you just want to remove them from estimates of the treatment effect in a conditional ignorability model.


## Orthogonal Machine Learning
The naive ML conflates two problems:
- selecting controls and predicting the response conditional upon controls

Instead, Orthogonal ML
- Estimate nuisance functions that are orthogonal to y in the conditional scrore
- Then estimation for y is robust to slwo learning on these nuisance function

The Orthogonal ML8 framework of Chernozhukov et al. [2017a] provides a general recipe for use of sample splitting with high-dimensional controls.

The nuisance functions are the expectations for treatment and response given controls,  [d|x] and  [y|x], and after estimating each of these on an auxiliary sample, you can use out-of-sample residuals as the basis for the treatment effect estimation. They also provide a clever cross-fitting algorithm that allows you to use all of your data in the final treatment effect estimation
![image](/pic/orthogonal_ML_for_LTE.png)

Example: Pricing
- 1.Predict prices from the demand variables: p~x
- 2.Predict sales from the demand variables: y~x
- 3.final regression: (y-y preidict) ~ (p - p predict)
- Estimated relationship is causal if x contains all demand info known to pricer
- For inference you can data split: use one sample for 1, 2, another rfor step 3

## HTE(Heterogeneous Treatment Effects)
The influence of the treatment (i.e., a policy variable that you control) varies as a function of who or what is being treated, is referred to as heterogeneous treatment effects (HTEs).

For randomized treatments—for example, in an AB trial—modeling HTEs is as easy as running a regression that **interacts the treatment variable with sources of heterogeneity**. For example, if d has been randomized across subjects, you can fit the basic interaction model
![image](/pic/HTE.png)

OHIE example: page 184
- The baseline treatment effect is now a 9% increase in PCP visit rates. 
- But there are large sources of heterogeneity around this value. 
  - For example, people of Pacific Islander descent have 13% increase as their treatment effect, while for Asians the treatment effect is only a 5% increase.

#### Price and Demand
As an example, we will consider data on sales of beer between 1989 and 1994 at Dominick’s stores in the Chicagoland area.16 For each beer unique product code (UPC), we have weekly total unit sales (MOVE) and average prices across 63 different stores.


