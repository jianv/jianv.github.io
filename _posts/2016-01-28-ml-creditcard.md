---
title: "Machine Learning: Credit Card Fraudulent Transaction Detection using Python"
date: 2016-01-28
excerpt: "Since the dataset is unbalanced, both of undersampling and oversampling methods are applied to detect fraudulent transactions."
mathjax: "true"
---
## 1 Introduction
The capability of investigating fraudulent transactions is essential for credit card companies to protect their customers. However, the dataset is usually unbalanced, because the number of fraudulent transactions is much smaller than the number of legitimate transactions. In this project, the undersampling and oversampling methods are introduced to solve this problem. 
  
## 2 Data  
The datasets were collected from European cardholders in two days. From the following figures, we could find that the dataset is highly unbalanced, the fradulent transactions account for 0.172% of all transactions. The datasets and more details are available on KAGGLE [link](https://www.kaggle.com/mlg-ulb/creditcardfraud).
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/1_1.png" alt="linearly separable data">
Related Python code block:
```python
    ax.pie([nlegit, nfraud], explode = (0, 0.1), colors = ["green", "orange"], labels = ('Normal Transactions','Fraudulent Transactions'), autopct='%0.3f%%', shadow=False, startangle=135)
    ax.axis('equal')
```

## 3 Methodology
Before machine learning algorithm implementation, a data preprocess pipeline was designed including missing value detection, shuffling and normalization using Python Scikit-learn. Then the Logistic Regression models were built seperately from raw datasets, undersampling datasets abd oversampling datasets.
Related Python code block:
```python
    from sklearn.preprocessing import StandardScaler
    data['normAmount'] = StandardScaler().fit_transform(data['Amount'].reshape(-1,1))
```

### 3.1 Raw Dataset
Intuitively, the raw datasets were utilized to realize the Logistic Regression model, where have 492 frauds out of 284,807 transactions. Firstly, the parameter of Inverse of Regularization Strength (IRS) was optimized by 5-folds cross-validation and l1 penalty was selected. The results showed that the mean recall value keeps growing with IRS increasing.
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_1_cparam.png" alt="linearly separable data">
Therefore, the IRS of 100 was selected for Logistic Regression modeland and the confusion matrix showed that the recall value is only 0.62. 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_1.png" alt="linearly separable data">
Related Python code block:
```python
    def cross_validation(x_train, y_train, c_param_range):
      # k-fold and result table
      fold = KFold(len(y_train),5,shuffle=False) 
      results_table = pd.DataFrame(index = range(len(c_param_range),2), columns = ['C_parameter','Mean recall score'])
      results_table['C_parameter'] = c_param_range
      
      # the k-fold will give 2 lists: train_indices = indices[0], test_indices = indices[1]
      j = 0
      for c_param in c_param_range:
        recall_accs = []
        for iteration, indices in enumerate(fold,start=1):
          # Call the logistic regression model with a certain C parameter
          lr = LogisticRegression(C = c_param, penalty = 'l1')
          lr.fit(x_train.iloc[indices[0],:], y_train.iloc[indices[0],:].values.ravel()) 
          
          # Predict values using the test indices in the training data
          y_pred_undersample = lr.predict(x_train.iloc[indices[1],:].values)
          recall_acc = recall_score(y_train.iloc[indices[1],:].values,y_pred_undersample)
          recall_accs.append(recall_acc)

      # The mean value of those recall scores is the metric we want to save and get hold of.
      results_table.ix[j,'Mean recall score'] = np.mean(recall_accs)
      j += 1
```

### 3.2 Undersampling Dataset
To improve the results, the undersampling method was present to the raw dataset
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_2.png" alt="linearly separable data">

### 3.3 Oversampling Dataset
Even though undersampling method improve the recall significantly, the value of negative True is pretty large. Hence, the oversampling method is introdueced to the rawdataset. 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/2_3.png" alt="linearly separable data">
Related Python code block:
```python
    from imblearn.over_sampling import SMOTE
    oversampler = SMOTE(random_state=0)
    x_train_oversample, y_train__oversample = oversampler.fit_sample(x_train, y_train)
```

## 4 Threshold Optimization
In the previous section, the default threshold value in Logistic Regression was used.  
The threshold in Logistic Regression should be ano value 
<img src="{{ site.url }}{{ site.baseurl }}/images/ml_creditcard/3_1.png" alt="linearly separable data">

## 5.Conclusion and Discussion
From the comparision, the oversampling method is the best option that can improve the result significantly. Even the oversampled datasets increased the expense of computation, the enhanced results could meet the high requirement of fraudulent transaction detection. To futrher the improvement, the experiment paied attention on the threshold selection in Logistic Regression. It showed that a better threshold can evidently boost the results. 
