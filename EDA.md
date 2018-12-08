---
title:  EDA & Cleaning
notebook: EDA.ipynb
nav_include: 2
---

## Contents
{:.no_toc}
*  
{: toc}


<br>

## 0. Custom Functions



















```python
#FUNCTION FOR EDA
num_observations = len(ls_clean)
def EDA_attr(attr):
    """ Displays basic EDA for given attribute"""
    attr_type = ls_clean[attr].dtype
    missing_values = ls_clean[attr].isnull().sum()
    display(Markdown('**{}**: {}'.format(attr, data_dict.get(attr, "NULL"))))
    print('\tType: \t\t\t{}'.format(attr_type))
    print('\tMissing Values: \t{} ({:.1%})'.format(
                    missing_values, missing_values/num_observations))    
    
    if attr_type == 'float64':  # numerical variables
        print('\tMean: \t\t\t{:.2f}'.format(ls_clean[attr].mean()))
        print('\tRange: \t\t\t({:.2f}, {:.2f})'.format(ls_clean[attr].min(), ls_clean[attr].max()))
        plt.hist(ls_clean[attr]); plt.ylabel('Number of Loans'); plt.xlabel(attr); plt.show()
    
    if attr_type == 'object':   # categorical variables
        print('\tNumber of Categories: \t{}'.format(len(ls_clean.groupby(attr))))
        print('\tMost Common Category: \t{}'.format(ls_clean.groupby(attr)['loan_amnt'].count().idxmax()))
    
    display(Markdown('\n'))
```




```python
#FUNCTION FOR DUMMY CREATION
def dummy_attr(attr):
    """ Creates dummy variables and drops original attribute"""
    global ls_clean
    if attr not in list(ls_clean): return
    prefix = 'D_' + attr
    dummies = pd.get_dummies(ls_clean[attr], prefix=prefix)
    ls_clean.drop([attr], axis=1, inplace=True)
    ls_clean = pd.concat([ls_clean, dummies], axis=1)
```




```python
#FUNCTION FOR SCALING
scaler_dict = {} # dictionary to store scalers, to be used for inverse transforms
scaler_list = [] # list to store variables to be scaled
def scale_attr(attr, fit_data=None, scaler=None):
    """ Scales attribute with StandardScaler (default) or MinMaxScaler"""
    scaler_list.append(attr)
    # if fit_data is None:
    #     fit_data = ls_clean[[attr]]
    # if scaler is None:
    #     scaler = StandardScaler()
    # scaler = scaler.fit(fit_data)
    # ls_clean[attr] = scaler.transform(ls_clean[[attr]])
    # scaler_dict[attr] = scaler
```




```python
#FUNCTION FOR OUTLIER DETECTION
ls_clean['outlier'] = 0 # this column is incremented for identified outlier instances
def outlier_attr(attr, threshold):
    """ Identifies outliers above threshold and updates outlier indictor""" 
    outliers = ls[attr] > threshold
    ls_clean['outlier'] = ls_clean['outlier'] + outliers
    return outliers
```


<br>

## 1. Inconsequential Variables

Our focus will be on loans that have completed their terms. This subset of **'term-complete'** loans provides the fully representative outcome information since current in-force loans can still default. Therefore, we remove the loan instances that are not term-complete:



```python
#DROP TERM INCOMPLETE LOANS
completed_36 = (ls['issue_d'] < '2015-04-01') & (ls['term']  == ' 36 months')
completed_60 = (ls['issue_d'] < '2013-04-01') & (ls['term']  == ' 60 months')
ls_clean = ls_clean[completed_36 | completed_60]
```


Next we drop non-existant, empty, constant or otherwise unmeaningful variables.



```python
#DROP INCONSEQUENTIAL VARIABLES
drop = ['addr_state', # not useful as dummy variable
        'all_util', # all missing values
        'dataset', # just indicates the dataset
        'desc', # non-standard text description
        'disbursement_method', # just indicates cash or direct_pay
        'emp_title', # non-standard text description
        'funded_amnt', # redundant with loan_amount
        'funded_amnt_inv', # redundant with loan_amount
        'grade', # redundant when using sub_grade
        'initial_list_status', # possible values are w or f
        'il_util', # all missing values
        'inq_fi', # all missing values
        'inq_last_12m', # all missing values
        'max_bal_bc', # all missing values
        'mths_since_rcnt_il', # all missing values
        'open_acc_6m', # all missing values
        'open_act_il', # all missing values
        'open_il_12m', # all missing values
        'open_il_24m', # all missing values
        'open_rv_12m', # all missing values
        'open_rv_24m', # all missing values
        'title', # non-standard text description
        'total_bal_il', # all missing values
        'total_cu_tl', # all missing values
        'zip_code'] # we could make into dummies, but there are 954 of them
ls_clean.drop(drop, axis=1, inplace=True)
```


LC only recently began accepting joint application loans, so none of these loans are term-complete. Therefore we remove these variables from the model.



```python
#DROP CO-BORROWER VARIABLES
joint = ['application_type', 'annual_inc_joint', 'dti_joint', 'revol_bal_joint', 
         'sec_app_chargeoff_within_12_mths', 'sec_app_collections_12_mths_ex_med', 
         'sec_app_earliest_cr_line', 'sec_app_inq_last_6mths', 'sec_app_mort_acc', 
         'sec_app_mths_since_last_major_derog', 'sec_app_num_rev_accts', 'sec_app_open_acc', 
         'sec_app_open_act_il', 'sec_app_revol_util', 'verification_status_joint']
ls_clean.drop(joint, axis=1, inplace=True)
```


<br>

## 2. Independent Variables

We performed type conversions, outlier identification, dummy creation and EDA for each independent variable.

<br>






**acc_now_delinq**: The number of accounts on which the borrower is now delinquent.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.00
    	Range: 			(0.00, 14.00)



![png](EDA_files/EDA_18_2.png)












**acc_open_past_24mths**: Number of trades opened in past 24 months.


    	Type: 			float64
    	Missing Values: 	50030 (2.5%)
    	Mean: 			4.19
    	Range: 			(0.00, 53.00)



![png](EDA_files/EDA_19_2.png)












**emp_length**: Employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years. 


    	Type: 			float64
    	Missing Values: 	21519 (1.1%)
    	Mean: 			5.84
    	Range: 			(0.00, 10.00)



![png](EDA_files/EDA_20_2.png)












**annual_inc**: The self-reported annual income provided by the borrower during registration.


    	Type: 			float64
    	Missing Values: 	4 (0.0%)
    	Mean: 			71625.96
    	Range: 			(1896.00, 8706582.00)



![png](EDA_files/EDA_21_2.png)












**avg_cur_bal**: Average current balance of all accounts


    	Type: 			float64
    	Missing Values: 	70285 (3.5%)
    	Mean: 			12685.60
    	Range: 			(0.00, 958084.00)



![png](EDA_files/EDA_22_2.png)












**bc_open_to_buy**: Total open to buy on revolving bankcards.


    	Type: 			float64
    	Missing Values: 	53734 (2.7%)
    	Mean: 			8498.77
    	Range: 			(0.00, 497445.00)



![png](EDA_files/EDA_23_2.png)












**bc_util**: Ratio of total current balance to high credit/credit limit for all bankcard accounts.


    	Type: 			float64
    	Missing Values: 	53975 (2.7%)
    	Mean: 			64.43
    	Range: 			(0.00, 339.60)



![png](EDA_files/EDA_24_2.png)












**chargeoff_within_12_mths**: Number of charge-offs within 12 months


    	Type: 			float64
    	Missing Values: 	145 (0.0%)
    	Mean: 			0.01
    	Range: 			(0.00, 7.00)



![png](EDA_files/EDA_25_2.png)












**collections_12_mths_ex_med**: Number of collections in 12 months excluding medical collections


    	Type: 			float64
    	Missing Values: 	145 (0.0%)
    	Mean: 			0.01
    	Range: 			(0.00, 20.00)



![png](EDA_files/EDA_26_2.png)












**delinq_2yrs**: The number of 30+ days past-due incidences of delinquency in the borrower's credit file for the past 2 years


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.29
    	Range: 			(0.00, 29.00)



![png](EDA_files/EDA_27_2.png)












**delinq_amnt**: The past-due amount owed for the accounts on which the borrower is now delinquent.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			8.69
    	Range: 			(0.00, 86399.00)



![png](EDA_files/EDA_28_2.png)












**dti**: A ratio calculated using the borrower’s total monthly debt payments on the total debt obligations, excluding mortgage and the requested LC loan, divided by the borrower’s self-reported monthly income.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			16.99
    	Range: 			(0.00, 39.99)



![png](EDA_files/EDA_29_2.png)












**earliest_cr_line**: The month the borrower's earliest reported credit line was opened


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			5749.94
    	Range: 			(184.00, 25933.00)



![png](EDA_files/EDA_30_2.png)












**emp_length**: Employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years. 


    	Type: 			float64
    	Missing Values: 	21519 (1.1%)
    	Mean: 			5.84
    	Range: 			(0.00, 10.00)



![png](EDA_files/EDA_31_2.png)












**home_ownership**: The home ownership status provided by the borrower during registration or obtained from the credit report. Our values are: RENT, OWN, MORTGAGE, OTHER


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Categories: 	4
    	Most Common Category: 	MORTGAGE












**inq_last_6mths**: The number of inquiries in past 6 months (excluding auto and mortgage inquiries)


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.78
    	Range: 			(0.00, 33.00)



![png](EDA_files/EDA_33_2.png)












**installment**: The monthly payment owed by the borrower if the loan originates.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			414.03
    	Range: 			(14.01, 1409.99)



![png](EDA_files/EDA_34_2.png)












**int_rate**: Interest Rate on the loan


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			12.74
    	Range: 			(5.42, 26.06)



![png](EDA_files/EDA_35_2.png)












**loan_amnt**: The listed amount of the loan applied for by the borrower. If at some point in time, the credit department reduces the loan amount, then it will be reflected in this value.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			12718.50
    	Range: 			(500.00, 35000.00)



![png](EDA_files/EDA_36_2.png)












**mo_sin_old_il_acct**: Months since oldest bank installment account opened


    	Type: 			float64
    	Missing Values: 	83911 (4.2%)
    	Mean: 			124.94
    	Range: 			(0.00, 649.00)



![png](EDA_files/EDA_37_2.png)












**mo_sin_old_rev_tl_op**: Months since oldest revolving account opened


    	Type: 			float64
    	Missing Values: 	70277 (3.5%)
    	Mean: 			180.39
    	Range: 			(3.00, 851.00)



![png](EDA_files/EDA_38_2.png)












**mo_sin_rcnt_rev_tl_op**: Months since most recent revolving account opened


    	Type: 			float64
    	Missing Values: 	70277 (3.5%)
    	Mean: 			13.25
    	Range: 			(0.00, 372.00)



![png](EDA_files/EDA_39_2.png)












**mo_sin_rcnt_tl**: Months since most recent account opened


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			8.37
    	Range: 			(0.00, 226.00)



![png](EDA_files/EDA_40_2.png)












**mort_acc**: Number of mortgage accounts.


    	Type: 			float64
    	Missing Values: 	50030 (2.5%)
    	Mean: 			1.69
    	Range: 			(0.00, 34.00)



![png](EDA_files/EDA_41_2.png)












**mths_since_last_delinq**: The number of months since the borrower's last delinquency.


    	Type: 			float64
    	Missing Values: 	224620 (11.2%)
    	Mean: 			34.30
    	Range: 			(0.00, 188.00)



![png](EDA_files/EDA_42_2.png)












**mths_since_last_major_derog**: Months since most recent 90-day or worse rating


    	Type: 			float64
    	Missing Values: 	329015 (16.4%)
    	Mean: 			42.46
    	Range: 			(0.00, 188.00)



![png](EDA_files/EDA_43_2.png)












**mths_since_last_record**: The number of months since the last public record.


    	Type: 			float64
    	Missing Values: 	360872 (18.0%)
    	Mean: 			72.88
    	Range: 			(0.00, 129.00)



![png](EDA_files/EDA_44_2.png)












**mths_since_recent_bc**: Months since most recent bankcard account opened.


    	Type: 			float64
    	Missing Values: 	53373 (2.7%)
    	Mean: 			24.53
    	Range: 			(0.00, 616.00)



![png](EDA_files/EDA_45_2.png)












**mths_since_recent_bc_dlq**: Months since most recent bankcard delinquency


    	Type: 			float64
    	Missing Values: 	331393 (16.5%)
    	Mean: 			40.30
    	Range: 			(0.00, 176.00)



![png](EDA_files/EDA_46_2.png)












**mths_since_recent_inq**: Months since most recent inquiry.


    	Type: 			float64
    	Missing Values: 	88898 (4.4%)
    	Mean: 			6.95
    	Range: 			(0.00, 25.00)



![png](EDA_files/EDA_47_2.png)












**mths_since_recent_revol_delinq**: Months since most recent revolving delinquency.


    	Type: 			float64
    	Missing Values: 	294683 (14.7%)
    	Mean: 			36.15
    	Range: 			(0.00, 180.00)



![png](EDA_files/EDA_48_2.png)












**num_accts_ever_120_pd**: Number of accounts ever 120 or more days past due


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			0.46
    	Range: 			(0.00, 35.00)



![png](EDA_files/EDA_49_2.png)












**num_actv_bc_tl**: Number of currently active bankcard accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			3.66
    	Range: 			(0.00, 30.00)



![png](EDA_files/EDA_50_2.png)












**num_actv_rev_tl**: Number of currently active revolving trades


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			5.67
    	Range: 			(0.00, 41.00)



![png](EDA_files/EDA_51_2.png)












**num_bc_sats**: Number of satisfactory bankcard accounts


    	Type: 			float64
    	Missing Values: 	58590 (2.9%)
    	Mean: 			4.61
    	Range: 			(0.00, 46.00)



![png](EDA_files/EDA_52_2.png)












**num_bc_tl**: Number of bankcard accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			8.60
    	Range: 			(0.00, 65.00)



![png](EDA_files/EDA_53_2.png)












**num_il_tl**: Number of installment accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			7.98
    	Range: 			(0.00, 150.00)



![png](EDA_files/EDA_54_2.png)












**num_op_rev_tl**: Number of open revolving accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			8.11
    	Range: 			(0.00, 62.00)



![png](EDA_files/EDA_55_2.png)












**num_rev_accts**: Number of revolving accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			14.93
    	Range: 			(0.00, 105.00)



![png](EDA_files/EDA_56_2.png)












**num_rev_tl_bal_gt_0**: Number of revolving trades with balance >0


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			5.65
    	Range: 			(0.00, 38.00)



![png](EDA_files/EDA_57_2.png)












**num_sats**: Number of satisfactory accounts


    	Type: 			float64
    	Missing Values: 	58590 (2.9%)
    	Mean: 			11.22
    	Range: 			(0.00, 84.00)



![png](EDA_files/EDA_58_2.png)












**num_tl_120dpd_2m**: Number of accounts currently 120 days past due (updated in past 2 months)


    	Type: 			float64
    	Missing Values: 	78691 (3.9%)
    	Mean: 			0.00
    	Range: 			(0.00, 3.00)



![png](EDA_files/EDA_59_2.png)












**num_tl_30dpd**: Number of accounts currently 30 days past due (updated in past 2 months)


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			0.00
    	Range: 			(0.00, 4.00)



![png](EDA_files/EDA_60_2.png)












**num_tl_90g_dpd_24m**: Number of accounts 90 or more days past due in last 24 months


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			0.09
    	Range: 			(0.00, 24.00)



![png](EDA_files/EDA_61_2.png)












**num_tl_op_past_12m**: Number of accounts opened in past 12 months


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			1.94
    	Range: 			(0.00, 26.00)



![png](EDA_files/EDA_62_2.png)












**open_acc**: The number of open credit lines in the borrower's credit file.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			11.01
    	Range: 			(0.00, 84.00)



![png](EDA_files/EDA_63_2.png)












**pct_tl_nvr_dlq**: Percent of trades never delinquent


    	Type: 			float64
    	Missing Values: 	70390 (3.5%)
    	Mean: 			94.44
    	Range: 			(7.70, 100.00)



![png](EDA_files/EDA_64_2.png)












**percent_bc_gt_75**: Percentage of all bankcard accounts > 75% of limit.


    	Type: 			float64
    	Missing Values: 	53858 (2.7%)
    	Mean: 			50.54
    	Range: 			(0.00, 100.00)



![png](EDA_files/EDA_65_2.png)












**pub_rec**: Number of derogatory public records


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.17
    	Range: 			(0.00, 63.00)



![png](EDA_files/EDA_66_2.png)












**pub_rec_bankruptcies**: Number of public record bankruptcies


    	Type: 			float64
    	Missing Values: 	1365 (0.1%)
    	Mean: 			0.11
    	Range: 			(0.00, 12.00)



![png](EDA_files/EDA_67_2.png)












**purpose**: A category provided by the borrower for the loan request. 


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Categories: 	14
    	Most Common Category: 	debt_consolidation












**revol_bal**: Total credit revolving balance


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			15406.16
    	Range: 			(0.00, 2568995.00)



![png](EDA_files/EDA_69_2.png)












**revol_util**: Revolving line utilization rate, or the amount of credit the borrower is using relative to all available revolving credit.


    	Type: 			object
    	Missing Values: 	296 (0.0%)
    	Number of Categories: 	1262
    	Most Common Category: 	0%












**sub_grade**: LC assigned loan subgrade


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			10.59
    	Range: 			(1.00, 35.00)



![png](EDA_files/EDA_71_2.png)












**tax_liens**: Number of tax liens


    	Type: 			float64
    	Missing Values: 	105 (0.0%)
    	Mean: 			0.04
    	Range: 			(0.00, 63.00)



![png](EDA_files/EDA_72_2.png)












**term**: The number of payments on the loan. Values are in months and can be either 36 or 60.


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Categories: 	2
    	Most Common Category: 	 36 months












**tot_coll_amt**: Total collection amounts ever owed


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			213.03
    	Range: 			(0.00, 9152545.00)



![png](EDA_files/EDA_74_2.png)












**tot_cur_bal**: Total current balance of all accounts


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			128545.52
    	Range: 			(0.00, 8000078.00)



![png](EDA_files/EDA_75_2.png)












**tot_hi_cred_lim**: Total high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			157552.13
    	Range: 			(0.00, 9999999.00)



![png](EDA_files/EDA_76_2.png)












**total_acc**: The total number of credit lines currently in the borrower's credit file


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			24.49
    	Range: 			(1.00, 162.00)



![png](EDA_files/EDA_77_2.png)












**total_bal_ex_mort**: Total credit balance excluding mortgage


    	Type: 			float64
    	Missing Values: 	50030 (2.5%)
    	Mean: 			44002.55
    	Range: 			(0.00, 2688920.00)



![png](EDA_files/EDA_78_2.png)












**total_bc_limit**: Total bankcard high credit/credit limit


    	Type: 			float64
    	Missing Values: 	50030 (2.5%)
    	Mean: 			19602.34
    	Range: 			(0.00, 760000.00)



![png](EDA_files/EDA_79_2.png)












**total_il_high_credit_limit**: Total installment high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			36279.50
    	Range: 			(0.00, 1241783.00)



![png](EDA_files/EDA_80_2.png)












**total_rev_hi_lim**: Total revolving high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (3.5%)
    	Mean: 			29473.47
    	Range: 			(0.00, 9999999.00)



![png](EDA_files/EDA_81_2.png)












**verification_status**: Indicates if income was verified by LC, not verified, or if the income source was verified


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Categories: 	3
    	Most Common Category: 	Not Verified







<br>

## 3. Dependent Variables

The following variables represent outcome information for the loan after it has been funded. This information is not be available to a prospective investor but instead represents aspects of how well or poorly the loan performed after issuance.



```python
#DEPENDENT VARIABLES
dependent_cols = [
    
    # Payment Variables (11): 
    'issue_d', 'last_pymnt_amnt', 'last_pymnt_d', 'loan_status', 
    'next_pymnt_d', 'out_prncp', 'out_prncp_inv', 'total_pymnt', 
    'total_pymnt_inv', 'total_rec_int', 'total_rec_prncp', 
    
    # Hardship/Collections/Settlements (27)
    'collection_recovery_fee', 'debt_settlement_flag', 'debt_settlement_flag_date', 
    'deferral_term', 'hardship_amount', 'hardship_dpd', 'hardship_end_date', 
    'hardship_flag', 'hardship_last_payment_amount','hardship_length', 'hardship_loan_status', 
    'hardship_payoff_balance_amount', 'hardship_reason', 'hardship_start_date', 
    'hardship_status', 'hardship_type', 'last_credit_pull_d', 
    'orig_projected_additional_accrued_interest', 'payment_plan_start_date', 'pymnt_plan', 
    'recoveries', 'settlement_amount', 'settlement_date', 'settlement_percentage', 
    'settlement_status', 'settlement_term', 'total_rec_late_fee', ]

ls_clean.drop(dependent_cols, axis=1, inplace=True)
```


We designed 3 features to represent loan outcomes:
- A. `OUT_Class`: outcome classification of Fully Repaid (1) vs. Not Fully Repaid (0)
- B. `OUT_Prncp_Repaid_Percentage`: percentage of principal repaid
- C. `OUT_Monthly_Rate_of_Return`: simple monthly rate of return

### 3A. `OUT_Class`

This outcome variable is an indicator whether the loan has been fully repaid (1) or charged off/defaulted (0). Note that 85.9% percent of all loans have been fully repaid.



```python
ls_clean['OUT_Class'] = 0
ls_clean.loc[ls['loan_status'].str.contains('Fully Paid'), 'OUT_Class'] = 1
ls_clean.loc[ls['loan_status'].str.contains('Current'), 'OUT_Class'] = 1
ls_clean['OUT_Class'].describe()
```





    count   420181.000
    mean         0.859
    std          0.348
    min          0.000
    25%          1.000
    50%          1.000
    75%          1.000
    max          1.000
    Name: OUT_Class, dtype: float64



### 3B. `OUT_Principle_Repaid_Percentage`

This outcome variable represents the percentage of loan principal that has been repaid. Note that the average principal repaid percentage is 91.5%.



```python
ls_clean['OUT_Principle_Repaid_Percentage'] = ls['total_rec_prncp'] / ls['loan_amnt']
ls_clean['OUT_Principle_Repaid_Percentage'].describe()
```





    count   420181.000
    mean         0.915
    std          0.226
    min          0.000
    25%          1.000
    50%          1.000
    75%          1.000
    max          1.000
    Name: OUT_Principle_Repaid_Percentage, dtype: float64



### 3C. `OUT_Monthly_Rate_of_Return`

This outcome variable represents the monthly rate of return that investors have achieved by holding the loan. This is the most comprehensive of our outcome features because it takes into account the total amount repaid (including interest) for the effective term of the loan.



```python
#Net_Repayment: amount repaid on the loan net of the loan amount
Net_Repayment = ls['total_pymnt'] - ls['loan_amnt']

#Repayment_Period: amount of time it took to repay the loan or charge off
Repayment_Period = (ls['last_pymnt_d'].dt.to_period('M') - 
                    ls['issue_d'].dt.to_period('M')).replace([pd.NaT,0], 1)

#Monthly_Rate_Of_Return: simple monthly return accrued over the term of the loan
ls_clean['OUT_Monthly_Rate_Of_Return'] = (Net_Repayment / Repayment_Period) / ls_clean['loan_amnt']
ls_clean['OUT_Monthly_Rate_Of_Return'].describe()
```





    count   420181.000
    mean        -0.002
    std          0.052
    min         -1.000
    25%          0.004
    50%          0.006
    75%          0.008
    max          0.208
    Name: OUT_Monthly_Rate_Of_Return, dtype: float64






