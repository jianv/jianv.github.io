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
There are several regression models were implemented in this section and the pros and cons were discussed in the next section.

### 4.1 Raw Dataset
Intuitively, the raw datasets were utilized to realize the Logistic Regression model, where have 492 frauds out of 284,807 transactions. Firstly, the parameter of Inverse of Regularization Strength (IRS) was optimized by 5-folds cross-validation and l1 penalty was selected. The results showed that the mean recall value keeps growing with IRS increasing and converged at IRS of 10. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cv_raw.png" alt="linearly separable data">

With considering the cost of computation, the IRS of 10 was selected for Logistic Regression model and the confusion matrix showed that the recall value is 0.62. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_raw.png" alt="linearly separable data">

Relevant Python libraries:
```python
from sklearn.cross_validation import KFold
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import recall_score 
from sklearn.metrics import confusion_matrix
```

### 4.2 Undersampling Dataset
To improve the results, the number of legitimate transactions was undersampled in the trainning dataset. Then the number of legitimate transactions is equal to the number of fraudulent transactions. After parameter optimization, the Logistic Regression was built by undersampling training dataset, and applied to the original test dataset. From the confusion matrix showed in the following figure, the value of recall was enhanced to 0.92. However, the value of the False Positive was pretty large. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_uds.png" alt="linearly separable data">

Relevant Python undersampling code:
```python
import numpy as np

def run(x_train, y_train):
    # index
    idx_fraud = y_train[y_train.Class == 1].index
    idx_legit = y_train[y_train.Class == 0].index
    nfraud = len(y_train[y_train.Class == 1])
    # random index
    randidx_legit = np.random.choice(idx_legit, nfraud, replace = False)
    idx_uds = np.concatenate([idx_fraud, randidx_legit])
    # undersampled dataset
    x_train_uds = x_train.loc[idx_uds, :]
    y_train_uds = y_train.loc[idx_uds, :]    
    return x_train_uds, y_train_uds
```

### 4.3 Oversampling Dataset
To further improve the results, the number of fraudulent transactions was oversampled in the trainning dataset. Then the number of fraudulent was increased to the number of legitimate transactions. Finally, the Logistic Regression classifier was trained by oversampled dataset, and applied to the original test dataset. The confusion matrix showed that recall value of 0.92 that is same with previous results, but False Positive was decrese from 7780 to 2097.

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_cm_os.png" alt="linearly separable data">

Relevant Python oversampling code:
```python
import pandas as pd
from imblearn.over_sampling import SMOTE

def run(x_train, y_train):
    # Smote
    oversampler=SMOTE(random_state=0)
    x_train_smote, y_train_smote = oversampler.fit_sample(x_train, y_train)
    x_train_os = pd.DataFrame(x_train_smote)
    y_train_os = pd.DataFrame(y_train_smote)
    # print
    nfraud = len(y_train_os[y_train_os.values == 1])
    nlegit = len(y_train_os[y_train_os.values == 0])
    print("Fraud/Legitimate = %d/%d" %(nfraud, nlegit))
    # return
    return x_train_os, y_train_os
```

## 5.Conclusion and Discussion
From the comparision, the oversampling method is the best option that can mitigate the impact of unbalance dataset. Even the oversampled datasets increase the expense of computation, the enhanced results could better meet the high requirement of fraudulent transaction detection. In the future work, the default threshold of the Logistic Regression could be another factor in the optimization.
