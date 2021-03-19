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

