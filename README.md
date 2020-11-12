# Classification Analysis of Liberian Demographic Data

### Description

In this project, I explore a veriety of classification methods as a means of modeling education levels in a set of data concerning Liberian demographic information. 

### Data Overview

The data looks like this.

### Logistic Regression

I applied multiple different logistic regression models to the data, varying the scaling or normalization method and distance weights. The either no scaling/normalization or one of the following sklearn.preprocessing classes were applied to the data prior to modeling: Normalizer, Standard Scaler, Robust Scaler, MinMax Scaler. The model itself either did or did not employ weighting proportional to the class proportions within the test data. The combinations of these settings resulted in a total of ten models, each of which underwent a K-Fold Validation for  k = 15, and were set to a maximum iteration limit of 500 iterations. The table below shows the results of this:

|    Weighting?    |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | Convergence? |
| No Dist. Weights | No Scal./Norm. |
|                  | Standard Scal. |
|                  |  MinMax Scal.  |
|                  |  Robust Scal.  |
|                  |   Normalizer   |
| Distance Weights | No Scal./Norm. |
|                  | Standard Scal. |
|                  |  MinMax Scal.  |
|                  |  Robust Scal.  |
|                  |   Normalizer   |

### k-Nearest Neighbor

k-Nearest Neighbor (kNN) modeling results in this. 

### Decision Tree/Random Forrest

Decision Tree and Random Forrest modeling results in this. 

how are you?
