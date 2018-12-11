---
title: Investment Strategy
notebook: Strategy.ipynb
nav_include: 4
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
directory = '../../data/'
ls = pd.read_hdf(directory + 'ls_CLEAN.h5', 'ls_CLEAN')
ls.sort_index(axis=1, inplace=True)
```


## 1. Preprocessing

### Test Set














    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-8-c5218a2ddf05> in <module>()
          1 #SCALE THE TRAINING SETS (fit_transform)
    ----> 2 train_vars_scaled = pd.DataFrame(scaler.fit_transform(ls_train[list(numeric_var_list)]),
          3                                  index=ls_train.index,
          4                                  columns=ls_train[list(numeric_var_list)].columns)
          5 feature_train = pd.concat([train_vars_scaled, 


    NameError: name 'ls_train' is not defined






## 2. Model Selection

## 3. Key Variables Driving Investment Decisions

## 4. Predictive Quality of Model

##  5. Expected Risk and Return
