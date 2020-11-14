# Classification Analysis of Liberian Demographic Data

### Description

In this project, I explore a variety of classification methods as a means of modeling education levels in a set of data concerning Liberian demographic information. 

### Data Overview

The data used in this project was collected in the Liberia Demographic and Health Survey of 2013. The survey was conducted by the Liberia Institute of Statistics and Geo-Information (LISGI) from mid-March to mid-July in 2013 and was funded by a number of international government and non-government organizations, including USAID and UNICEF. The survey covered a broad stroke of information ranging from HIV testing to access to drinking water. For the purposes of this project, Location, Size, Wealth, Gender, and Age were used as features in an attempt to model a target variable, Education. 

Location and Gender are nominal variables, Age and Size are treated as continuous variables, and Wealth and Education ordinal variable. Location refers to six groups of counties partitioned by LISGI, designated as the North Western, North Central, South Central, South Eastern A, and South-Eastern B regions, shown in the map below:

![Map](https://jocain.github.io/Data-146-Extra-Credit/map.png)

The survey was conducted such that the random houses in each of the regions were chosen, then all the members of the household answered the survey. Therefore, while Gender, Age, Education refer to an individual's qualities, Location, Size, and Wealth refer to attributes of the household they belong to. Household wealth in this study is broken up into quintiles, and Size refers to the number of members of a household. Of those members, Gender is taken to be binary (male or female), Age is the member's age, and Education is ranked by the highest level of education the member has received: None(0), Preschool/Kindergarten(1), Primary(2), Secondary(3), and Tertiary(4). This particular variable required some cleaning and amending. The original mapping of the survey responses was the following: None(0), Preschool/Kindergarten(6), Primary(1), Secondary(2), Tertiary(3), and Unknown(8). In fact, there was never any recording of an educational value of 6 anywhere in the dataset. However, some responses recorded an education value of 9, which is not valid, and was assumed to be 6. To maintain variable ordinance, Preschool/Kindergarten was shifted to 1, and the other categories accordingly. Finally, all entries with an educational value of 8 were removed. The pair plot below breaks down some of the post-processing relationships within the data:

![Pairplot of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/parwise.png)

Additionally, a correlation matrix was generated for continuous and ordinal data:

![Correlation Heatmap of Demographics Data](https://jocain.github.io/Data-146-Extra-Credit/correlations.png)

As conventional wisdom might suggest there appear to be slight correlations between education and age, and education and wealth. Additionally, there seems to be some discrepancy in education across genders in the correlation matrix that might not be as obvious in the pair plot. In fact, the pair plot itself does not reveal very many meaningful relationships, besides many of the variables being left-skewed in some capacity. This is particularly troubling in the case of education:

Histogram of Target Data
![Histogram of Target Data](https://jocain.github.io/Data-146-Extra-Credit/logeducation.png)

Because so many of the individuals interviewed have not received secondary or higher education (80.85 % have not), the models generated in this project are expected to err heavily on the side of low education. I, therefore, should be careful of overfitting in the case of mean squared errors (MSEs) close to one, even if the model does moderately well. This will also motivate class and distance weighting to mitigate this. 

To find the best method of modeling education using these demographic features, I used Logistic Regression, k-Nearest Neighbor (kNN), and Decision Tree classification models, in addition to a Random Forest Ensemble. 

### Logistic Regression

I applied multiple different logistic regression models to the data, varying the scaling or normalization method and distance weights. Either no scaling/normalization or one of the following sklearn.preprocessing classes were applied to the data prior to modeling: Normalizer, Standard Scaler, Robust Scaler, MinMax Scaler. The model itself either did or did not employ weighting proportional to the class proportions within the test data. The combinations of these settings resulted in a total of ten models, each of which underwent a K-Fold Validation for k = 15, and were set to a maximum iteration limit of 500 iterations. The table below shows the results of this:

|    Weighting     |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    | Convergence? |
| No Class Weights |      None      | 0.573177896913 | 0.573212146393 | 0.941012565680 | 0.940736933087 | No  |
|                  | Standard Scal. | 0.573241698116 | 0.573232966601 | 0.940698012357 | 0.940861265385 | Yes |

|                  |  MinMax Scal.  | 0.573246149782 | 0.573274509937 | 0.940749941163 | 0.940757377922 | Yes |
|                  |  Robust Scal.  | 0.573238730735 | 0.573253747977 | 0.940714333415 | 0.940778139882 | Yes |
|                  |   Normalizer   | 0.573246149782 | 0.573274509937 | 0.940749941163 | 0.940757377922 | Yes |
|  Class Weights   |      None      | 0.459977684085 | 0.459815938234 | 1.271575775154 | 1.272595170579 | No  |
|                  | Standard Scal. | 0.460022196056 | 0.459961330202 | 1.271480817308 | 1.271930496620 | Yes |
|                  |  MinMax Scal.  | 0.459842663723 | 0.459815925290 | 1.272004575652 | 1.272428990763 | Yes |
|                  |  Robust Scal.  | 0.460011809958 | 0.459961330202 | 1.271614353221 | 1.272221306443 | Yes |
|                  |   Normalizer   | 0.551395158917 | 0.551173066922 | 1.063744303841 | 1.064393374423 | Yes |


As it turned out, scaling and normalization assisted with convergence, but did not help the overall accuracy of the models. This is likely due to native regularization methods within sklearn's LogisticRegression class. Class weighting did not appear to assist fitting or overfitting for these models. 

### k-Nearest Neighbor

For kNN modeling, I again applied different scaling and normalization methods, in addition to toggling distance weighting, in the absence of a class weighting option. The combination of the five scaling/normalization methods used in Logistic Regression modeling and the weighting options led to ten variations of kNN modeling. For each variation, a range of k-values was tested, ranging from k = 10 to k = 50. An example of this prior to focusing the k-variation window down is pictured below:

![k-NN k-Variation Example](https://jocain.github.io/Data-146-Extra-Credit/knnexample.png)

Once the optimal k value was found, the model then underwent K-Fold validation to get an accurate impression of the model accuracy and error. The results of the K-Fold Validation are below: 

|    Weighting     |   Scal./Norm.  | Training Score |   Test Score   |  Training MSE  |    Test MSE    |
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

Unlike the previous two modeling methods, no scaling or normalization was applied to the data prior to modeling. For Decision Tree modeling, the parameters of interest were maximum allowed tree depth and class weighting. For both weighted and unweighted modeling, a maximum tree depth range of 2 to 20 levels was tested, the best of which kept for K-Fold Validation. For Random Forrest Ensemble modeling, the parameters of interest were the same, in addition to the number of trees in the forest. Random Forrest modeling necessarily handled weighting differently, as a bootstrapping of the data was necessary to provide variation within the forest. As such, class weighting prior to and after the bootstrapping was possible. For all weighting options (no weights, pre-/post-bootstrap weighting), a maximum tree depth range of 2 to 15 levels and tree counts of 5, 10, 25, and 100 were tested, again the best of which being kept for K-Fold Validation. An example of this process is pictured below:

For both Decision Tree and Random Forrest modeling, the best models, as determined by model accuracy, The result of the K-Fold validation are below:

|    Model    |   Weighting  | Training Score |   Test Score   |  Training MSE  |    Test MSE   |
### Conclusions


