---
title: "Machine Learning: Trading Electricity Price Prediction in the Day-ahead Market"
date: 2018-04-08
excerpt: "The grid company that offers electricity to their customs, so the prediction of electricity demand is valuable for them to make market strategies."
mathjax: "true"
---
## 1. Introduction
Electricity is produced at power plants and transmitted to consumers through grid companies. Even there are several ways to store the electricity, such as chemical storage, electrical storage, and thermal storage, the costs are increased largely. The best way is to generate the electricity depends on the demands of consumers. As the bridge between power plants and consumers, the grid company needs valuable predictions of electricity demands to make market strategies. 
 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/1_1.png" alt="linearly separable data">

## 2. Data
In this project, the historical electricity datasets are obtained from the FTP of New York State Independent Service Authority (NYISO) [link](http://mis.nyiso.com/public/P-58Blist.htm). There are mainly eleven load zones distributed around New York area, and the Capital sub-region (marked by a red star) is selected to build regression models. 
 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/2_1.png" alt="linearly separable data">

## 3. Feature Engineer
As a primary factor, the temperature impacts the electricity consumption notably. Hence, the weather dataset is considered from Weather Underground [link](https://www.wunderground.com/). The prediction of electricity demand is a problem based on time series, then more features are established based time stamps (Barta et al. 2015), such as day of the week, the day of the year, the day of the month, week of the year, etc.

## 4. Modeling
After data preprocessing, several regression models are implemented in this section. The goal of this experiment is to estimate the performance of each regression for this specific case of electricity prediction. 

### 4.1 Ridge Regression
There are mainly two categories of the features which are weather relevance and timestamp relevance. So, the collinearity maybe exists and the ridge regression is selected firstly. Because Ridge regression not only could reduce the variance, but always has a good performance when the input data are multi-colinear. The parameter of ‘alpha’ is paid attention in cross-validation, and the error is minimized when alpha reaches 15.
 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_1.png" alt="linearly separable data">

Relevant Python Code:
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

### 4.2 Ensembles
The unavoidable contamination of dataset by outlier and noise largely impacts the predictive results and it is usually hard to detect outliers ideally. One way to mitigate this problem is to apply ensemble methods. In this section, the Random Forest, Bagging (Bootstrap aggregating), Gradient Boosting Decision Tree (GBDT) and XGBoost are implemented after parameter tuning and the results are shown in the following the figure. It shows that the error approximately decreases from left to right. There is no improvement from Ridge to Random Forest, we can conclude that the noise distribution is similar for different features. The error decreased sharply in bagging and boosting because the noise distribution is various for different sub-samples. As was expected, the XGboost has the best performance. 
 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/4_2.png" alt="linearly separable data">

## 5. Feature Importance
To further investigation, the feature importance is calculated. The following figure shows the item of ‘day of the year (doy)’ is most significant. The reason is probably related to holidays. There are much more events for celebrating that cost large amount of electricity loads. The item of temperature is also quite significant because the electricity consumption is usually increased in summer and winter by the air conditioner, which is highly correlated with the seasonality of temperature. 
 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_electricity/5_1.png" alt="linearly separable data">

## 6. Conclusion
In this study, several regression models are implemented for this specific problem of electricity load prediction and the XGBoost outperforms other algorithms. In the feature importance investigation, the items of ‘doy’ and ‘temperature’ are visible significant that is probably related to the holidays and seasonality of weather.
