# Classification Analysis of Liberian Demographic Data

### Description

In this project, I explore a variety of classification methods as a means of modeling education levels in a set of data concerning Liberian demographic information. 

### Data Overview

The data used in this project was collected in the Liberia Demographic and Health Survey of 2013. The survey was conducted by the Liberia Institute of Statistics and Geo-Information (LISGI) from mid-March to mid-July in 2013 and was funded by a number of international government and non-government organizations, including USAID and UNICEF. The survey covered a broad stroke of information ranging from HIV testing to access to drinking water. For the purposes of this project, Location, Size, Wealth, Gender, and Age were used as features in an attempt to model a target variable, Education. 

Location and Gender are nominal variables, Age and Size are treated as continuous variables, and Wealth and Education ordinal variable. Location refers to six groups of counties partitioned by LISGI, designated as the North Western, North Central, South Central, South Eastern A, and South-Eastern B regions, shown in the map below:

![Map](https://jocain.github.io/Data-146-Extra-Credit/map.png)

The survey was conducted such that the random houses in each of the regions were chosen, then all the members of the household answered the survey. Therefore, while Gender, Age, Education refer to an individual's qualities, Location, Size, and Wealth refer to attributes of the household they belong to. Household wealth in this study is broken up into quintiles, and Size refers to the number of members of a household. Of those members, Gender is taken to be binary (male or female), Age is the member's age, and Education is ranked by the highest level of education the member has received: None (0), Preschool/Kindergarten (1), Primary (2), Secondary (3), and Tertiary (4). This particular variable required some cleaning and amending. The original mapping of the survey responses was the following: None was 0, Preschool/Kindergarten was 6, Primary was 1, Secondary was 2, Tertiary was 3, and Unknown Level was 8. In fact, there was never any recording of an educational value of 6 anywhere in the dataset. However, some responses recorded an education value of 9, which is not valid, and was assumed to be 6. To maintain variable ordinance, Preschool/Kindergarten was shifted to 1, and the other categories accordingly. Finally, all entries with an educational value of 8 were removed. The pair plot below breaks down some of the post-processing relationships within the data:

![Pairplot of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/parwise.png)

Additionally, a correlation matrix was generated for continuous and ordinal data:

![Correlation Heatmap of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/correlations.png)

As conventional wisdom might suggest there appear to be slight correlations between education and age, and education and wealth. Additionally, there seems to be some discrepancy in education across genders in the correlation matrix that might not be as obvious in the pair plot. In fact, the pair plot itself does not reveal very many meaningful relationships, besides many of the variables being left-skewed in some capacity. This is particularly troubling in the case of education:

Histogram of Target Data
![Histogram of Target Data](https://jocain.github.io/Data-146-Extra-Credit/logeducation.png)

Because so many of the individuals interviewed have not received secondary or higher education (80.73 % have not), the models generated in this project are expected to err heavily on the side of low education. I, therefore, should be careful of overfitting in the case of mean squared errors (MSEs) close to one, even if the model does moderately well. This will also motivate class and distance weighting to mitigate this. 

To find the best method of modeling education using these demographic features, I used Logistic Regression, k-Nearest Neighbor (kNN), and Decision Tree classification models, in addition to a Random Forest Ensemble. 

### Logistic Regression

I applied multiple different logistic regression models to the data, varying the scaling or normalization method and distance weights. Either no scaling/normalization or one of the following sklearn.preprocessing classes were applied to the data prior to modeling: Normalizer, Standard Scaler, Robust Scaler, MinMax Scaler. The model itself either did or did not employ weighting proportional to the class proportions within the test data. The combinations of these settings resulted in a total of ten models, each of which underwent a K-Fold Validation for k = 15, and were set to a maximum iteration limit of 500 iterations. The table below shows the results of this:

|    Weighting     |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | Convergence? |
| No Class Weights |      None      | 0.572324799121 | 0.572441834470 | 2.450168270646 | 2.450307677369 | No |
|                  | Standard Scal. | 0.572338131565 | 0.572213730558 | 2.449476468733 | 2.449747711460 | Yes |
|                  |  MinMax Scal.  | 0.572345538018 | 0.572400400915 | 2.450046801805 | 2.448793494506 | Yes |
|                  |  Robust Scal.  | 0.572352945162 | 0.572213730558 | 2.449486839860 | 2.449415881722 | Yes |
|                  |   Normalizer   | 0.572345538018 | 0.572400400915 | 2.450046801805 | 2.448793494506 | Yes |
|  Class Weights   |      None      | 0.282613853026 | 0.281553551836 | 2.682674774940 | 2.685386078209 | No |
|                  | Standard Scal. | 0.282873092566 | 0.281906124159 | 2.682427390049 | 2.685946024762 | Yes |
|                  |  MinMax Scal.  | 0.283163442560 | 0.282341641012 | 2.682954752719 | 2.684888514253 | Yes |
|                  |  Robust Scal.  | 0.282855316270 | 0.281843909309 | 2.682451092172 | 2.685386052402 | Yes |
|                  |   Normalizer   | 0.439689086153 | 0.438508411392 | 2.150058054968 | 2.150437584659 | Yes |


As it turned out, scaling and normalization assisted with convergence, but did not help the overall accuracy of the models. This is likely due to native regularization methods within sklearn's LogisticRegression class. Class weighting did not appear to assist overfitting for these models, and actually severely stunted accuracy.

### k-Nearest Neighbor

For kNN modeling, I again applied different scaling and normalization methods, in addition to toggling distance weighting, in the absence of a class weighting option. The combination of the five scaling/normalization methods used in Logistic Regression modeling and the weighting options led to ten variations of kNN modeling. For each variation, a range of k-values was tested, ranging from k = 10 to k = 50. An example of this prior to focusing the k-variation window down is pictured below:

![k-NN k-Variation Example](https://jocain.github.io/Data-146-Extra-Credit/knnexample.png)

Once the optimal k value was found, the model then underwent K-Fold validation for k = 15 to get an accurate impression of the model accuracy and error. The results of the K-Fold Validation are below: 

|    Weighting     |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | k |
| No Dist. Weights |      None      | 0.714664944872 | 0.705338283165 | 1.373759355218 | 1.425193459279 | 63 |
|                  | Standard Scal. | 0.738759387849 | 0.706043260063 | 1.102121014479 | 1.229542634075 | 17 |
|                  |  MinMax Scal.  | 0.732823547319 | 0.713509196900 | 1.114683001322 | 1.192439350521 | 30 |
|                  |  Robust Scal.  | 0.730026725376 | 0.702372371006 | 1.135654738401 | 1.247792488990 | 33 |
|                  |   Normalizer   | 0.690857886226 | 0.670973453040 | 1.378535290621 | 1.463311864919 | 22 |
|  Dist. Weights   |      None      | 0.915914626025 | 0.681446935597 | 0.353272034898 | 1.463061728060 | 22 |
|                  | Standard Scal. | 0.915914626025 | 0.687357436668 | 0.353272034898 | 1.333670124516 | 45 |
|                  |  MinMax Scal.  | 0.915914626025 | 0.691795685219 | 0.353272034898 | 1.310607564172 | 39 |
|                  |  Robust Scal.  | 0.915914626025 | 0.688166326494 | 0.353272034898 | 1.329875341260 | 46 |
|                  |   Normalizer   | 0.716522533716 | 0.704535825798 | 0.682558462471 | 0.706238388749 | 48 |

The k-NN modeling attempts, in general, performed much better than Logistic Regression. Distance weighting introduced extraordinary amounts of overfitting and did not particularly help the test accuracy. The models seem to fail near the primary education level (education value of 1), resulting in an MSE of about 1 for all models. 

### Decision Tree/Random Forrest

Unlike the previous two modeling methods, no scaling or normalization was applied to the data prior to modeling. For Decision Tree modeling, the parameters of interest were maximum allowed tree depth and class weighting. For both weighted and unweighted modeling, a maximum tree depth range of 2 to 20 levels was tested, the best of which kept for K-Fold Validation. For Random Forrest Ensemble modeling, the parameters of interest were the same, in addition to the number of trees in the forest. Random Forrest modeling necessarily handled weighting differently, as a bootstrapping of the data was necessary to provide variation within the forest. As such, class weighting prior to and after the bootstrapping was possible. For all weighting options (no weights, pre-/post-bootstrap weighting), a maximum tree depth range of 2 to 15 levels and tree counts of 5, 10, 25, and 100 were tested, again the best of which being kept for K-Fold Validation. An example of this process is pictured below:

![k-NN k-Variation Example](https://jocain.github.io/Data-146-Extra-Credit/dtexample.png)

When appropriate, model simplicity was prioritized over accuracy. Specifically, in the graph above, n = 25, 50, and 100 have marginally different accuracies (on the order of 0.001%) that could easily be attributed to stochastic processes. I, therefore, chose n = 25, d = 13 as the optimal model here. Such priorities were in play when generating other models, but this is the only place where such a decision seemed appropriate. For both Decision Tree and Random Forrest modeling, the best models, as determined by model accuracy, The result of the K-Fold validation are below:

|     Model     |    Weighting   | Training Score |   Test Score   |  Training MSE  |    Test MSE   | d | n |
| Decision Tree |      None      | 0.724357530182 | 0.720768198230 | 1.199846856086 | 1.21740668659 | 7 | n/a |
|               | Class Weights  | 0.724170897618 | 0.637666475370 | 1.192854663078 | 1.51150955368 | 14 | n/a | 
| Random Forest |      None      | 0.754127088815 | 0.723257069656 | 1.043369975047 | 1.17437457881 | 11 | 25 |
|               |  Pre-Bootstrap | 0.900366191758 | 0.679206846149 | 0.421262371735 | 1.32906674176 | 13 | 25 |
|               | Post-Bootstrap | 0.900361746820 | 0.680285479900 | 0.421928990800 | 1.32811222157 | 9 | 5 |

The Decision Tree and Random Forrest modeling again outperformed Logistic Regression and performed about as well as k-NN modeling. It also likely experienced the same setbacks of weighting causing overfitting and a seemingly general failure around an education value of 1.

### Conclusions

The purpose of this project was to model education levels in Liberia using the Liberia Demographic and Health Survey of 2013. To do so, I produced a variety of Logistic Regression, k-NN, Decision Tree, and Random Forest models. As chosen by testing accuracy, the best models are tabulated below:

|     Model     |    Scaling      |    Weighting   | Training Score |   Test Score   |  Training MSE  |    Test MSE   |
| Random Forest |    No Scaler    |    No Weights  | 0.754127088815 | 0.723257069656 | 1.043369975047 | 1.17437457881 |
| Decision Tree |    No Scaler    |    No Weights  | 0.724357530182 | 0.720768198230 | 1.199846856086 | 1.21740668659 |
|     k-NN      |  MinMax Scaler  |    No Weights  | 0.732823547319 | 0.713509196900 | 1.114683001322 | 1.19243935052 |
|     k-NN      | Standard Scaler |    No Weights  | 0.738759387849 | 0.706043260063 | 1.102121014479 | 1.22954263407 |
|     k-NN      |    No Scaler    |    No Weights  | 0.714664944872 | 0.705338283165 | 1.373759355218 | 1.42519345927 |

In general, Decision Tree/Random Forest and k-NN modeling greatly outperformed Logistic Regression modeling:

![Avg Model Scores](https://jocain.github.io/Data-146-Extra-Credit/final.png)

While class and distance weighting did not particularly help results, I still believe that finding a way to address the difference in class proportions within the data is critical to improving these models. While purely speculative, in future variations of this project, I believe a further breakdown of location and wealth might help improve the models' accuracy, especially if location breakdowns can target particular counties or school districts, if such an equivalent to the American style of geographic schooling break downs exists there. Additionally, non-demographic factors such as access to water might also affect an individual's ability to attend school and might be a productive addition to the current dataset. 

### References

Liberia Institute of Statistics and Geo-Information Services (LISGIS), Ministry of Health and Social
Welfare \[Liberia], National AIDS Control Program \[Liberia], and ICF International. 2014. Liberia
Demographic and Health Survey 2013. Monrovia, Liberia: Liberia Institute of Statistics and GeoInformation Services (LISGIS) and ICF International. 


