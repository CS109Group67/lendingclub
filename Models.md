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

Some data preprocessing is needed in order to build and evaluate our models.





### 1A. Variable Summary

3 outcome features were designed in the cleaning section and are already processed for modeling.





    1 OUT_Class
    2 OUT_Monthly_Rate_Of_Return
    3 OUT_Principle_Repaid_Percentage


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


### 1B. Train-Test Split

We split the `ls` dataset into a train and test part. We do this in a stratified fashion ensuring that the outcome classes (fully paid loans and not fully paid loans) are equally represented in each set. For the splitting algorithm, we use `sklearn`'s `train_test_split` function. This function creates random train and test subsets of the dataset. The flag `stratify` ensures that both classes are equally represented in each set. 



```python
from sklearn.model_selection import train_test_split
ls_train, ls_test = train_test_split(ls, test_size=0.2, stratify=ls['OUT_Class'])
```


### 1C. Standard Scaling

Most of the models used later will assume all features are on similar scales. To achieve this, we use standardization to transform the numeric variables such that they have a mean of 0 and standard deviation of 1. We use `sklearn`'s `StandardScaler` function.



```python
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


### 1C. Missing Value Imputation

The models we will use require that we handle the missing values in the dataset. 



```python
print('{:<30} {}'.format('VARIABLE', 'MISSING'))
for var in sorted(numeric_var_list):
    if ls[var].isnull().sum() > 0:
        print('{:<30} {:.0%}'.format(var, ls[var].isnull().sum()/len(ls)))
```


    VARIABLE                       MISSING
    acc_now_delinq                 0%
    acc_open_past_24mths           12%
    annual_inc                     0%
    avg_cur_bal                    17%
    bc_open_to_buy                 13%
    bc_util                        13%
    chargeoff_within_12_mths       0%
    collections_12_mths_ex_med     0%
    delinq_2yrs                    0%
    delinq_amnt                    0%
    earliest_cr_line               0%
    emp_length                     5%
    inq_last_6mths                 0%
    mo_sin_old_il_acct             20%
    mo_sin_old_rev_tl_op           17%
    mo_sin_rcnt_rev_tl_op          17%
    mo_sin_rcnt_tl                 17%
    mort_acc                       12%
    mths_since_last_delinq         53%
    mths_since_last_major_derog    78%
    mths_since_last_record         86%
    mths_since_recent_bc           13%
    mths_since_recent_bc_dlq       79%
    mths_since_recent_inq          21%
    mths_since_recent_revol_delinq 70%
    num_accts_ever_120_pd          17%
    num_actv_bc_tl                 17%
    num_actv_rev_tl                17%
    num_bc_sats                    14%
    num_bc_tl                      17%
    num_il_tl                      17%
    num_op_rev_tl                  17%
    num_rev_accts                  17%
    num_rev_tl_bal_gt_0            17%
    num_sats                       14%
    num_tl_120dpd_2m               19%
    num_tl_30dpd                   17%
    num_tl_90g_dpd_24m             17%
    num_tl_op_past_12m             17%
    open_acc                       0%
    pct_tl_nvr_dlq                 17%
    percent_bc_gt_75               13%
    pub_rec                        0%
    pub_rec_bankruptcies           0%
    revol_util                     0%
    tax_liens                      0%
    tot_coll_amt                   17%
    tot_cur_bal                    17%
    tot_hi_cred_lim                17%
    total_acc                      0%
    total_bal_ex_mort              12%
    total_bc_limit                 12%
    total_il_high_credit_limit     17%
    total_rev_hi_lim               17%




```python
len(outcome_test)
```





    84036



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
        745                     ensure_min_features=ensure_min_features,
        746                     warn_on_dtype=warn_on_dtype,
    --> 747                     estimator=estimator)
        748     if multi_output:
        749         y = check_array(y, 'csr', force_all_finite=True, ensure_2d=False,


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/validation.py in check_array(array, accept_sparse, accept_large_sparse, dtype, order, copy, force_all_finite, ensure_2d, allow_nd, ensure_min_samples, ensure_min_features, warn_on_dtype, estimator)
        566         if force_all_finite:
        567             _assert_all_finite(array,
    --> 568                                allow_nan=force_all_finite == 'allow-nan')
        569 
        570     shape_repr = _shape_repr(array.shape)


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/validation.py in _assert_all_finite(X, allow_nan)
         54                 not allow_nan and not np.isfinite(X).all()):
         55             type_err = 'infinity' if allow_nan else 'NaN, infinity'
    ---> 56             raise ValueError(msg_err.format(type_err, X.dtype))
         57 
         58 


    ValueError: Input contains NaN, infinity or a value too large for dtype('float64').




```python

```





    826096    1
    861888    1
    353431    0
    837420    0
    383519    1
    191341    1
    414683    1
    4479      1
    242956    1
    201355    1
    343151    1
    30033     0
    860846    1
    394910    1
    138958    1
    117233    1
    422825    1
    836949    1
    35194     1
    346527    1
    273750    1
    75694     1
    14420     1
    103244    1
    203790    1
    320556    1
    32205     1
    225221    1
    204021    1
    354487    1
    250947    1
    425511    1
    46002     1
    34269     1
    403491    0
    456816    1
    838911    1
    91856     1
    209269    1
    124605    1
    164613    1
    828790    1
    5910      1
    378715    1
    220358    1
    462001    1
    100556    1
    101363    1
    45130     0
    38938     1
             ..
    396924    1
    175172    1
    863921    1
    825545    1
    110803    1
    23545     0
    404221    1
    102236    1
    85349     1
    97780     1
    147664    0
    10037     1
    11146     1
    827718    0
    412779    0
    26403     1
    23375     1
    129976    1
    384366    1
    258843    1
    199846    1
    37901     1
    220980    1
    283969    1
    335810    1
    19326     1
    158762    1
    297378    1
    198052    0
    111007    1
    819836    1
    273201    0
    302344    1
    847887    1
    156814    1
    256614    0
    881227    1
    104929    1
    116962    1
    375098    1
    283529    1
    111082    0
    212314    1
    125986    1
    33552     1
    330145    1
    419592    1
    848041    1
    197836    0
    821625    1
    Name: OUT_Class, Length: 84036, dtype: int64



## Regression

## Trees and Forest

## K-Nearest Neighbors
