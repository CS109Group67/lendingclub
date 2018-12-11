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

















## 2. Model Selection

## 3. Key Variables Driving Investment Decisions

## 4. Predictive Quality of Model

##  5. Expected Risk and Return
