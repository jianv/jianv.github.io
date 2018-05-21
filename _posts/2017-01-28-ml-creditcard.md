---
title: "Machine Learning: Credit Card Fraudulent Transaction Detection using Python"
date: 2017-01-28
excerpt: "Since the dataset was unbalanced, the Logistic Regression classifier was implemented by both of undersampling and oversampling datasets to detect fraudulent transactions."
mathjax: "true"
---
## 1 Introduction
The capability of investigating fraudulent transactions is essential for credit card companies to protect their customers. However, the dataset is usually unbalanced, because the number of fraudulent transactions is much smaller than the number of legitimate transactions. In this project, the undersampling and oversampling methods are introduced seperately to solve this problem. 
  
## 2 Data  
The datasets were collected from European cardholders in two days. From the following figures, we could find that the dataset is highly unbalanced, the fradulent transactions account for 0.172% of all transactions. The datasets and more details are available on KAGGLE [link](https://www.kaggle.com/mlg-ulb/creditcardfraud).

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/1_1.png" alt="linearly separable data">

Relevant Python code block:
```python
import matplotlib.pyplot as plt
import pandas as pd

def run(path_read_data, path_save_plot):
    # read data    
    data = pd.read_csv("data/creditcard.csv")
    # check dataset intuitively
    nlegit = len(data[data.Class == 0])
    nfraud = len(data[data.Class == 1])
    # plot
    fig = plt.figure(dpi = 120)
    ax = fig.subplots()
    ax.pie([nlegit, nfraud], explode = (0, 0.1), colors = ["green", "orange"], labels = ('Normal Transactions','Fraudulent Transactions'), autopct='%0.3f%%', shadow=False, startangle=135)
    ax.axis('equal')
    plt.show()
    fig.savefig("plot/1_1.png")    
    return data
```

## 3 Methodology
Before modeling, a data preprocess pipeline was designed including missing value detection, shuffling and normalization using Python Scikit-learn. Then the Logistic Regression models were built seperately from raw datasets, undersampling datasets and oversampling datasets. To protect every valuable customer, the credit card company need to successfully detect each fraudulent transaction. So the term of recall was utilized on the assessment of algorithm performance instead of precision[link](http://scikit-learn.org/stable/auto_examples/model_selection/plot_precision_recall.html). 

### 3.1 Raw Dataset
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

### 3.2 Undersampling Dataset
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

### 3.3 Oversampling Dataset
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

## 4.Conclusion and Discussion
From the comparision, the oversampling method is the best option that can mitigate the impact of unbalance dataset. Even the oversampled datasets increase the expense of computation, the enhanced results could better meet the high requirement of fraudulent transaction detection. In the future work, the default threshold of the Logistic Regression could be another factor in the optimization.
