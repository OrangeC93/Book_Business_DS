## Decition Tree
- objectives: sum-squared error, mulinomial deviance, gini imputiry .. 
- greedy search: construct splits sequentially and recursively

## CART
- overfit: build a path of candidate models and apply CV.
  - mincut is the minimum size for a new child. 
  - mindev is the minimum (proportion) deviance improvement for proceeding with a new split.
- eg. prostate cancer tumors


## Random Forest
we’ve found that this input randomization is useful in small samples (i.e., if p ≈ √—n or bigger) but that it can damage performance if you have large n, which would support its interpretation as extra regularization.

The disadvantage of moving from a tree to a forest is that you lose the single-tree interpretability. Howerver, with the argument *importance="impurity"*, ranger tracks each b’s predictive performance on the out-of-bag sample—the observations that were not included in the bth resample. These errors are paired with information about which variables are split upon in each tree. This yields a *variable.importance* statistic: the increase in error that occurs when that variable is not used to define tree splits.

Another disadvantage is that the full parallelization becomes infeasible and slow, one common stratey is to replace the sampleing of RFs with subsampling: each tree is fit to a without-replacement sample of size smaller than n(small enough the fit on a single core), which is called subsampling forest, but it tends to have sharply worse predictve performance than RFs. So it motivates people to formalize a simple alternative called *empirical Bayesian forest*

#### EBF
The algorithm is based on a simple observation: when you have a large dataset, the tops (or trunks—the first few splits) of the trees in a forest are mostly similar. Since the trunks in the forest are all the same, just fit the trunk once and then use it to partition the data and facilitate parallelization. This is the empirical Bayesian forest (EBF) algorithm.

![image](/pic/EBF.png)
The result is a sort of hybrid CART-RF object, with fixed initial splits and a bootstrap sample of trees for deeper structure.

## Causal Tree
![image](/pic/causal_tree.png)
One for determining the tree splits and a second that is used to re-estimate the treatment effects within each leaf conditional upon this tree structure.
![image](/pic/OHIE_causal_tree.png)

## Gaussian Processes
- R: laGP
