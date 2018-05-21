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
In this project, the historical electricity datasets were obtained from the FTP of New York State Independent Service Authority (NYISO) [link](http://mis.nyiso.com/public/P-58Blist.htm). There were mainly eleven load zones distributed around New York area, and the Capital subregion was selected to build regresson models. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/2_1_NYISO.png" alt="linearly separable data">

## 3 Feature Engineer
As a primary factor, the temperature impacts the electricity consumption notably. Hence, the weather dataset was considered from Weather Underground [link](https://www.wunderground.com/). The prediction of electricity demand is a problem based on time series, then more features were established based time stamps (Barta et al. 2015[link](https://arxiv.org/pdf/1506.06972.pdf)). The rule of new fearues were as follow:
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
Since there are several features related to time stamp, the ridge regresson was used that can mitigate multicollinearity. From the cross validation, we could find that the error is minimum when alpha reaches 15.

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_1_cv_ridge.png" alt="linearly separable data">

Relevant Python libraries:
```python
from sklearn.linear_model import Ridge
from sklearn.model_selection import cross_val_score

alphas = np.logspace(-3, 2, 50)
errors = []
for alpha in alphas:
    clf = Ridge(alpha)
    error = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    errors.append(np.mean(error))
```

### 4.2 Random Forest
Random forest is an ensemble learning method that operate by constructing a multitude of decision trees. It can also mitigate the impacts of multicollinearity. When the maximum feature was set to 30%, the error arrived its valley of 0.137.

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_2_cv_random_forester.png" alt="linearly separable data">

Relevant Python undersampling code:
```python
from sklearn.ensemble import RandomForestRegressor

max_features = [.1, .3, .5, .7, .9, .99]
errors = []
for max_feat in max_features:
    clf = RandomForestRegressor(n_estimators=200, max_features=max_feat)
    error = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=5, scoring='neg_mean_squared_error'))
    errors.append(np.mean(error))
```

### 4.3 Bagging
Bagging is also an ensemble method that operates by creating many random sub-samples of dataset and trains a regression tree on each sub-sample. Final output considers all the sub-sample results. When the number of estimators is set to 25, the errors obtain its minimum of 0.132. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_3_bagging_ridge.png" alt="linearly separable data">

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
Boosting is based on bagging, but considers the performance of previous models. When the number of estimators is set to 40, the errors obtain its minimum of 0.133. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_4_cv_boosting.png" alt="linearly separable data">

```python
from sklearn.ensemble import AdaBoostRegressor

params = [10, 15, 20, 25, 30, 35, 40, 45, 50]
test_scores = []
for param in params:
    clf = AdaBoostRegressor(n_estimators=param, base_estimator=ridge)
    test_score = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    test_scores.append(np.mean(test_score))
```
### 4.5 XGBoost
XGBoost is an implementation of graident boosting machines. It not only considers regularization that can reducing overfitting, but also allow users to define custimized optimizer. When the depth of decision tree was set to 5, the error was reduced to 0.127.

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_5_cv_xgboost.png" alt="linearly separable data">

```python
from xgboost import XGBRegressor
params = [1,2,3,4,5,6]
errors = []
for param in params:
    clf = XGBRegressor(max_depth=param)
    error = np.sqrt(-cross_val_score(clf, X_train, y_train, cv=10, scoring='neg_mean_squared_error'))
    errors.append(np.mean(error))
```

## 5.Conclusion and Discussion
NoteDecistion Tree is worse than ridge regression
