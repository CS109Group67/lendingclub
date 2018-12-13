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





In the [Modeling](https://cs109group67.github.io/lendingclub/Modeling.html) section we preformed a variety of classifiction and regression models on the three outcome features. A summary of the model scoring metrics is as follows:






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

**Top 10 Key Variables**:





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


With the key variables established, we examine decision tree and random forest analysis on this key subset to establish the investment strategy. We focus on the most comprehensive outcome feature of `OUT_Monthly_Rate_Of_Return` since this feature takes into account the total amount repeiad with interest for the effective term of the loan. We visualize both a larger tree (for completeness and demonstrative purposes) and a simplified tree. 






**FULL Key Feaure Decision Tree**:


    DecisionTreeRegressor(criterion='mse', max_depth=10, max_features=None,
               max_leaf_nodes=None, min_impurity_decrease=0.0,
               min_impurity_split=None, min_samples_leaf=1,
               min_samples_split=0.05, min_weight_fraction_leaf=0.0,
               presort=False, random_state=0, splitter='best')



![svg](Strategy_files/Strategy_24_2.svg)


    







**SIMPLIFIED Key Feature Decision Tree**:


    DecisionTreeRegressor(criterion='mse', max_depth=10, max_features=None,
               max_leaf_nodes=10, min_impurity_decrease=0.0,
               min_impurity_split=None, min_samples_leaf=1,
               min_samples_split=0.05, min_weight_fraction_leaf=0.0,
               presort=False, random_state=0, splitter='best')



![svg](Strategy_files/Strategy_25_2.svg)


    







**Key Feature Random Forest**:


    RandomForestRegressor(bootstrap=True, criterion='mse', max_depth=None,
               max_features='auto', max_leaf_nodes=None,
               min_impurity_decrease=0.0, min_impurity_split=None,
               min_samples_leaf=1, min_samples_split=2,
               min_weight_fraction_leaf=0.0, n_estimators=10, n_jobs=None,
               oob_score=False, random_state=None, verbose=0, warm_start=False)



![png](Strategy_files/Strategy_26_2.png)


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



```python
strategy = non_renters & short_term & few_recent_accounts & low_dti & solid_emp & low_grade
```




```python
ls[strategy]
```





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>D_home_ownership_MORTGAGE</th>
      <th>D_home_ownership_OTHER</th>
      <th>D_home_ownership_OWN</th>
      <th>D_home_ownership_RENT</th>
      <th>D_purpose_car</th>
      <th>D_purpose_credit_card</th>
      <th>D_purpose_debt_consolidation</th>
      <th>D_purpose_home_improvement</th>
      <th>D_purpose_house</th>
      <th>D_purpose_major_purchase</th>
      <th>D_purpose_medical</th>
      <th>D_purpose_moving</th>
      <th>D_purpose_other</th>
      <th>D_purpose_renewable_energy</th>
      <th>D_purpose_small_business</th>
      <th>D_purpose_vacation</th>
      <th>D_purpose_wedding</th>
      <th>D_term_ 36 months</th>
      <th>D_term_ 60 months</th>
      <th>D_verification_status_Not Verified</th>
      <th>D_verification_status_Source Verified</th>
      <th>D_verification_status_Verified</th>
      <th>OUT_Class</th>
      <th>OUT_Monthly_Rate_Of_Return</th>
      <th>OUT_Principle_Repaid_Percentage</th>
      <th>...</th>
      <th>num_op_rev_tl</th>
      <th>num_rev_accts</th>
      <th>num_rev_tl_bal_gt_0</th>
      <th>num_sats</th>
      <th>num_tl_120dpd_2m</th>
      <th>num_tl_30dpd</th>
      <th>num_tl_90g_dpd_24m</th>
      <th>num_tl_op_past_12m</th>
      <th>open_acc</th>
      <th>pct_tl_nvr_dlq</th>
      <th>percent_bc_gt_75</th>
      <th>pub_rec</th>
      <th>pub_rec_bankruptcies</th>
      <th>revol_bal</th>
      <th>revol_util</th>
      <th>sub_grade</th>
      <th>tax_liens</th>
      <th>tot_coll_amt</th>
      <th>tot_cur_bal</th>
      <th>tot_hi_cred_lim</th>
      <th>total_acc</th>
      <th>total_bal_ex_mort</th>
      <th>total_bc_limit</th>
      <th>total_il_high_credit_limit</th>
      <th>total_rev_hi_lim</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1005326</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>10.000</td>
      <td>12.000</td>
      <td>5.000</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>12.000</td>
      <td>96.000</td>
      <td>42.900</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>12689.000</td>
      <td>41.300</td>
      <td>2.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>358595.000</td>
      <td>393700.000</td>
      <td>25.000</td>
      <td>37191.000</td>
      <td>29900.000</td>
      <td>28000.000</td>
      <td>30700.000</td>
    </tr>
    <tr>
      <th>1006038</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>6.000</td>
      <td>10.000</td>
      <td>5.000</td>
      <td>13.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>13.000</td>
      <td>100.000</td>
      <td>75.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18243.000</td>
      <td>81.400</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>205734.000</td>
      <td>295241.000</td>
      <td>19.000</td>
      <td>131423.000</td>
      <td>20200.000</td>
      <td>112591.000</td>
      <td>22400.000</td>
    </tr>
    <tr>
      <th>1006206</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>12.000</td>
      <td>19.000</td>
      <td>2.000</td>
      <td>14.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>14.000</td>
      <td>96.900</td>
      <td>20.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>7513.000</td>
      <td>14.400</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>130182.000</td>
      <td>197661.000</td>
      <td>32.000</td>
      <td>23537.000</td>
      <td>31100.000</td>
      <td>19900.000</td>
      <td>52100.000</td>
    </tr>
    <tr>
      <th>1006460</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>12.000</td>
      <td>17.000</td>
      <td>9.000</td>
      <td>15.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>15.000</td>
      <td>100.000</td>
      <td>20.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>42061.000</td>
      <td>41.200</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>197609.000</td>
      <td>266546.000</td>
      <td>26.000</td>
      <td>53191.000</td>
      <td>93800.000</td>
      <td>18292.000</td>
      <td>102000.000</td>
    </tr>
    <tr>
      <th>1006519</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.008</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>14.000</td>
      <td>4.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>100.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18648.000</td>
      <td>80.700</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>2530.000</td>
      <td>25962.000</td>
      <td>58904.000</td>
      <td>19.000</td>
      <td>25962.000</td>
      <td>14500.000</td>
      <td>35804.000</td>
      <td>23100.000</td>
    </tr>
    <tr>
      <th>1006562</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>9.000</td>
      <td>4.000</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>9.000</td>
      <td>93.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>12633.000</td>
      <td>35.800</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>436447.000</td>
      <td>478162.000</td>
      <td>15.000</td>
      <td>61086.000</td>
      <td>29300.000</td>
      <td>39027.000</td>
      <td>35300.000</td>
    </tr>
    <tr>
      <th>1006680</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.006</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>7.000</td>
      <td>4.000</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2.000</td>
      <td>10.000</td>
      <td>50.000</td>
      <td>33.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>4134.000</td>
      <td>14.400</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>10989.000</td>
      <td>53577.000</td>
      <td>14.000</td>
      <td>10989.000</td>
      <td>28100.000</td>
      <td>24877.000</td>
      <td>28700.000</td>
    </tr>
    <tr>
      <th>1006835</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>8.000</td>
      <td>12.000</td>
      <td>8.000</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11.000</td>
      <td>86.400</td>
      <td>33.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>38116.000</td>
      <td>52.900</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>381561.000</td>
      <td>482818.000</td>
      <td>22.000</td>
      <td>42654.000</td>
      <td>67500.000</td>
      <td>0.000</td>
      <td>72100.000</td>
    </tr>
    <tr>
      <th>1006953</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>16.000</td>
      <td>4.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>96.400</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>16259.000</td>
      <td>46.600</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>162442.000</td>
      <td>229008.000</td>
      <td>28.000</td>
      <td>42922.000</td>
      <td>9500.000</td>
      <td>63108.000</td>
      <td>34900.000</td>
    </tr>
    <tr>
      <th>1007061</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>4.000</td>
      <td>6.000</td>
      <td>4.000</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>6.000</td>
      <td>100.000</td>
      <td>25.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18867.000</td>
      <td>68.100</td>
      <td>2.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>186620.000</td>
      <td>208191.000</td>
      <td>11.000</td>
      <td>27976.000</td>
      <td>27700.000</td>
      <td>13571.000</td>
      <td>27700.000</td>
    </tr>
    <tr>
      <th>1007125</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.008</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>3.000</td>
      <td>2.000</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>5.000</td>
      <td>100.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>7582.000</td>
      <td>82.400</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>68614.000</td>
      <td>91473.000</td>
      <td>8.000</td>
      <td>15246.000</td>
      <td>1700.000</td>
      <td>18376.000</td>
      <td>9200.000</td>
    </tr>
    <tr>
      <th>1007128</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>25.000</td>
      <td>4.000</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>9.000</td>
      <td>83.800</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>4906.000</td>
      <td>13.800</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18576.000</td>
      <td>86793.000</td>
      <td>37.000</td>
      <td>18576.000</td>
      <td>27200.000</td>
      <td>51293.000</td>
      <td>35500.000</td>
    </tr>
    <tr>
      <th>1007149</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.006</td>
      <td>1.000</td>
      <td>...</td>
      <td>2.000</td>
      <td>5.000</td>
      <td>2.000</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>4.000</td>
      <td>90.900</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>10125.000</td>
      <td>84.400</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>96388.000</td>
      <td>150000.000</td>
      <td>11.000</td>
      <td>13604.000</td>
      <td>12000.000</td>
      <td>10000.000</td>
      <td>12000.000</td>
    </tr>
    <tr>
      <th>1007357</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.062</td>
      <td>0.257</td>
      <td>...</td>
      <td>5.000</td>
      <td>6.000</td>
      <td>5.000</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>6.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>29512.000</td>
      <td>55.900</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>142710.000</td>
      <td>190800.000</td>
      <td>7.000</td>
      <td>29512.000</td>
      <td>32800.000</td>
      <td>0.000</td>
      <td>52800.000</td>
    </tr>
    <tr>
      <th>1007361</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>8.000</td>
      <td>8.000</td>
      <td>7.000</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>12.000</td>
      <td>95.500</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11592.000</td>
      <td>61.000</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>116972.000</td>
      <td>164340.000</td>
      <td>22.000</td>
      <td>31835.000</td>
      <td>10200.000</td>
      <td>35250.000</td>
      <td>19000.000</td>
    </tr>
    <tr>
      <th>1007396</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>4.000</td>
      <td>14.000</td>
      <td>3.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>88.200</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>24826.000</td>
      <td>72.000</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>135920.000</td>
      <td>187708.000</td>
      <td>35.000</td>
      <td>57242.000</td>
      <td>17800.000</td>
      <td>64504.000</td>
      <td>34500.000</td>
    </tr>
    <tr>
      <th>1007431</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.009</td>
      <td>1.000</td>
      <td>...</td>
      <td>1.000</td>
      <td>6.000</td>
      <td>1.000</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>3.000</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>9564.000</td>
      <td>73.600</td>
      <td>15.000</td>
      <td>0.000</td>
      <td>61.000</td>
      <td>223127.000</td>
      <td>265748.000</td>
      <td>14.000</td>
      <td>29627.000</td>
      <td>13000.000</td>
      <td>32748.000</td>
      <td>13000.000</td>
    </tr>
    <tr>
      <th>1007463</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>8.000</td>
      <td>19.000</td>
      <td>7.000</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>9.000</td>
      <td>95.000</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>53480.000</td>
      <td>65.100</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>163934.000</td>
      <td>228100.000</td>
      <td>21.000</td>
      <td>53480.000</td>
      <td>68800.000</td>
      <td>0.000</td>
      <td>82100.000</td>
    </tr>
    <tr>
      <th>1007483</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.007</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>9.000</td>
      <td>2.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>100.000</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2816.000</td>
      <td>23.300</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>23254.000</td>
      <td>51969.000</td>
      <td>22.000</td>
      <td>23254.000</td>
      <td>4500.000</td>
      <td>39869.000</td>
      <td>12100.000</td>
    </tr>
    <tr>
      <th>1007597</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>8.000</td>
      <td>10.000</td>
      <td>7.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>100.000</td>
      <td>33.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18621.000</td>
      <td>49.100</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>18621.000</td>
      <td>37900.000</td>
      <td>13.000</td>
      <td>18621.000</td>
      <td>17900.000</td>
      <td>0.000</td>
      <td>37900.000</td>
    </tr>
    <tr>
      <th>1007598</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>11.000</td>
      <td>21.000</td>
      <td>8.000</td>
      <td>14.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>14.000</td>
      <td>100.000</td>
      <td>44.400</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>39708.000</td>
      <td>72.200</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>263304.000</td>
      <td>305518.000</td>
      <td>30.000</td>
      <td>54352.000</td>
      <td>49500.000</td>
      <td>21518.000</td>
      <td>55000.000</td>
    </tr>
    <tr>
      <th>1007654</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>8.000</td>
      <td>1.000</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>5.000</td>
      <td>46.200</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>456.000</td>
      <td>38.000</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>84.000</td>
      <td>37949.000</td>
      <td>45354.000</td>
      <td>13.000</td>
      <td>37949.000</td>
      <td>1100.000</td>
      <td>44154.000</td>
      <td>1200.000</td>
    </tr>
    <tr>
      <th>1007702</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-0.012</td>
      <td>0.611</td>
      <td>...</td>
      <td>8.000</td>
      <td>15.000</td>
      <td>8.000</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>10.000</td>
      <td>92.300</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>7514.000</td>
      <td>50.100</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>179059.000</td>
      <td>203590.000</td>
      <td>26.000</td>
      <td>9259.000</td>
      <td>10800.000</td>
      <td>7800.000</td>
      <td>15000.000</td>
    </tr>
    <tr>
      <th>1007790</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.007</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>7.000</td>
      <td>3.000</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>3.000</td>
      <td>83.300</td>
      <td>33.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>3773.000</td>
      <td>75.500</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>3773.000</td>
      <td>5000.000</td>
      <td>12.000</td>
      <td>3773.000</td>
      <td>5000.000</td>
      <td>0.000</td>
      <td>5000.000</td>
    </tr>
    <tr>
      <th>1007799</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.007</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>12.000</td>
      <td>5.000</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11.000</td>
      <td>100.000</td>
      <td>33.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>14728.000</td>
      <td>34.000</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>604.000</td>
      <td>175670.000</td>
      <td>220667.000</td>
      <td>28.000</td>
      <td>41670.000</td>
      <td>20600.000</td>
      <td>29657.000</td>
      <td>43300.000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1842598</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.007</td>
      <td>1.000</td>
      <td>...</td>
      <td>4.000</td>
      <td>22.000</td>
      <td>3.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>8.000</td>
      <td>56.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>5462.000</td>
      <td>73.000</td>
      <td>14.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>93025.000</td>
      <td>119556.000</td>
      <td>36.000</td>
      <td>93025.000</td>
      <td>5500.000</td>
      <td>112056.000</td>
      <td>7500.000</td>
    </tr>
    <tr>
      <th>1842606</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>-0.099</td>
      <td>0.112</td>
      <td>...</td>
      <td>3.000</td>
      <td>12.000</td>
      <td>3.000</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>1.000</td>
      <td>4.000</td>
      <td>66.700</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1369.000</td>
      <td>12.400</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8175.000</td>
      <td>18499.000</td>
      <td>21.000</td>
      <td>8175.000</td>
      <td>1000.000</td>
      <td>7499.000</td>
      <td>11000.000</td>
    </tr>
    <tr>
      <th>1842611</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.011</td>
      <td>1.000</td>
      <td>...</td>
      <td>13.000</td>
      <td>25.000</td>
      <td>7.000</td>
      <td>14.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>14.000</td>
      <td>100.000</td>
      <td>14.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>29003.000</td>
      <td>42.300</td>
      <td>15.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>229705.000</td>
      <td>279189.000</td>
      <td>28.000</td>
      <td>29003.000</td>
      <td>63800.000</td>
      <td>0.000</td>
      <td>68600.000</td>
    </tr>
    <tr>
      <th>1842644</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>6.000</td>
      <td>8.000</td>
      <td>3.000</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>7.000</td>
      <td>100.000</td>
      <td>33.300</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>4250.000</td>
      <td>48.900</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>553.000</td>
      <td>19604.000</td>
      <td>32900.000</td>
      <td>9.000</td>
      <td>4250.000</td>
      <td>5400.000</td>
      <td>0.000</td>
      <td>8700.000</td>
    </tr>
    <tr>
      <th>1842655</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.010</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>14.000</td>
      <td>4.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>94.700</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>4834.000</td>
      <td>19.500</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>224547.000</td>
      <td>261800.000</td>
      <td>19.000</td>
      <td>4834.000</td>
      <td>24800.000</td>
      <td>0.000</td>
      <td>24800.000</td>
    </tr>
    <tr>
      <th>1842661</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>10.000</td>
      <td>23.000</td>
      <td>3.000</td>
      <td>19.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>19.000</td>
      <td>95.200</td>
      <td>50.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>24164.000</td>
      <td>40.400</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>244558.000</td>
      <td>331112.000</td>
      <td>46.000</td>
      <td>37311.000</td>
      <td>23300.000</td>
      <td>41312.000</td>
      <td>59800.000</td>
    </tr>
    <tr>
      <th>1842668</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>8.000</td>
      <td>6.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11393.000</td>
      <td>43.500</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>15514.000</td>
      <td>31400.000</td>
      <td>11.000</td>
      <td>15514.000</td>
      <td>22100.000</td>
      <td>5200.000</td>
      <td>26200.000</td>
    </tr>
    <tr>
      <th>1842680</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>-0.100</td>
      <td>0.145</td>
      <td>...</td>
      <td>7.000</td>
      <td>11.000</td>
      <td>6.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>100.000</td>
      <td>60.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>27534.000</td>
      <td>55.300</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>104.000</td>
      <td>130608.000</td>
      <td>156800.000</td>
      <td>18.000</td>
      <td>27534.000</td>
      <td>38700.000</td>
      <td>0.000</td>
      <td>49800.000</td>
    </tr>
    <tr>
      <th>1842691</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.006</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>7.000</td>
      <td>3.000</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>4.000</td>
      <td>92.900</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>30189.000</td>
      <td>95.800</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>172758.000</td>
      <td>187500.000</td>
      <td>14.000</td>
      <td>30189.000</td>
      <td>31500.000</td>
      <td>0.000</td>
      <td>31500.000</td>
    </tr>
    <tr>
      <th>1842696</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>14.000</td>
      <td>4.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>90.000</td>
      <td>66.700</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>10335.000</td>
      <td>39.000</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>214098.000</td>
      <td>242515.000</td>
      <td>20.000</td>
      <td>10335.000</td>
      <td>12000.000</td>
      <td>0.000</td>
      <td>26500.000</td>
    </tr>
    <tr>
      <th>1842703</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>8.000</td>
      <td>13.000</td>
      <td>3.000</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>11.000</td>
      <td>95.000</td>
      <td>66.700</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>32375.000</td>
      <td>60.600</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>521954.000</td>
      <td>610100.000</td>
      <td>21.000</td>
      <td>79520.000</td>
      <td>39200.000</td>
      <td>56700.000</td>
      <td>53400.000</td>
    </tr>
    <tr>
      <th>1842714</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.009</td>
      <td>1.000</td>
      <td>...</td>
      <td>10.000</td>
      <td>22.000</td>
      <td>6.000</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11.000</td>
      <td>96.000</td>
      <td>16.700</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>21029.000</td>
      <td>59.200</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>150302.000</td>
      <td>173410.000</td>
      <td>25.000</td>
      <td>21029.000</td>
      <td>32100.000</td>
      <td>0.000</td>
      <td>35500.000</td>
    </tr>
    <tr>
      <th>1842715</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.009</td>
      <td>1.000</td>
      <td>...</td>
      <td>13.000</td>
      <td>40.000</td>
      <td>7.000</td>
      <td>14.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>14.000</td>
      <td>96.000</td>
      <td>40.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>9499.000</td>
      <td>59.000</td>
      <td>9.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>12720.000</td>
      <td>26609.000</td>
      <td>50.000</td>
      <td>12720.000</td>
      <td>9300.000</td>
      <td>10509.000</td>
      <td>16100.000</td>
    </tr>
    <tr>
      <th>1842723</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.008</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>13.000</td>
      <td>6.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>8.000</td>
      <td>94.700</td>
      <td>83.300</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>13308.000</td>
      <td>80.200</td>
      <td>18.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>217760.000</td>
      <td>224472.000</td>
      <td>23.000</td>
      <td>13308.000</td>
      <td>12600.000</td>
      <td>0.000</td>
      <td>16600.000</td>
    </tr>
    <tr>
      <th>1842732</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.006</td>
      <td>1.000</td>
      <td>...</td>
      <td>13.000</td>
      <td>16.000</td>
      <td>10.000</td>
      <td>13.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>13.000</td>
      <td>100.000</td>
      <td>33.300</td>
      <td>2.000</td>
      <td>0.000</td>
      <td>26854.000</td>
      <td>46.200</td>
      <td>10.000</td>
      <td>2.000</td>
      <td>0.000</td>
      <td>26854.000</td>
      <td>58100.000</td>
      <td>20.000</td>
      <td>26854.000</td>
      <td>13500.000</td>
      <td>0.000</td>
      <td>58100.000</td>
    </tr>
    <tr>
      <th>1842739</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>10.000</td>
      <td>3.000</td>
      <td>6.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>6.000</td>
      <td>69.200</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>4742.000</td>
      <td>60.800</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>205405.000</td>
      <td>221349.000</td>
      <td>13.000</td>
      <td>4742.000</td>
      <td>6700.000</td>
      <td>0.000</td>
      <td>7800.000</td>
    </tr>
    <tr>
      <th>1842766</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>6.000</td>
      <td>4.000</td>
      <td>13.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>13.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>393848.000</td>
      <td>82.000</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1046184.000</td>
      <td>1349203.000</td>
      <td>29.000</td>
      <td>91118.000</td>
      <td>77000.000</td>
      <td>119682.000</td>
      <td>479500.000</td>
    </tr>
    <tr>
      <th>1842769</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0.002</td>
      <td>0.836</td>
      <td>...</td>
      <td>5.000</td>
      <td>8.000</td>
      <td>4.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>64.300</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2066.000</td>
      <td>23.000</td>
      <td>10.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>56302.000</td>
      <td>72317.000</td>
      <td>14.000</td>
      <td>56302.000</td>
      <td>7500.000</td>
      <td>63317.000</td>
      <td>9000.000</td>
    </tr>
    <tr>
      <th>1842770</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>15.000</td>
      <td>40.000</td>
      <td>9.000</td>
      <td>20.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>20.000</td>
      <td>100.000</td>
      <td>28.600</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>15371.000</td>
      <td>23.800</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>378399.000</td>
      <td>441564.000</td>
      <td>58.000</td>
      <td>29009.000</td>
      <td>39500.000</td>
      <td>24564.000</td>
      <td>64500.000</td>
    </tr>
    <tr>
      <th>1842786</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.003</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>5.000</td>
      <td>2.000</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>2.000</td>
      <td>5.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>31063.000</td>
      <td>47.000</td>
      <td>3.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>328915.000</td>
      <td>358164.000</td>
      <td>14.000</td>
      <td>57251.000</td>
      <td>47200.000</td>
      <td>30164.000</td>
      <td>53200.000</td>
    </tr>
    <tr>
      <th>1842800</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.007</td>
      <td>1.000</td>
      <td>...</td>
      <td>10.000</td>
      <td>18.000</td>
      <td>6.000</td>
      <td>12.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>12.000</td>
      <td>82.100</td>
      <td>80.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>46922.000</td>
      <td>89.900</td>
      <td>15.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>211469.000</td>
      <td>253575.000</td>
      <td>28.000</td>
      <td>64052.000</td>
      <td>50400.000</td>
      <td>25375.000</td>
      <td>52200.000</td>
    </tr>
    <tr>
      <th>1842823</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.005</td>
      <td>1.000</td>
      <td>...</td>
      <td>6.000</td>
      <td>20.000</td>
      <td>6.000</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>7.000</td>
      <td>78.600</td>
      <td>66.700</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>11000.000</td>
      <td>85.300</td>
      <td>7.000</td>
      <td>0.000</td>
      <td>6532.000</td>
      <td>215836.000</td>
      <td>237900.000</td>
      <td>28.000</td>
      <td>11000.000</td>
      <td>3500.000</td>
      <td>0.000</td>
      <td>12900.000</td>
    </tr>
    <tr>
      <th>1842827</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>5.000</td>
      <td>16.000</td>
      <td>4.000</td>
      <td>11.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>11.000</td>
      <td>100.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>20937.000</td>
      <td>57.500</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>574985.000</td>
      <td>675532.000</td>
      <td>39.000</td>
      <td>96287.000</td>
      <td>36400.000</td>
      <td>109434.000</td>
      <td>36400.000</td>
    </tr>
    <tr>
      <th>1842828</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0.004</td>
      <td>1.000</td>
      <td>...</td>
      <td>3.000</td>
      <td>15.000</td>
      <td>3.000</td>
      <td>5.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>5.000</td>
      <td>63.600</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>37670.000</td>
      <td>71.400</td>
      <td>4.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>428773.000</td>
      <td>508514.000</td>
      <td>33.000</td>
      <td>78159.000</td>
      <td>4300.000</td>
      <td>48964.000</td>
      <td>41300.000</td>
    </tr>
    <tr>
      <th>1842858</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0.014</td>
      <td>1.000</td>
      <td>...</td>
      <td>7.000</td>
      <td>19.000</td>
      <td>6.000</td>
      <td>8.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>8.000</td>
      <td>90.500</td>
      <td>42.900</td>
      <td>2.000</td>
      <td>1.000</td>
      <td>6122.000</td>
      <td>15.200</td>
      <td>17.000</td>
      <td>1.000</td>
      <td>259.000</td>
      <td>13021.000</td>
      <td>60865.000</td>
      <td>26.000</td>
      <td>13021.000</td>
      <td>40200.000</td>
      <td>20665.000</td>
      <td>40200.000</td>
    </tr>
  </tbody>
</table>
<p>33059 rows × 85 columns</p>
</div>





```python
ls_strategy = ls[strategy]
```




```python
ls_strategy.to_hdf(directory + 'ls_STRATEGY.h5', 'ls_STRATEGY')
```

