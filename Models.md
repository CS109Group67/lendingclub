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
## 1. Data Preparation

Some data preprocessing is needed in order to build and evaluate our models.





### 1A. Outcome Features

3 outcome features were designed in the cleaning section and are already processed for modeling.





    1 OUT_Class
    2 OUT_Monthly_Rate_Of_Return
    3 OUT_Principle_Repaid_Percentage


### 1B. Dummy Variables

23 dummy variables are used to indicate the borrower's state and home ownership, as well as the purpose, term and verification status of the loan. By using one-hot encoding and not discarding one dummy variable, we are implicitly adding an intercept.





    1  D_home_ownership_MORTGAGE
    2  D_home_ownership_OTHER
    3  D_home_ownership_OWN
    4  D_home_ownership_RENT
    5  D_purpose_car
    6  D_purpose_credit_card
    7  D_purpose_debt_consolidation
    8  D_purpose_educational
    9  D_purpose_home_improvement
    10 D_purpose_house
    11 D_purpose_major_purchase
    12 D_purpose_medical
    13 D_purpose_moving
    14 D_purpose_other
    15 D_purpose_renewable_energy
    16 D_purpose_small_business
    17 D_purpose_vacation
    18 D_purpose_wedding
    19 D_term_ 36 months
    20 D_term_ 60 months
    21 D_verification_status_Not Verified
    22 D_verification_status_Source Verified
    23 D_verification_status_Verified


### 1C. Numeric Variables

60 numeric variables represent different loan and borrower characteristics. These variables will be transformed to a standard scale (section 1E) before modeling.





    1  acc_now_delinq
    2  acc_open_past_24mths
    3  annual_inc
    4  avg_cur_bal
    5  bc_open_to_buy
    6  bc_util
    7  chargeoff_within_12_mths
    8  collections_12_mths_ex_med
    9  delinq_2yrs
    10 delinq_amnt
    11 dti
    12 earliest_cr_line
    13 emp_length
    14 inq_last_6mths
    15 installment
    16 int_rate
    17 loan_amnt
    18 mo_sin_old_il_acct
    19 mo_sin_old_rev_tl_op
    20 mo_sin_rcnt_rev_tl_op
    21 mo_sin_rcnt_tl
    22 mort_acc
    23 mths_since_last_delinq
    24 mths_since_last_major_derog
    25 mths_since_last_record
    26 mths_since_recent_bc
    27 mths_since_recent_bc_dlq
    28 mths_since_recent_inq
    29 mths_since_recent_revol_delinq
    30 num_accts_ever_120_pd
    31 num_actv_bc_tl
    32 num_actv_rev_tl
    33 num_bc_sats
    34 num_bc_tl
    35 num_il_tl
    36 num_op_rev_tl
    37 num_rev_accts
    38 num_rev_tl_bal_gt_0
    39 num_sats
    40 num_tl_120dpd_2m
    41 num_tl_30dpd
    42 num_tl_90g_dpd_24m
    43 num_tl_op_past_12m
    44 open_acc
    45 pct_tl_nvr_dlq
    46 percent_bc_gt_75
    47 pub_rec
    48 pub_rec_bankruptcies
    49 revol_bal
    50 revol_util
    51 sub_grade
    52 tax_liens
    53 tot_coll_amt
    54 tot_cur_bal
    55 tot_hi_cred_lim
    56 total_acc
    57 total_bal_ex_mort
    58 total_bc_limit
    59 total_il_high_credit_limit
    60 total_rev_hi_lim


### 1D. Train-Test Split

We split the `ls` dataset into a train and test part. We do this in a stratified fashion ensuring that the outcome classes (fully paid loans and not fully paid loans) are equally represented in each set. For the splitting algorithm, we use `sklearn`'s `train_test_split` function. This function creates random train and test subsets of the dataset. The flag `stratify` ensures that both classes are equally represented in each set. 



```python
from sklearn.model_selection import train_test_split
ls_train, ls_test = train_test_split(ls, test_size=0.2, stratify=ls['OUT_Class'])
```


### 1E. Standard Scaling

Most of the models used later will assume all features are on similar scales. To achieve this, we use standardization to transform the numeric variables such that they have a mean of 0 and standard deviation of 1. We use `sklearn`'s `StandardScaler` function.



```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
```




```python
train_vars_scaled = scaler.fit_transform(ls_train[list(numeric_var_list)])
train_vars_scaled = pd.DataFrame(train_vars_scaled, 
                                 index=ls_train.index, 
                                 columns=ls_train[list(numeric_var_list)].columns)
ls_train_scaled = pd.concat(objs=[train_vars_scaled,
                                  ls_train[list(outcome_var_list)],
                                  ls_train[list(dummy_var_list)]],
                            axis=1).sort_index(axis=1)
```




```python
test_vars_scaled = scaler.transform(ls_test[list(numeric_var_list)])
test_vars_scaled = pd.DataFrame(test_vars_scaled, 
                                index=ls_test.index, 
                                columns=ls_test[list(numeric_var_list)].columns)
ls_test_scaled = pd.concat(objs=[test_vars_scaled,
                                ls_test[list(outcome_var_list)],
                                ls_test[list(dummy_var_list)]],
                           axis=1).sort_index(axis=1)
```


## Classification

## Regression

## Trees and Forest

## K-Nearest Neighbors
