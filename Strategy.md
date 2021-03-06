---
title: Investment Strategy
notebook: Strategy.ipynb
nav_include: 4
---

## Contents
{:.no_toc}
*  
{: toc}






























## 1. Modeling Summary





In the [Modeling](https://cs109group67.github.io/lendingclub/Modeling.html) section we performed a variety of classification and regression models on the three outcome features. A summary of the model scoring metrics is as follows:






**`OUT_Class`:**





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>Model</th>
      <th colspan="3" halign="left">Training Scores</th>
      <th colspan="3" halign="left">Testing Scores</th>
    </tr>
    <tr>
      <th></th>
      <th>Accuracy</th>
      <th>Precision</th>
      <th>Recall</th>
      <th>Accuracy</th>
      <th>Precision</th>
      <th>Recall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Baseline Classifier</th>
      <td>0.502</td>
      <td>0.864</td>
      <td>0.502</td>
      <td>0.501</td>
      <td>0.864</td>
      <td>0.502</td>
    </tr>
    <tr>
      <th>Logistic Classifier</th>
      <td>0.565</td>
      <td>0.926</td>
      <td>0.540</td>
      <td>0.565</td>
      <td>0.925</td>
      <td>0.541</td>
    </tr>
  </tbody>
</table>
</div>








**`OUT_Principle_Repaid`:**





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>Model</th>
      <th colspan="2" halign="left">Training Scores</th>
      <th colspan="2" halign="left">Testing Scores</th>
    </tr>
    <tr>
      <th></th>
      <th>Neg MSE</th>
      <th>R2</th>
      <th>Neg MSE</th>
      <th>R2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Baseline Regressor</th>
      <td>-0.002</td>
      <td>-0.000</td>
      <td>-0.002</td>
      <td>-0.000</td>
    </tr>
    <tr>
      <th>Linear Regressor</th>
      <td>-0.002</td>
      <td>0.007</td>
      <td>-0.002</td>
      <td>0.005</td>
    </tr>
    <tr>
      <th>Ridge Regressor</th>
      <td>-0.002</td>
      <td>0.007</td>
      <td>-0.002</td>
      <td>0.006</td>
    </tr>
    <tr>
      <th>Lasso Regressor</th>
      <td>-0.002</td>
      <td>0.008</td>
      <td>-0.002</td>
      <td>0.006</td>
    </tr>
    <tr>
      <th>Polynomial Regressor</th>
      <td>-0.002</td>
      <td>0.009</td>
      <td>-0.002</td>
      <td>0.006</td>
    </tr>
  </tbody>
</table>
</div>








**`OUT_Monthly_Rate_Of_Return`:**





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>Model</th>
      <th colspan="2" halign="left">Training Scores</th>
      <th colspan="2" halign="left">Testing Scores</th>
    </tr>
    <tr>
      <th></th>
      <th>Neg MSE</th>
      <th>R2</th>
      <th>Neg MSE</th>
      <th>R2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Baseline Regressor</th>
      <td>-0.047</td>
      <td>-0.000</td>
      <td>-0.047</td>
      <td>-0.000</td>
    </tr>
    <tr>
      <th>Linear Regressor</th>
      <td>-0.998</td>
      <td>-20.427</td>
      <td>-0.950</td>
      <td>-19.218</td>
    </tr>
    <tr>
      <th>Ridge Regressor</th>
      <td>-0.921</td>
      <td>-18.730</td>
      <td>-0.895</td>
      <td>-18.010</td>
    </tr>
    <tr>
      <th>Lasso Regressor</th>
      <td>-0.895</td>
      <td>-18.170</td>
      <td>-0.894</td>
      <td>-17.993</td>
    </tr>
    <tr>
      <th>Polynomial Regressor</th>
      <td>-305.134</td>
      <td>-6566.432</td>
      <td>-0.676</td>
      <td>-13.230</td>
    </tr>
  </tbody>
</table>
</div>



The classification and regression models did not perform well at predicting the target features, but they did provide important information about which variables potentially hold the most inferential information for a prospective investor. The visualizations below display cofficients assigned across the models. The focus is on features that have the most significant magnitude across distinct models.










![png](Strategy_files/Strategy_16_0.png)







![png](Strategy_files/Strategy_17_0.png)







![png](Strategy_files/Strategy_18_0.png)


## 2. Key Variables Driving Investment Decisions

The modeling has revealed key variables that should power the investment decisions: those which have significant coefficients across the distinct classification and regression models. These form the subset for our investment strategy formulation. This will aid interpretation and understanding for the investor at the loss of minimal investment efficacy. 

### 2A. Key Variables





    0         D_home_ownership_RENT
    1             D_term_ 36 months
    2          acc_open_past_24mths
    3                           dti
    4                    emp_length
    5                      int_rate
    6                      num_sats
    7                      open_acc
    8                     sub_grade
    9    total_il_high_credit_limit
    Name: key_variables, dtype: object


### 2B. Decision Tree

With the key variables established, we examine decision tree and random forest analysis on this key subset to establish the investment strategy. We focus on the most comprehensive outcome feature of `OUT_Monthly_Rate_Of_Return` since this feature takes into account the total amount repeiad with interest for the effective term of the loan. We visualize both a larger tree (for completeness and demonstrative purposes) and a simplified tree. 





    DecisionTreeRegressor(criterion='mse', max_depth=10, max_features=None,
               max_leaf_nodes=None, min_impurity_decrease=0.0,
               min_impurity_split=None, min_samples_leaf=1,
               min_samples_split=0.05, min_weight_fraction_leaf=0.0,
               presort=False, random_state=0, splitter='best')



![svg](Strategy_files/Strategy_25_1.svg)


    


### 2C. Simplified Decision Tree





    DecisionTreeRegressor(criterion='mse', max_depth=10, max_features=None,
               max_leaf_nodes=10, min_impurity_decrease=0.0,
               min_impurity_split=None, min_samples_leaf=1,
               min_samples_split=0.05, min_weight_fraction_leaf=0.0,
               presort=False, random_state=0, splitter='best')



![svg](Strategy_files/Strategy_27_1.svg)


    


### 2D. Random Forest





    RandomForestRegressor(bootstrap=True, criterion='mse', max_depth=None,
               max_features='auto', max_leaf_nodes=None,
               min_impurity_decrease=0.0, min_impurity_split=None,
               min_samples_leaf=1, min_samples_split=2,
               min_weight_fraction_leaf=0.0, n_estimators=10, n_jobs=None,
               oob_score=False, random_state=None, verbose=0, warm_start=False)



![png](Strategy_files/Strategy_29_1.png)


## 3. Investment Strategy

Based on the completed analysis, we postulate the following simple investment strategy to only invest in loans that meet the following criteria. To reduce risk and get the full benefit of diversification, we recommended that the mimimun qualifying investment should be made across as many qualifying loans as possible.

#### Non-Renter

Only invest in applications from non-renters.



```python
non_renters = (ls['D_home_ownership_RENT'] == 0)
```


#### Shorter Term

Only invest in applications with term of 36 months.



```python
short_term = (ls['D_term_ 36 months'] == 1)
```


#### Few Recent Accounts Opened

Only invest in applications from borrowers with less than 4 accounts opened in the last 24 months.



```python
few_recent_accounts = (ls['acc_open_past_24mths'] < 4)
```


#### Low Debt-to-Income Ratio

Only invest in applications from borrowers with debt-to-equity ratios of less than 20%.



```python
low_dti = (ls['dti'] < 20)
```


#### Solidly Employed

Only invest in applications from borrowers who have been employed over 5 years.



```python
solid_emp = (ls['emp_length'] > 5)
```


#### Low Grade

Only invest in loans of grade A, B, C or D



```python
low_grade = (ls['sub_grade'] <= 20)
```


## 4. Predictive Quality of Strategy

The investment strategy reduces the size of potential applications to about 9.6% of loans.



```python
strategy = non_renters & short_term & few_recent_accounts & low_dti & solid_emp & low_grade
```






    Non-Renter: 57.67% of all loans
    Shorter Term: 97.63% of all loans
    Few Recent Accounts Opened: 46.08% of all loans
    Low Debt-to-Income Ratio: 62.89% of all loans
    Solidly Employed: 51.27% of all loans
    Low Grade: 95.75% of all loans
    
    Full Strategy: 9.60% of all loans
    
    OUT_Class
    	Strategy Mean: 0.925 
    	Random Mean: 0.8645
    
    OUT_Principle_Repaid_Percentage
    	Strategy Mean: 0.9593 
    	Random Mean: 0.922
    
    OUT_Monthly_Rate_Of_Return
    	Strategy Mean: 0.002064 
    	Random Mean: -0.001205
    	Strategy Median: 0.005432 
    	Random Median: 0.006085
    


The strategy performs well across all of the outcome metrics. 

- The investor can expect 92.5% of loans to be fully repaid, versus 86.5% from investing randomly, a gain of 6%.
- The investor can expect 95.9% of principle to be repaid, versus 92.3% from investing randomly, a gain of 3%.
- The investor can expect a mean monthly return of 0.20% and median monthly return of 0.61%
