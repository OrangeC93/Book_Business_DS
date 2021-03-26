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

The disadvantage of moving from a tree to a forest is that you lose the single-tree interpretability
With the argument *importance="impurity"*, ranger tracks each b’s predictive performance on the out-of-bag sample—the observations that were not included in the bth resample. These errors are paired with information about which variables are split upon in each tree. This yields a *variable.importance* statistic: the increase in error that occurs when that variable is not used to define tree splits.

#### EBF

