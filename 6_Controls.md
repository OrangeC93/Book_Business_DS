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
- The model can be extended in a number of ways, for example by having γ change with x in a heterogeneous treatment effect specification or replacing x′β and x′τ with flexible functions l(x) and m(x) in a “partially linear” treatment effects model.
- The first line: a reduced-form model where βj represents average change in y with xj but without any worry about causation.
- The second line stats that x contains all of the variables that influence both d and y, such that the conditional ignorability assumption holds and γ can be interpreted causally.

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



