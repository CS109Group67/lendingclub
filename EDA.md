---
title: EDA & Cleaning
notebook: EDA.ipynb
nav_include: 2
---

## Contents
{:.no_toc}
*  
{: toc}






























## 1. Term-Complete Subset

'Term-complete' loans have completed their full term whereas 'in-force' loans have not completed their term.  To get the most representative outcome information we first remove the loan instances that are not term-complete.



```python
#TERM COMPLETE LOANS
completed_36 = (ls['issue_d'] < '2015-04-01') & (ls['term']  == ' 36 months')
completed_60 = (ls['issue_d'] < '2013-04-01') & (ls['term']  == ' 60 months')
ls = ls[completed_36 | completed_60]
```


## 2. Inconsequential Variable Removal

We removed variables from the `loanstats` dataset that would not be meaningful in the modeling for the following reasons:
1. LC recently began reporting some new varaibles which are not reported in the term-complete subset. 
2. LC recently began accepting joint loans and variables related to coborrowers are empty in the term-complete subset. 
3. We designed three outcome features based on the set of dependent variables (see next section)
4. Some variables are non-standard text descriptions or otherwise inconsequential



```python
#1 NEW
empty = ['all_util', 'il_util', 'inq_fi', 'inq_last_12m', 'max_bal_bc', 
         'mths_since_rcnt_il', 'open_acc_6m', 'open_act_il', 'open_il_12m', 
         'open_il_24m', 'open_rv_12m', 'open_rv_24m','total_bal_il', 'total_cu_tl']

#2 CO-BORROWER
joint = ['application_type', 'annual_inc_joint', 'dti_joint', 'revol_bal_joint', 
         'sec_app_chargeoff_within_12_mths', 'sec_app_collections_12_mths_ex_med', 
         'sec_app_earliest_cr_line', 'sec_app_inq_last_6mths', 'sec_app_mort_acc', 
         'sec_app_mths_since_last_major_derog', 'sec_app_num_rev_accts', 'sec_app_open_acc', 
         'sec_app_open_act_il', 'sec_app_revol_util', 'verification_status_joint']

#3 DEPENDENT
dependent = [# Payment Variables (11): 
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
              'settlement_status', 'settlement_term', 'total_rec_late_fee']

#4 OTHER INCONSEQUENTIAL
inconsequential = ['addr_state', # not useful as dummy variable
                   'dataset', # just indicates the dataset
                   'desc', # non-standard text description
                   'disbursement_method', # just indicates cash or direct_pay
                   'emp_title', # non-standard text description
                   'funded_amnt', # redundant with loan_amount
                   'funded_amnt_inv', # redundant with loan_amount
                   'grade', # redundant when using sub_grade
                   'initial_list_status', # possible values are w or f
                   'title', # non-standard text description
                   'zip_code'] # we could make into dummies, but there are 954 of them
```






## 3. Outcome Feature Design

We designed 3 outcome features to represent how well or poorly the loan performed: `OUT_Class`, `OUT_Principle_Repaid_Percentage` and `OUT_Monthly_Rate_of_Return`.

### 3A. `OUT_Class`

This outcome variable is a binary classification of whether the loan has been Fully Repaid (1) or Not Fully Repaid (0). The percentage of loans that have been fully repaid is 85.9%.



```python
ls['OUT_Class'] = 0
ls.loc[ls['loan_status'].str.contains('Fully Paid'), 'OUT_Class'] = 1
```






    	Type: 			int64
    	Missing Values: 	0 (0.0%)
    	Mean: 			0.86
    	Range: 			(0.00, 1.00)



![png](EDA_files/EDA_18_1.png)







### 3B. `OUT_Principle_Repaid_Percentage`

This outcome variable represents the percentage of loan principle that has been repaid. The average principal repaid percentage is 91.5%.



```python
ls['OUT_Principle_Repaid_Percentage'] = ls['total_rec_prncp'] / ls['loan_amnt']
```






    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			0.92
    	Range: 			(0.00, 1.00)



![png](EDA_files/EDA_21_1.png)







### 3C. `OUT_Monthly_Rate_of_Return`

This outcome variable represents the simple monthly rate of return that investors recieved by holding the loan. This is the most comprehensive of the three outcome features because it takes into account the total amount repaid (including interest) for the effective term of the loan. The median monthly rate of return is 0.6%.



```python
#NET_REPAYMENT: amount repaid on the loan net of the loan amount
Net_Repayment = ls['total_pymnt'] - ls['loan_amnt']

#REPAYMENT_PERIOD: amount of time it took to repay the loan or charge off
Repayment_Period = (ls['last_pymnt_d'].dt.to_period('M') - 
                    ls['issue_d'].dt.to_period('M')).replace([pd.NaT,0], 1)

#MONTHLY_RATE_OF_RETURN: simple monthly return accrued over the term of the loan
ls['OUT_Monthly_Rate_Of_Return'] = (Net_Repayment / Repayment_Period) / ls['loan_amnt']
```






    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			-0.00
    	Range: 			(-1.00, 0.21)



![png](EDA_files/EDA_24_1.png)







## 4. Independent Variables

We cleaned each independent variable with type conversions, dummy creation, outlier identication and missing value imputation.










**acc_now_delinq**: The number of accounts on which the borrower is now delinquent.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.00
    	Range: 			(0.00, 14.00)



![png](EDA_files/EDA_27_2.png)








**acc_open_past_24mths**: Number of trades opened in past 24 months.


    	Type: 			float64
    	Missing Values: 	50030 (11.9%)
    	Mean: 			4.19
    	Range: 			(0.00, 53.00)



![png](EDA_files/EDA_27_6.png)








**annual_inc**: The self-reported annual income provided by the borrower during registration.


    	Type: 			float64
    	Missing Values: 	4 (0.0%)
    	Mean: 			71625.96
    	Range: 			(1896.00, 8706582.00)



![png](EDA_files/EDA_27_10.png)








**avg_cur_bal**: Average current balance of all accounts


    	Type: 			float64
    	Missing Values: 	70285 (16.7%)
    	Mean: 			12685.60
    	Range: 			(0.00, 958084.00)



![png](EDA_files/EDA_27_14.png)








**bc_open_to_buy**: Total open to buy on revolving bankcards.


    	Type: 			float64
    	Missing Values: 	53734 (12.8%)
    	Mean: 			8498.77
    	Range: 			(0.00, 497445.00)



![png](EDA_files/EDA_27_18.png)








**bc_util**: Ratio of total current balance to high credit/credit limit for all bankcard accounts.


    	Type: 			float64
    	Missing Values: 	53975 (12.8%)
    	Mean: 			64.43
    	Range: 			(0.00, 339.60)



![png](EDA_files/EDA_27_22.png)








**chargeoff_within_12_mths**: Number of charge-offs within 12 months


    	Type: 			float64
    	Missing Values: 	145 (0.0%)
    	Mean: 			0.01
    	Range: 			(0.00, 7.00)



![png](EDA_files/EDA_27_26.png)








**collections_12_mths_ex_med**: Number of collections in 12 months excluding medical collections


    	Type: 			float64
    	Missing Values: 	145 (0.0%)
    	Mean: 			0.01
    	Range: 			(0.00, 20.00)



![png](EDA_files/EDA_27_30.png)








**delinq_2yrs**: The number of 30+ days past-due incidences of delinquency in the borrower's credit file for the past 2 years


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.29
    	Range: 			(0.00, 29.00)



![png](EDA_files/EDA_27_34.png)








**delinq_amnt**: The past-due amount owed for the accounts on which the borrower is now delinquent.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			8.69
    	Range: 			(0.00, 86399.00)



![png](EDA_files/EDA_27_38.png)








**dti**: A ratio calculated using the borrower’s total monthly debt payments on the total debt obligations, excluding mortgage and the requested LC loan, divided by the borrower’s self-reported monthly income.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			16.99
    	Range: 			(0.00, 39.99)



![png](EDA_files/EDA_27_42.png)








**earliest_cr_line**: The month the borrower's earliest reported credit line was opened


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			5749.94
    	Range: 			(184.00, 25933.00)



![png](EDA_files/EDA_27_46.png)








**emp_length**: Employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years. 


    	Type: 			float64
    	Missing Values: 	21519 (5.1%)
    	Mean: 			5.84
    	Range: 			(0.00, 10.00)



![png](EDA_files/EDA_27_50.png)








**home_ownership**: The home ownership status provided by the borrower during registration or obtained from the credit report. Our values are: RENT, OWN, MORTGAGE, OTHER


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Dummies: 	4
    	Most Common Category: 	MORTGAGE








**inq_last_6mths**: The number of inquiries in past 6 months (excluding auto and mortgage inquiries)


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.78
    	Range: 			(0.00, 33.00)



![png](EDA_files/EDA_27_57.png)








**installment**: The monthly payment owed by the borrower if the loan originates.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			414.03
    	Range: 			(14.01, 1409.99)



![png](EDA_files/EDA_27_61.png)








**int_rate**: Interest Rate on the loan


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			12.74
    	Range: 			(5.42, 26.06)



![png](EDA_files/EDA_27_65.png)








**loan_amnt**: The listed amount of the loan applied for by the borrower. If at some point in time, the credit department reduces the loan amount, then it will be reflected in this value.


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			12718.50
    	Range: 			(500.00, 35000.00)



![png](EDA_files/EDA_27_69.png)








**mo_sin_old_il_acct**: Months since oldest bank installment account opened


    	Type: 			float64
    	Missing Values: 	83911 (20.0%)
    	Mean: 			124.94
    	Range: 			(0.00, 649.00)



![png](EDA_files/EDA_27_73.png)








**mo_sin_old_rev_tl_op**: Months since oldest revolving account opened


    	Type: 			float64
    	Missing Values: 	70277 (16.7%)
    	Mean: 			180.39
    	Range: 			(3.00, 851.00)



![png](EDA_files/EDA_27_77.png)








**mo_sin_rcnt_rev_tl_op**: Months since most recent revolving account opened


    	Type: 			float64
    	Missing Values: 	70277 (16.7%)
    	Mean: 			13.25
    	Range: 			(0.00, 372.00)



![png](EDA_files/EDA_27_81.png)








**mo_sin_rcnt_tl**: Months since most recent account opened


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			8.37
    	Range: 			(0.00, 226.00)



![png](EDA_files/EDA_27_85.png)








**mort_acc**: Number of mortgage accounts.


    	Type: 			float64
    	Missing Values: 	50030 (11.9%)
    	Mean: 			1.69
    	Range: 			(0.00, 34.00)



![png](EDA_files/EDA_27_89.png)








**mths_since_last_delinq**: The number of months since the borrower's last delinquency.


    	Type: 			float64
    	Missing Values: 	224620 (53.5%)
    	Mean: 			34.30
    	Range: 			(0.00, 188.00)



![png](EDA_files/EDA_27_93.png)








**mths_since_last_major_derog**: Months since most recent 90-day or worse rating


    	Type: 			float64
    	Missing Values: 	329015 (78.3%)
    	Mean: 			42.46
    	Range: 			(0.00, 188.00)



![png](EDA_files/EDA_27_97.png)








**mths_since_last_record**: The number of months since the last public record.


    	Type: 			float64
    	Missing Values: 	360872 (85.9%)
    	Mean: 			72.88
    	Range: 			(0.00, 129.00)



![png](EDA_files/EDA_27_101.png)








**mths_since_recent_bc**: Months since most recent bankcard account opened.


    	Type: 			float64
    	Missing Values: 	53373 (12.7%)
    	Mean: 			24.53
    	Range: 			(0.00, 616.00)



![png](EDA_files/EDA_27_105.png)








**mths_since_recent_bc_dlq**: Months since most recent bankcard delinquency


    	Type: 			float64
    	Missing Values: 	331393 (78.9%)
    	Mean: 			40.30
    	Range: 			(0.00, 176.00)



![png](EDA_files/EDA_27_109.png)








**mths_since_recent_inq**: Months since most recent inquiry.


    	Type: 			float64
    	Missing Values: 	88898 (21.2%)
    	Mean: 			6.95
    	Range: 			(0.00, 25.00)



![png](EDA_files/EDA_27_113.png)








**mths_since_recent_revol_delinq**: Months since most recent revolving delinquency.


    	Type: 			float64
    	Missing Values: 	294683 (70.1%)
    	Mean: 			36.15
    	Range: 			(0.00, 180.00)



![png](EDA_files/EDA_27_117.png)








**num_accts_ever_120_pd**: Number of accounts ever 120 or more days past due


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			0.46
    	Range: 			(0.00, 35.00)



![png](EDA_files/EDA_27_121.png)








**num_actv_bc_tl**: Number of currently active bankcard accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			3.66
    	Range: 			(0.00, 30.00)



![png](EDA_files/EDA_27_125.png)








**num_actv_rev_tl**: Number of currently active revolving trades


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			5.67
    	Range: 			(0.00, 41.00)



![png](EDA_files/EDA_27_129.png)








**num_bc_sats**: Number of satisfactory bankcard accounts


    	Type: 			float64
    	Missing Values: 	58590 (13.9%)
    	Mean: 			4.61
    	Range: 			(0.00, 46.00)



![png](EDA_files/EDA_27_133.png)








**num_bc_tl**: Number of bankcard accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			8.60
    	Range: 			(0.00, 65.00)



![png](EDA_files/EDA_27_137.png)








**num_il_tl**: Number of installment accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			7.98
    	Range: 			(0.00, 150.00)



![png](EDA_files/EDA_27_141.png)








**num_op_rev_tl**: Number of open revolving accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			8.11
    	Range: 			(0.00, 62.00)



![png](EDA_files/EDA_27_145.png)








**num_rev_accts**: Number of revolving accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			14.93
    	Range: 			(0.00, 105.00)



![png](EDA_files/EDA_27_149.png)








**num_rev_tl_bal_gt_0**: Number of revolving trades with balance >0


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			5.65
    	Range: 			(0.00, 38.00)



![png](EDA_files/EDA_27_153.png)








**num_sats**: Number of satisfactory accounts


    	Type: 			float64
    	Missing Values: 	58590 (13.9%)
    	Mean: 			11.22
    	Range: 			(0.00, 84.00)



![png](EDA_files/EDA_27_157.png)








**num_tl_120dpd_2m**: Number of accounts currently 120 days past due (updated in past 2 months)


    	Type: 			float64
    	Missing Values: 	78691 (18.7%)
    	Mean: 			0.00
    	Range: 			(0.00, 3.00)



![png](EDA_files/EDA_27_161.png)








**num_tl_30dpd**: Number of accounts currently 30 days past due (updated in past 2 months)


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			0.00
    	Range: 			(0.00, 4.00)



![png](EDA_files/EDA_27_165.png)








**num_tl_90g_dpd_24m**: Number of accounts 90 or more days past due in last 24 months


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			0.09
    	Range: 			(0.00, 24.00)



![png](EDA_files/EDA_27_169.png)








**num_tl_op_past_12m**: Number of accounts opened in past 12 months


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			1.94
    	Range: 			(0.00, 26.00)



![png](EDA_files/EDA_27_173.png)








**open_acc**: The number of open credit lines in the borrower's credit file.


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			11.01
    	Range: 			(0.00, 84.00)



![png](EDA_files/EDA_27_177.png)








**outlier**: 


    	Type: 			int64
    	Missing Values: 	0 (0.0%)
    	Mean: 			0.00
    	Range: 			(0.00, 0.00)



![png](EDA_files/EDA_27_181.png)








**pct_tl_nvr_dlq**: Percent of trades never delinquent


    	Type: 			float64
    	Missing Values: 	70390 (16.8%)
    	Mean: 			94.44
    	Range: 			(7.70, 100.00)



![png](EDA_files/EDA_27_185.png)








**percent_bc_gt_75**: Percentage of all bankcard accounts > 75% of limit.


    	Type: 			float64
    	Missing Values: 	53858 (12.8%)
    	Mean: 			50.54
    	Range: 			(0.00, 100.00)



![png](EDA_files/EDA_27_189.png)








**pub_rec**: Number of derogatory public records


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			0.17
    	Range: 			(0.00, 63.00)



![png](EDA_files/EDA_27_193.png)








**pub_rec_bankruptcies**: Number of public record bankruptcies


    	Type: 			float64
    	Missing Values: 	1365 (0.3%)
    	Mean: 			0.11
    	Range: 			(0.00, 12.00)



![png](EDA_files/EDA_27_197.png)








**purpose**: A category provided by the borrower for the loan request. 


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Dummies: 	14
    	Most Common Category: 	debt_consolidation








**revol_bal**: Total credit revolving balance


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			15406.16
    	Range: 			(0.00, 2568995.00)



![png](EDA_files/EDA_27_204.png)








**revol_util**: Revolving line utilization rate, or the amount of credit the borrower is using relative to all available revolving credit.


    	Type: 			float64
    	Missing Values: 	296 (0.1%)
    	Mean: 			55.02
    	Range: 			(0.00, 892.30)



![png](EDA_files/EDA_27_208.png)








**sub_grade**: LC assigned loan subgrade


    	Type: 			float64
    	Missing Values: 	0 (0.0%)
    	Mean: 			10.59
    	Range: 			(1.00, 35.00)



![png](EDA_files/EDA_27_212.png)








**tax_liens**: Number of tax liens


    	Type: 			float64
    	Missing Values: 	105 (0.0%)
    	Mean: 			0.04
    	Range: 			(0.00, 63.00)



![png](EDA_files/EDA_27_216.png)








**term**: The number of payments on the loan. Values are in months and can be either 36 or 60.


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Dummies: 	2
    	Most Common Category: 	 36 months








**tot_coll_amt**: Total collection amounts ever owed


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			213.03
    	Range: 			(0.00, 9152545.00)



![png](EDA_files/EDA_27_223.png)








**tot_cur_bal**: Total current balance of all accounts


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			128545.52
    	Range: 			(0.00, 8000078.00)



![png](EDA_files/EDA_27_227.png)








**tot_hi_cred_lim**: Total high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			157552.13
    	Range: 			(0.00, 9999999.00)



![png](EDA_files/EDA_27_231.png)








**total_acc**: The total number of credit lines currently in the borrower's credit file


    	Type: 			float64
    	Missing Values: 	29 (0.0%)
    	Mean: 			24.49
    	Range: 			(1.00, 162.00)



![png](EDA_files/EDA_27_235.png)








**total_bal_ex_mort**: Total credit balance excluding mortgage


    	Type: 			float64
    	Missing Values: 	50030 (11.9%)
    	Mean: 			44002.55
    	Range: 			(0.00, 2688920.00)



![png](EDA_files/EDA_27_239.png)








**total_bc_limit**: Total bankcard high credit/credit limit


    	Type: 			float64
    	Missing Values: 	50030 (11.9%)
    	Mean: 			19602.34
    	Range: 			(0.00, 760000.00)



![png](EDA_files/EDA_27_243.png)








**total_il_high_credit_limit**: Total installment high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			36279.50
    	Range: 			(0.00, 1241783.00)



![png](EDA_files/EDA_27_247.png)








**total_rev_hi_lim**: Total revolving high credit/credit limit


    	Type: 			float64
    	Missing Values: 	70276 (16.7%)
    	Mean: 			29473.47
    	Range: 			(0.00, 9999999.00)



![png](EDA_files/EDA_27_251.png)








**verification_status**: Indicates if income was verified by LC, not verified, or if the income source was verified


    	Type: 			object
    	Missing Values: 	0 (0.0%)
    	Number of Dummies: 	3
    	Most Common Category: 	Not Verified










