---
title: Investment Strategy
notebook: Strategy.ipynb
---

## Contents
{:.no_toc}
*  
{: toc}




```python
#IMPORTS
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns; sns.set(font_scale=1.15)
pd.options.display.max_rows = 100
pd.options.display.max_columns = 100
pd.options.display.float_format = '{:.10f}'.format
plt.rcParams['figure.figsize'] = (8, 5)
```




```python
#LOAD LOANSTATS
directory = '../../../data/'
ls = pd.read_hdf(directory + 'LS_CLEAN.h5', 'LS_CLEAN')
ls.sort_index(axis=1, inplace=True)
```


## Preprocessing

### Test Set



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


## Model Selection

## Key Variables Driving Investment Decisions

## Predictive Quality of Model

##  Expected Risk and Return
