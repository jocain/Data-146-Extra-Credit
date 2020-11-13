# Classification Analysis of Liberian Demographic Data

### Description

In this project, I explore a veriety of classification methods as a means of modeling education levels in a set of data concerning Liberian demographic information. 

### Data Overview

The data used in this project was collected in the Liberia Demographic and Health Survey of 2013. The survey was conducted by the Liberia Institute of Statistics and Geo-Information (LISGI) from mid-March to mid-July in 2013, and was funded by a number of international governmnet and non-goverment organizations, including USAID and UNICEF. The survey covered a broad stroke of information ranging from HIV testing to access to drinking water. For the purposes of this project, Location, Size, Wealth, Gender, and Age were used as features in an attempt to model a target variable, Education. 

Location, Wealth, Education, and Gender are nominal variables, whereas Age and Size are treated as continuous variables. Location refers to six groups of counties paritioned by LISGI, shown in the map below:

![Map](https://jocain.github.io/Data-146-Extra-Credit/map.png)



Pairplot
![Pairplot of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/parwise.png)

Histogram of Target Data
![Histogram of Target Data](https://jocain.github.io/Data-146-Extra-Credit/logeducation.png)



### Logistic Regression

I applied multiple different logistic regression models to the data, varying the scaling or normalization method and distance weights. The either no scaling/normalization or one of the following sklearn.preprocessing classes were applied to the data prior to modeling: Normalizer, Standard Scaler, Robust Scaler, MinMax Scaler. The model itself either did or did not employ weighting proportional to the class proportions within the test data. The combinations of these settings resulted in a total of ten models, each of which underwent a K-Fold Validation for  k = 15, and were set to a maximum iteration limit of 500 iterations. The table below shows the results of this:

|    Weighting?    |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | Convergence? |
| No Class Weights |      None      | 0.572271450839 | 0.572222563093 | 1.066941627605 | 1.067155585190 |      No      |
|                  | Standard Scal. | 0.572349962346 | 0.572263996647 | 1.066714980295 | 1.067632742056 |      Yes     |
|                  |  MinMax Scal.  | 0.572370701671 | 0.572201781797 | 1.066417229149 | 1.068026670511 |      Yes     |
|                  |  Robust Scal.  | 0.572370701671 | 0.572201781797 | 1.066417229149 | 1.068026670511 |      Yes     |
|                  |   Normalizer   | 0.572336630198 | 0.572263983744 | 1.066814230370 | 1.067695002069 |      Yes     |
|  Class Weights   |      None      | 0.272520039045 | 0.271822034431 | 23.88594807159 | 23.95425182013 |      No      |
|                  | Standard Scal. | 0.273051822132 | 0.271926134462 | 23.77752901468 | 23.90392399375 |      Yes     |
|                  |  MinMax Scal.  | 0.267833070115 | 0.266513403806 | 24.29619114166 | 24.42519052370 |      Yes     |
|                  |  Robust Scal.  | 0.419169358696 | 0.418839970153 | 15.58084085588 | 15.62237537755 |      Yes     |
|                  |   Normalizer   | 0.272829621471 | 0.271739470557 | 23.80813048002 | 23.93098978258 |      Yes     |

As it turned out, even with scaling or normalization, not all the models. However, whether the model converged or not, the scaling or normalization method seemed to have very little effect on the accuracy or error of the model. The one exception to this might be 

### k-Nearest Neighbor

k-Nearest Neighbor (kNN) modeling results in this. 

![k-NN k-Variation Example](https://jocain.github.io/Data-146-Extra-Credit/knnexample.png)

|    Weighting?    |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    |
| No Dist. Weights |      None      | 0.716522533716 | 0.704535825798 | 0.682558462471 | 0.706238388749 |
|                  | Standard Scal. | 0.739052179696 | 0.705385878203 | 0.572102599014 | 0.626166534888 |
|                  |  MinMax Scal.  | 0.727945121679 | 0.713038640241 | 0.583785893911 | 0.609409249257 |
|                  |  Robust Scal.  | 0.733849724996 | 0.703705941766 | 0.578873790882 | 0.626311849112 |
|                  |   Normalizer   | 0.694363219362 | 0.671249984273 | 0.665290515902 | 0.704973142708 |
|  Dist. Weights   |      None      | 0.915823046197 | 0.681909756531 | 0.205197721293 | 0.748047225993 |
|                  | Standard Scal. | 0.915823046197 | 0.688255774454 | 0.205197721293 | 0.683508548493 |
|                  |  MinMax Scal.  | 0.915823046197 | 0.691926502216 | 0.205197721293 | 0.676477631719 |
|                  |  Robust Scal.  | 0.915823046197 | 0.687488402056 | 0.205197721293 | 0.687324590479 |
|                  |   Normalizer   | 0.910830941655 | 0.665277242545 | 0.214456074666 | 0.745102682245 |

### Decision Tree/Random Forrest

Decision Tree and Random Forrest modeling results in this. 

how are you?
