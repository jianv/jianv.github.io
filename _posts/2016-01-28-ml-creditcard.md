---
title: "Machine Learning: Credit Card Fraudulent Transaction Detection using Python"
date: 2016-01-28
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
fig = plt.figure(dpi = 120)
ax = fig.subplots()
# nlegit and nfraud represent the numbers of legitimate and fraudulent transactions
ax.pie([nlegit, nfraud], explode = (0, 0.1), colors = ["green", "orange"], labels = ('Normal Transactions','Fraudulent Transactions'), autopct='%0.3f%%', shadow=False, startangle=135)
ax.axis('equal')
plt.show()
```

## 3 Methodology
Before modeling, a data preprocess pipeline was designed including missing value detection, shuffling and normalization using Python Scikit-learn. Then the Logistic Regression models were built seperately from raw datasets, undersampling datasets and oversampling datasets. To protect every valuable customer, the credit card company need to successfully detect each fraudulent transaction. So the term of recall was utilized on the assessment of algorithm performance instead of precision[link](http://scikit-learn.org/stable/auto_examples/model_selection/plot_precision_recall.html). 

Relevant Python code block:
```python
from sklearn.preprocessing import StandardScaler
# standarization and shuffling (data represents all credit card transactions)
data['normAmount'] = StandardScaler().fit_transform(data['Amount'].reshape(-1,1))
data_shuffled = data.sample(frac=1)
```

### 3.1 Raw Dataset
Intuitively, the raw datasets were utilized to realize the Logistic Regression model, where have 492 frauds out of 284,807 transactions. Firstly, the parameter of Inverse of Regularization Strength (IRS) was optimized by 5-folds cross-validation and l1 penalty was selected. The results showed that the mean recall value keeps growing with IRS increasing and converged at IRS of 100. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_1_cparam.png" alt="linearly separable data">

Therefore, the IRS of 100 was selected for Logistic Regression model and the confusion matrix showed that the recall value is 0.62. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_1.png" alt="linearly separable data">

### 3.2 Undersampling Dataset
To improve the results, the number of legitimate transactions was undersampled in the trainning dataset. Then the number of legitimate transactions is equal to the number of fraudulent transactions. After parameter optimization, the Logistic Regression was built by undersampling training dataset, and applied to the original test dataset. From the confusion matrix showed in the following figure, the value of recall was enhanced to 0.90. However, the value of the False Positive was pretty large. 

<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_2.png" alt="linearly separable data">

### 3.3 Oversampling Dataset
To further improve the results, the number of fraudulent transactions was oversampled in the trainning dataset. Then the number of fraudulent was increased to the number of legitimate transactions. Finally, the Logistic Regression classifier was trained by oversampled dataset, and applied to the original test dataset. The confusion matrix showed that recall value of 0.90 is similiar with previous results, but False Positive was decrese from XXX to XXX.
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_3.png" alt="linearly separable data">

## 4 Threshold Optimization
This section aims at opti
In the previous section, the default threshold value of 0.5 in Logistic Regression was used.  
The threshold in Logistic Regression should be ano value 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/3_1.png" alt="linearly separable data">

## 5.Conclusion and Discussion
From the comparision, the oversampling method is the best option that can mitigate the impact of unbalance dataset. Even the oversampled datasets increase the expense of computation, the enhanced results could meet the high requirement of fraudulent transaction detection. To futrher the improvement, the experiment paied attention on the threshold selection in Logistic Regression. It showed that a better threshold can evidently boost the results. 
