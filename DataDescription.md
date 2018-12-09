---
title:  Data Description
notebook: DataDescription.ipynb
nav_include: 1
---

## Contents
{:.no_toc}
*  
{: toc}






<br>

## 1. LoanStats

Lending Club publishes funded loan data (`LoanStats`) which are updated each quarter. We accessed the data as of the 2018Q2 update. These quarterly datasets were merged to create a DataFrame which initially contained 145 features for 2,004,063 approved loans.








![png](DataDescription_files/DataDescription_4_0.png)



The loan issue dates range from June 2007 to June 2018. The loan terms are 36 months or 60 months, which means **'term-complete' loans** are only those issued prior to June 2013 for 60-month terms and prior to June 2015 for 36-month terms. The number and notional of LC loan issuances over time is visualized below.








![png](DataDescription_files/DataDescription_6_0.png)



After processing the `loanstats` data (see [EDA & Cleaning](https://cs109group67.github.io/lendingclub/EDA.html)) the dataset contains 3 outcome features, 23 dummy variables and  60 numeric variables. The full list of variables is summarized here.





    OUTCOME FEATURES
    1 OUT_Class
    2 OUT_Monthly_Rate_Of_Return
    3 OUT_Principle_Repaid_Percentage
    
    DUMMY VARIABLES
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
    
    NUMERIC VARIABLES
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


<br>

## 2. RejectStats

Rejected loan datasets (`RejectStats`) were merged to create a DataFrame with 9 features of 22,469,074 loan applications that failed to meet LC underwriting policy








![png](DataDescription_files/DataDescription_11_0.png)



<br>

## 3. Census Data

To understand whether there is discrimination in LC’s practices and ultimately our proposed investing strategy, we obtained the U.S. Census Bureau’s demographic data and leveraged zip codes to tie the demographic information to the loan data. This data reports the total population and population by different demographic groups (i.e., race, income, education, sex, household structure) and other characteristics like poverty and unemployment rate by zip code. Because LC data is reported at the 3-digit zip code level, we aggregated up to the 3-digit zip code level and calculated percentage of total population and percentage of total household values. We also derived some additional variables such as household size (from population and number of households) and occupied rate (from number of households and housing units). We dropped the variables related to age, as we felt that discrimination related to age is appropriate because it is highly correlated with length of credit history, a valid criterion on which loan applications should be evaluated. Post processing, our census dataset had 891 3-digit zip codes and 68 features.

There are five records with missing data, likely originating from sparsely populated zip code areas. At least one 3-digit zip code area has a recorded population of zero. On average, the population is ~350k, with a household size of ~2.6 and a median income of $55k. For the average zip code area, the population is about evenly split between male and female, with the majority of the population White and having at least a High School education.
