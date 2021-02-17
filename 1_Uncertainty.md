## Bootstrap

Why we need?
- The theoretical sampling distribution reply for much of classical statistics, not valid for complex setting.
- When the number of model parameters is large relative to the number of observations, the CLT will give a poor representation of world performance.

How? Instead of relying on theoretical approximation, the bootstrap uses resampling from your current sample to mimic the sampling distribution.
![image](/pic/bootstrap_algorithm1.png)
- The bootstrap method has an equal probability of randomly drawing each original data point for inclusion in the resampled datasets.
- The procedure can select a data point more than once for a resampled dataset. This property is the “with replacement” aspect of the process.
- The procedure creates resampled datasets that are the same size as the original dataset.

The process ends with your simulated datasets having many different combinations of the values that exist in the original dataset. Each simulated dataset has its own set of sample statistics, such as the mean, median, and standard deviation. Bootstrapping procedures use the distribution of the sample statistics across the simulated samples as the sampling distribution.

Disadvantage: when it breaks
- It happen if the empirical data distribution is a poor approximation to the population 
- If the statistic you are targeting requires a lot of information about the entire population distribution

```
For example, we tend to not trust bootstraps for high-dimensional parameters because the joint sampling distribution is a function of all the population cross-variable dependencies. 

Or, when the data population has heavy tails (i.e., it includes some rare large values), you will have trouble bootstrapping the mean because the extreme values have an outsized influence on your distributional estimate.
```
![image](/pic/bootstrap_algorithm2.png)

## Hypothesis Testing and False Discovery Rate Control
PDR = # false positives/ # tests called significant
![image](/pic/bootstrap_algorithm3.png)

[Why](https://www.statisticshowto.com/benjamini-hochberg-procedure/)
The Benjamini-Hochberg Procedure is a powerful tool that decreases the false discovery rate. Adjusting the rate helps to control for the fact that sometimes small p-values (less than 5%) happen by chance, which could lead you to incorrectly reject the true null hypotheses. In other words, the B-H Procedure helps you to avoid Type I errors (false positives).


![image](/pic/BH_eg.png)
The recipe in Algorithm 3 is now simple: find the largest p-value that is below this line and call it and all smaller p-values “significant.” These five points are shown in Figure 1.9; you expect that around q = 0.1 of them (0 to 1 of the values) are false positives.

## Bayesian Inference
Bayesian uncertainty, which characterizes probabilities over models and parameters by appealing to the idea of subjective beliefs rather than repeated trials.



