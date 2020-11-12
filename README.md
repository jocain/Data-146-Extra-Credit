# Classification Analysis of Liberian Demographic Data

### Description

In this project, I explore a veriety of classification methods as a means of modeling education levels in a set of data concerning Liberian demographic information. 

### Data Overview

Pairplot
![Pairplot of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/parwise.png)

Histogram of Target Data
![Histogram of Target Data](https://jocain.github.io/Data-146-Extra-Credit/logeducation.png)



### Logistic Regression

I applied multiple different logistic regression models to the data, varying the scaling or normalization method and distance weights. The either no scaling/normalization or one of the following sklearn.preprocessing classes were applied to the data prior to modeling: Normalizer, Standard Scaler, Robust Scaler, MinMax Scaler. The model itself either did or did not employ weighting proportional to the class proportions within the test data. The combinations of these settings resulted in a total of ten models, each of which underwent a K-Fold Validation for  k = 15, and were set to a maximum iteration limit of 500 iterations. The table below shows the results of this:

|    Weighting?    |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | Convergence? |
| No Class Weights | No Scal./Norm. | 0.572271450839 | 0.572222563093 | 1.066941627605 | 1.067155585190 |      No      |
|                  | Standard Scal. | 0.572349962346 | 0.572263996647 | 1.066714980295 | 1.067632742056 |      Yes     |
|                  |  MinMax Scal.  | 0.572370701671 | 0.572201781797 | 1.066417229149 | 1.068026670511 |      Yes     |
|                  |  Robust Scal.  | 0.572370701671 | 0.572201781797 | 1.066417229149 | 1.068026670511 |      Yes     |
|                  |   Normalizer   | 0.572336630198 | 0.572263983744 | 1.066814230370 | 1.067695002069 |      Yes     |
|  Class Weights   | No Scal./Norm. | 0.272520039045 | 0.271822034431 | 23.88594807159 | 23.95425182013 |      No      |
|                  | Standard Scal. | 0.273051822132 | 0.271926134462 | 23.77752901468 | 23.90392399375 |      Yes     |
|                  |  MinMax Scal.  | 0.267833070115 | 0.266513403806 | 24.29619114166 | 24.42519052370 |      Yes     |
|                  |  Robust Scal.  | 0.419169358696 | 0.418839970153 | 15.58084085588 | 15.62237537755 |      Yes     |
|                  |   Normalizer   | 0.272829621471 | 0.271739470557 | 23.80813048002 | 23.93098978258 |      Yes     |

As it turned out, even with scaling or normalization, not all the models. However, whether the model converged or not, the scaling or normalization method seemed to have very little effect on the accuracy or error of the model. The one exception to this might be 

### k-Nearest Neighbor

k-Nearest Neighbor (kNN) modeling results in this. 

### Decision Tree/Random Forrest

Decision Tree and Random Forrest modeling results in this. 

how are you?
