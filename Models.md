---
title: Models
notebook: Models.ipynb
nav_include: 3
---

## Contents
{:.no_toc}
*  
{: toc}










<br><br>
## 1. Data Preprocessing

### 1A. Train-Test Split

We split the `ls` dataset into a train and test part. We do this in a stratified fashion ensuring that the outcome classes (fully paid loans and not fully paid loans) are equally represented in each set. For the splitting algorithm, we use `sklearn`'s `train_test_split` function. This function creates random train and test subsets of the dataset. The flag `stratify` ensures that both classes are equally represented in each set. 



```python
from sklearn.model_selection import train_test_split
ls_train, ls_test = train_test_split(ls, test_size=0.2, stratify=ls['OUT_Class'])
```


### 1B. Standard Scaling

Most of the models used later will assume all features are on similar scales. To achieve this, we use standardization to transform the numeric variables such that they have a mean of 0 and standard deviation of 1. We use `sklearn`'s `StandardScaler` function.



```python
#IDENTIFY THE OUTCOME, DUMMY AND NUMERIC VARIABLES
var_list = set(ls.columns)
outcome_var_list = set(out_var for out_var in var_list if "OUT_" in out_var)
dummy_var_list = set(dummy for dummy in var_list if "D_" in dummy)
numeric_var_list = var_list - outcome_var_list - dummy_var_list
```




```python
#STANDARD SCALER
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
```




```python
#SCALE THE TRAINING SETS (fit_transform)
train_vars_scaled = pd.DataFrame(scaler.fit_transform(ls_train[list(numeric_var_list)]),
                                 index=ls_train.index, 
                                 columns=ls_train[list(numeric_var_list)].columns)

feature_train = pd.concat([train_vars_scaled, 
                           ls_train[list(dummy_var_list)]], 
                          axis=1).sort_index(axis=1)

outcome_train = ls_train[list(outcome_var_list)]
```




```python
#STANDARDIZE THE TEST SET (transform)
test_vars_scaled = pd.DataFrame(scaler.transform(ls_test[list(numeric_var_list)]),
                                index=ls_test.index, 
                                columns=ls_test[list(numeric_var_list)].columns)

feature_test = pd.concat([test_vars_scaled, 
                          ls_test[list(dummy_var_list)]], 
                         axis=1).sort_index(axis=1)

outcome_test = ls_test[list(outcome_var_list)]

```


## Classification



```python
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression(random_state=0)
model_logreg = logreg.fit(feature_train, outcome_test.iloc[:,0])

```


    /Users/michal/anaconda3/lib/python3.6/site-packages/sklearn/linear_model/logistic.py:432: FutureWarning: Default solver will be changed to 'lbfgs' in 0.22. Specify a solver to silence this warning.
      FutureWarning)



    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-47-7a4b50aacbb2> in <module>()
          1 from sklearn.linear_model import LogisticRegression
          2 logreg = LogisticRegression(random_state=0)
    ----> 3 model_logreg = logreg.fit(feature_train, outcome_test.iloc[:,0])
    

    ~/anaconda3/lib/python3.6/site-packages/sklearn/linear_model/logistic.py in fit(self, X, y, sample_weight)
       1282 
       1283         X, y = check_X_y(X, y, accept_sparse='csr', dtype=_dtype, order="C",
    -> 1284                          accept_large_sparse=solver != 'liblinear')
       1285         check_classification_targets(y)
       1286         self.classes_ = np.unique(y)


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/validation.py in check_X_y(X, y, accept_sparse, accept_large_sparse, dtype, order, copy, force_all_finite, ensure_2d, allow_nd, multi_output, ensure_min_samples, ensure_min_features, y_numeric, warn_on_dtype, estimator)
        755         y = y.astype(np.float64)
        756 
    --> 757     check_consistent_length(X, y)
        758 
        759     return X, y


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/validation.py in check_consistent_length(*arrays)
        228     if len(uniques) > 1:
        229         raise ValueError("Found input variables with inconsistent numbers of"
    --> 230                          " samples: %r" % [int(l) for l in lengths])
        231 
        232 


    ValueError: Found input variables with inconsistent numbers of samples: [336144, 84037]


## Regression

## Trees and Forest

## K-Nearest Neighbors
