---
title: "Machine Learning: Trading Electricity Price Prediction in the Day-ahead Market"
date: 2018-04-20
excerpt: "The grid company that offers electricity to their customs, so the prediction of electricity demand is valuable for them to make market strategies."
mathjax: "true"
---
## 1 Introduction
Electricity is produced at power plants and transmitted to consumers through grid companies. Even there are several ways to store the electricity, such as chemical storage, electrical storage and thermal storage, the costs are increased largely. The best way is to generate the electricity depends on the demands of consumers. As the bridge between power plants and consumers, the grid company need valuable predictions of electricity demands to make market strategies. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/1_1_company.png" alt="linearly separable data">

## 2 Data  
In this project, the historical electricity datasets were obtained from the FTP of New York State Independent Service Authority (NYISO) [link](http://mis.nyiso.com/public/P-58Blist.htm). There were mainly eleven load zones distributed around New York area, and the Capital subregion was selected to build regresson models. As a primary factor, the temperature impacts the electricity consumption notably. Hence, the weather dataset was considered from Weather Underground [link](https://www.wunderground.com/). 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/2_1_NYISO.png" alt="linearly separable data">

## 3 Feature Engineer
The prediction of electricity demand is a problem based on time series, then more features were established based time stamps (Barta et al. 2015[link](https://arxiv.org/pdf/1506.06972.pdf)). The rule of new fearues were as follow:
* dow: day of the week (integer 0-6)
* doy: day of the year (integer 0-365)
* day: day of the month (integer 1-31)
* woy: week of the year (integer 1-52)
* month: month of the year (integer 1-12)
* hour: hour of the day (integer 0-23)
* minute: minute of the day (integer 0-1339)

# 4 Modeling
After data preprocess, there are several regression models were implemented in this section. Then the pros and cons were discussed in the next section.

### 4.1 Ridge Regression
Intuitively, the raw datasets were utilized to realize the Logistic Regression model, where have 492 frauds out of 284,807 transactions. Firstly, the parameter of Inverse of Regularization Strength (IRS) was optimized by 5-folds cross-validation and l1 penalty was selected. The results showed that the mean recall value keeps growing with IRS increasing and converged at IRS of 10. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cv_raw.png" alt="linearly separable data">

With considering the cost of computation, the IRS of 10 was selected for Logistic Regression model and the confusion matrix showed that the recall value is 0.62. 
可见，大概alpha=10~20的时候，可以把score达到0.135左右。
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_raw.png" alt="linearly separable data">

Relevant Python libraries:
```python
from sklearn.linear_model import Ridge
from sklearn.model_selection import cross_val_score

alphas = np.logspace(-3, 2, 50)
test_scores = []
for alpha in alphas:
    clf = Ridge(alpha)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))
```

### 4.2 Random Forest
To improve the results, the number of legitimate transactions was undersampled in the trainning dataset. Then the number of legitimate transactions is equal to the number of fraudulent transactions. After parameter optimization, the Logistic Regression was built by undersampling training dataset, and applied to the original test dataset. From the confusion matrix showed in the following figure, the value of recall was enhanced to 0.92. However, the value of the False Positive was pretty large. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_uds.png" alt="linearly separable data">

用RF的最优值达到了0.137

Relevant Python undersampling code:
```python
from sklearn.ensemble import RandomForestRegressor

max_features = [.1, .3, .5, .7, .9, .99]
test_scores = []
for max_feat in max_features:
    clf = RandomForestRegressor(n_estimators=200, max_features=max_feat)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=5, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))

plt.plot(max_features, test_scores)
plt.title("Max Features vs CV Error");
```

### 4.3 Bagging
To further improve the results, the number of fraudulent transactions was oversampled in the trainning dataset. Then the number of fraudulent was increased to the number of legitimate transactions. Finally, the Logistic Regression classifier was trained by oversampled dataset, and applied to the original test dataset. The confusion matrix showed that recall value of 0.92 that is same with previous results, but False Positive was decrese from 7780 to 2097.

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_os.png" alt="linearly separable data">
可见，前一个版本中，ridge最优结果也就是0.135；而这里，我们使用25个小ridge分类器的bagging，达到了低于0.132的结果。

当然了，你如果并没有提前测试过ridge模型，你也可以用Bagging自带的DecisionTree模型：
Relevant Python oversampling code:
```python
from sklearn.linear_model import Ridge
from sklearn.ensemble import BaggingRegressor
from sklearn.model_selection import cross_val_score
ridge = Ridge(15)

params = [1, 10, 15, 20, 25, 30, 40]
test_scores = []
for param in params:
    clf = BaggingRegressor(n_estimators=param, base_estimator=ridge)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))
```
### 4.4 Boosting
```python
from sklearn.ensemble import AdaBoostRegressor
params = [10, 15, 20, 25, 30, 35, 40, 45, 50]
test_scores = []
for param in params:
    clf = BaggingRegressor(n_estimators=param, base_estimator=ridge)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))
```
### 4.5 XGBoost
惊了，深度为5的时候，错误率缩小
到0.127
```python
from xgboost import XGBRegressor
params = [1,2,3,4,5,6]
test_scores = []
for param in params:
    clf = XGBRegressor(max_depth=param)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))
```
## 5.Conclusion and Discussion
NoteDecistion Tree is worse than ridge regression
