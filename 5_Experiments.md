## Randomized Control Trials
Background: In 2008, the state of Oregon gained funding to expand its coverage for Medicaid—the U.S. social program that provides health insurance to people who cannot afford private insurance. Since the demand was expected to outstrip supply, they designed a lottery that randomly allocated eligibility for Medicaid enrollment among the states's low income population.

OHIE: a randomized controlled AB trial for measuring the treatment effect of Medicaid eligibility.
- People in treatment group were tracked 12 month
- We need to observe how Medicaid changes the use of health services. 
  - This is important both for estimating the cost of expanded public insurance and for modeling the downstream public health improvements. 
  - For example, researchers found a small increase in hospitalization for those selected in the Medicaid lottery (hereafter the treated or selected group) but no increase in usage for emergency room (ER) services (indeed, the point estimate shows slightly decreased ER usage), which helps put an upper bound on cost projections.

Data: nber.org/oregon/4.data.html,
- The *doc_any_12m* variable is based upon a 12-month follow-up survey results from 23,000 of the total 75,000 people in the full sample (survey targeting and nonresponse was balanced across treatment groups so that you can treat these results as representative of the population, **However, nonresponse and other tracking problems (e.g., missing users because of cookie clearing in web experiments) are common sources of bias in survey results**).
- *doc_any_12m* is coded as yi=1 for patients who saw a primary care physician(PCP) in 12 months of the study. 
  - Primary care usage is generally viewed as a good thing: PCP visits are a relatively low-cost form of healthcare and they can prevent the need for expensive acute care from hospitalization or via the ER.
- The treatment of interest is the *selected* variable, which is 1 for the approximately 50% of people who were randomly selected in the lottery to be able to apply for Medicaid.
  - Each of these 23,107 individuals was enrolled into the lottery for the chance to apply for Medicaid. 
- *Medicaid* variable is 1 if these selected people then actually enrolled for Medicaid. 
  - Not all those who were selected ended up actually enrolled; they might not have bothered applying, or they could have been found ineligible for a variety of reasons (e.g., if their income was higher than the state expected).
- *household_id*—some people in the study shared a household— and *numhh*, the number of people from each household that applied for Medicaid.

Scenario1: 
- The sandard ATE estimate: 0.057, increase in PCP visit rates due to lottery selection

Scenario2: Map from sample of individual in the experiment to the future treatment popluation
- For example, perhaps young people are harder to reach for a phone survey and they are underrepresented in the sample (in both treatment and control groups—this is a different issue than covariate imbalance).
- However, for many business settings, this is less of an issue because you are able to sample representatively from your customers.
- Alternatively, we can use regresion adjustment to offer a reduced-variance ATE, whichs is sometimes referred as to **post stratification**.
  - With observable covariates xi for each individual, a regression adjustment first fits a linear model for each treatment group, given the pooled covariate average, we could get the adjusted ATE estimator.
  - However, we tend to advise against these adjustments unless you have small sample sizes and know of a few factors that have a large influence on the response. For a large AB experiment with a perfectly randomized design, you can stick with the original ATE calculation.

**Perfect Experiment are Rare**
- Imperfect randomization: Each person selected in the lottery could apply for Medicaid for all of their family members so that these people are then also selected, which leads to an over-representation of large families in the selected group.
  - Simple model:
![image](/pic/simple_model.png)
  - Full model:
![image](/pic/full_model.png)
  - Full interaction model is more robust to heterogeneous (covariate-dependent) treatment effects.
- Dependence between units: The sample includes members of the same household whose behavior—for example, the decision to visit a PCP—will be correlated.
  - Solution1: calculated on household level rather than user level. The group-level and individual-level models are similar(if we calculate the confidence level) here because there are a large number of groups—most households in the sample contain only a single person.
![image](/pic/household_level.png)
  - Solution2&3: We could obtain nonparametric standard error through use of bootstrap or HC
    - When you have dependence between observations, you need to use a blocked bootstrap that resamples on the group rather than individual level. In this case, we resample households with-replacement while fitting the person-level model for each bootstrap sample.

**Linear vs Logistic**
(1)Linear regression (i.e., OLS) is often used for ATEs because the treatment effect is thought of as additive and because steps like regression adjustment are easiest in linear models. 

(2)Moreover, OLS will give a robust estimate for the ATE even if the truth is nonlinear and the errors are non-Gaussian. 

However, it often makes more sense to talk about multiplicative effects and, especially if you have covariates, a logistic regression might do a better job in real data examples at recovering the true conditional mean function.

## Near Experimental Designs
#### A DIFF-IN-DIFF Analysis
Example: Study the effect of search engine market from eBay
- What's the effect of paid search advertising
- What would happen to scale revenue if eBay stopped paying SEM
- Do they get any benefit from also appearing in sponsored slots, How big the benefits, it's worth the cost?

eBay stopped bidding on any AdWords (the marketplace through which Google SEM is sold) for 65 of the 210 “designated market areas” (DMA) in the United States for the eight weeks following May 22, 2012. (These DMAs are viewed as roughly independent markets around metropolitan centers ranging from Boston to Los Angeles.)
![image](/pic/did_revenue.png)
- The top line corresponds to those DMAs that are never treated (SEM is always on)
- The bottom line is for those where SEM was turned off on May 22 (marked with the dashed vertical). 
- This was not a fully randomized trial.Thus, if SEM works, the revenue difference between treatment and control groups should be larger after May 22.

Here we focus on the differences in log revenue because we anticipate that the treatment and control groups are related on percentage scale. There does appear to be an increase in the log difference following May 22. Is it real (i.e., statistically significant), and what are the implications for the return on investment for SEM?
![image](/pic/log_revenue_did.png)

Assuming that the nothing other than the SEM-turnoff changes between the treatment groups after May 22. 
![image](/pic/did_regression_model.png)
- t denotes before and after May 22
- d denotes control and treatment group
![image](/pic/did_regression_r.png)
- This says that the treatment—turning off paid search ads—has **a small but not statistically** significant negative effect on log revenue (0.66 percentage point drop with a standard error of 0.55 points). 
- Even if the result was statistically significant, it is **doubtful** that paid search would have a positive ROI once the cost of the marketing is accounted for.

Controlling for DMA specific revenue levels in the regression mena equation: The estimates for γ and its standard errors are practically unchanged from our earlier regression analysis using clustered standard errors.
![image](/pic/did_regression_model_dma.png)

Aonther way: pairing between pre and post treatment observations, the results are unchanged from our previous regression analyses.
- Caculate the sample of pre-post differences for each DMA ri = yi1-yi0
- Collect the average difference for the treated and control group

This routine is the source of the “difference in differences” name. Independence between DMAs implies independence for each ri.


#### Regression Discountinuity Estimators
Definition: treatment allocation is determined by a threshold on some “forcing variable,” and subjects that are close to the threshold, on either side, are comparable for causal estimation purposes.
- In a strict RD, the treatment is fully determined by the forcing variable so you just need to control for that variable. However, in a imperfect experimental design, you need to control for (i.e., include in the regression) any variable that is correlated with both treatment allocation and the response.
- In a “fuzzy” RD designs, the threshold changes the probabilities for different treatments, but allocation is not deterministic, these are versions of the IV setup.

Assumption: continuity assumption
- If the threshold were to move slightly, subjects switching treatment groups will behave similarly to those near their new treatment group.

Formular:
- di is treatment group, yi(di) is the observed response
- forcing variable ri, the location of ri relative to their treatment threshold determins the treatment status.
![image](/pic/5_17.png)
- For example, we will usually estimate separate linear regressions within some distance δ of the threshold:
![image](/pic/5_20.png)
- Conditional ATE:
![image](/pic/5_21.png)

Example
- Forcing variable(rank score - reserve)
- Treatment status: whether or not the ad is shown in the mainline instead of the sidebar
- Response variable y is the version of ad revenue

Method1: compare the mean revenue y on the either side of the r=0 threshhold
- Pitfall: this difference in means analysis is implicitly assuming a model where the response is constant within the window on the either side of the threshold. It's unlikely, since the rank score increases with both advertiser bid and the expected click rate, which means higher ri leads to higher yi

Method2: localized linear regression 
![image](/pic/localized_regression.png)

Local analysis window: calculate the RD treatment effect estimate for a range of windows.
- After an initial high-variance region, where the window size is too small for you to estimate a reliable linear regression,
- Moving to a wider window leads to lower uncertainty around the estimate, but this is at the expense of more restrictive linearity assumptions
- In practice, this window-length selection is more art than science, and you should make sure that results are stable within a range of plausible values.

#### Instrumental Variables
IVs are the indirect randomizers, or upstream sources of randomization which indirectly randomized though experiments on variables that influence policy selection.

![image](/pic/diagram_iv_model.png)
- e, unobserved factors or errors, say e, that have influence on both treatment and response
- instrument z: it acts on y only through p and it's completely independent from the unobserved error e
- 
