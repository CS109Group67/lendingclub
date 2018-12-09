---
title: Ethical Implications
notebook: EthicalImplications.ipynb
nav_include: 5
---

## Contents
{:.no_toc}
*  
{: toc}


## Lending and Discrimination

The [Equal Credit Opportunity Act (ECOA)](https://www.consumer.ftc.gov/articles/0347-your-equal-credit-opportunity-rights) is a federal law that prohibits lending entities (both institutions and people) from discriminating "on the basis of race, color, religion, national origin, sex, marital status, age," or because the prospective borrower receives public assistance. Lenders are, however, permitted to consider an applicant's "income, expenses, debts, and credit history" in evaluating the applicant's probability of repaying the debt to decide whether to accept or reject loan applications and to determine loan terms. Nevertheless, differential treatment by race, color, religion, national origin, sex, etc can still arise even when an institution or person is not explicitly discriminating based on those characteristics. Thus, we want to assess whether we see evidence of discrimination in LendingClub's acceptances and rejections of loan applications or in the terms it sets for accepted loans. In addition, we want to evaluate whether our proposed investing strategy results in differential treatment.

## Literature Review

To inform our approach, we surveyed existing research of how predictive algorithms can result in discrimination and methods for addressing it.

#### Sweeney, L. "Discrimination in Online Ad Delivery". Communications of the ACM, May 2013, Vol. 56 No. 5, Pages 44-54.

Sweeney's research presents evidence of discrimination in ad delivery on Google.com and Reuters.com on the basis of racially associated names. Using a chi-squared analysis, Sweeney found that running a search on a black-associated name was 25% more likely to get arrested-related ads compared to running a search on a white-associated name. All names used in the study were the full names of real people, roughly evenly split between professionals and "netizens" (i.e. people active on the internet - social media and blogs), and about one-third black and two-thirds white.

While disentangling the root cause of the discrimination fell outside the scope of the study, Sweeney's research clearly showcases the need to actively evaluate possibly unintended societal implications of the algorithms we put in place.

#### Datta, A., Tschantz, M.C., and Datta, A. "Automated Experiments on Ad Privacy Settings: A Tale of Opacity, Choice, and Discrimination". Proceedings on Privacy Enhancing Technologies 2015; 2015 (1):92–112.

Datta et al. found evidence of gender discimination in employment-related searches by experimentally manipulating whether a simulated user specified male or female in Google's ad settings. Their results revealed that simulated male users were shown ads regarding coaching for high-paying careers more often than they were shown to simulated female users. They collected the ads results of 1000 simulated users - half male and half female - and trained a classifier predicting gender using the ad URLs and titles as the feature set. Their classifier used a 90-10 train-test split, and achieved 93% accuracy on the test set. The high predictive power of the classifier suggests gender discrimination in Google's employment-related ad results, and such discrimination can exacerbate the current gender pay gap.

With limited visibility into the details of Google's ad vending algorithm, the authors of this paper were also unable to isolate the mechanisms through which the discrimination is arising. But again, like Sweeney's research, it demonstrates algorithms, while not inherently subjective, can be discriminatory as well.

#### Angwin, J., Larson, J., Mattu, S., and Kirchner, L. "Machine Bias". ProPublica. May 23, 2016.

In an article published on ProPublica, the authors found that COMPAS (Correctional Offender Management Profiling for Alternative Sanctions), one of the nation's most popular tools in criminal risk assessment, discriminates unfavorably toward black defendants but favorably toward white defendants. Although the prediction accuracy was about the same as the overall accuracy (61%) in both groups, COMPAS makes a systematically different kind of error depending on the defendant's race group. Specifically, black defendants were twice as likely as black defendants to fall victim to a false positive result, i.e. they did not reoffend within the next two years but were wrongly labelled as highly likely to reoffend (i.e. high risk). On the other hand, white defendants were twice as likely as black defendants to receive a false negative assessment, i.e. they did reoffend within the next two years but were incorrectly labelled as not very likely to reoffend (i.e. low risk). Using a logistic regression model, Angwin et al. found that bias against black defendants in COMPAS's predictions (higher risk scores) remains even after controlling for age, criminal history, future (actual) recidivism, charge degree, and gender.

## Census Data Description and Cleaning

Especially when machine-generated predictions carry significant life consequences, developers and scientists have a responsibility to ensure their algorithms do not create or exacerbate societal problems through disparate outcomes and impacts on different groups.

While LendingClub does not have the demographic data associated with each loan or loan application (either they don't collect it or they just don't make it publicly available), LendingClub does share the 3-digit zip codes of the borrowers and applicants. To understand whether there is discrimination in LendingClub’s practices and ultimately our proposed investing strategy, we obtained the U.S. Census Bureau’s demographic data and leveraged these zip codes to tie high-level demographic information to the loan data. This data reports the total population and population by different demographic groups (i.e., race, income, education, sex, household structure) and other characteristics like poverty and unemployment rates by zip code. Because LendingClub data is reported at the 3-digit zip code level, we aggregated up to the 3-digit zip code level and calculated percentage of total population and percentage of total household values. We also derived some additional variables such as household size (from population and number of households) and occupied rate (from number of households and housing units). We dropped the variables related to age, as we felt that discrimination related to age is appropriate because it is highly correlated with length of credit history, a valid criterion on which loan applications should be evaluated but is not available in the LendingClub data. Post processing, our census dataset had 891 3-digit zip codes and 68 features.

There are five records with missing data, likely originating from sparsely populated zip code areas. At least one 3-digit zip code area has a recorded population of zero. On average, the population is ~350k, with a household size of ~2.6 and a median income of $55k. For the average zip code area, the population is about evenly split between male and female, with the majority of the population White and having at least a High School education. (See **Table 1** below.)



```python
import requests
from IPython.core.display import HTML
styles = requests.get("https://raw.githubusercontent.com/Harvard-IACS/2018-CS109A/master/content/styles/cs109.css").text
HTML(styles)

%matplotlib inline
import pandas as pd
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns
```




```python
census_df = pd.read_csv("data/census_data_clean_new.csv", index_col=False)
```


#### Table 1. Selected Summary Statistics for Census Data



```python
pd.set_option('float_format', '{:f}'.format)
census_df[['Population', 'Household_size', 'Avg_median_household_inc', 'Male_pct', 'White_pct', 'No_Diploma_pct']].describe()
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
      <th>Population</th>
      <th>Household_size</th>
      <th>Avg_median_household_inc</th>
      <th>Male_pct</th>
      <th>White_pct</th>
      <th>No_Diploma_pct</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>891.000000</td>
      <td>886.000000</td>
      <td>886.000000</td>
      <td>886.000000</td>
      <td>886.000000</td>
      <td>885.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>357514.425365</td>
      <td>2.657810</td>
      <td>54876.389742</td>
      <td>49.533976</td>
      <td>71.255064</td>
      <td>12.865526</td>
    </tr>
    <tr>
      <th>std</th>
      <td>391271.361292</td>
      <td>0.306602</td>
      <td>16378.655265</td>
      <td>1.806755</td>
      <td>20.720257</td>
      <td>5.511241</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.960550</td>
      <td>10039.113879</td>
      <td>42.618155</td>
      <td>2.420546</td>
      <td>0.873224</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>116037.500000</td>
      <td>2.493136</td>
      <td>44655.485670</td>
      <td>48.773422</td>
      <td>58.467006</td>
      <td>8.968413</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>217838.000000</td>
      <td>2.611310</td>
      <td>51368.147218</td>
      <td>49.374504</td>
      <td>75.983965</td>
      <td>11.838107</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>451807.000000</td>
      <td>2.761605</td>
      <td>61534.292170</td>
      <td>50.041174</td>
      <td>89.283953</td>
      <td>15.580997</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3105203.000000</td>
      <td>8.104651</td>
      <td>184414.729328</td>
      <td>88.256228</td>
      <td>97.704745</td>
      <td>38.063859</td>
    </tr>
  </tbody>
</table>
</div>



## Exploratory Data Analysis

Leveraging zip codes, we explored whether there were demographic differences between loans that LendingClub accepted or rejected for its marketplace.



```python
loan_stats_df = pd.read_hdf("data/LoanStats_clean.h5")
```




```python
loan_cols_to_keep = ['loan_amnt', 'funded_amnt', 'funded_amnt_inv', 'term', 'int_rate', 'installment', 
                'grade', 'sub_grade', 'emp_length', 'home_ownership', 'annual_inc', 'verification_status', 
                'zip_code', 'dti']
accepted_df = loan_stats_df[loan_cols_to_keep]
accepted_df['int_rate'] = accepted_df['int_rate'].apply(lambda x: float(x[:-1]))
accepted_df['accepted'] = 1
accepted_df.head()
```


    /anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    /anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys





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
      <th>loan_amnt</th>
      <th>funded_amnt</th>
      <th>funded_amnt_inv</th>
      <th>term</th>
      <th>int_rate</th>
      <th>installment</th>
      <th>grade</th>
      <th>sub_grade</th>
      <th>emp_length</th>
      <th>home_ownership</th>
      <th>annual_inc</th>
      <th>verification_status</th>
      <th>zip_code</th>
      <th>dti</th>
      <th>accepted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5000.0</td>
      <td>5000.0</td>
      <td>4975.0</td>
      <td>36 months</td>
      <td>10.65</td>
      <td>162.87</td>
      <td>B</td>
      <td>B2</td>
      <td>10+ years</td>
      <td>RENT</td>
      <td>24000.0</td>
      <td>Verified</td>
      <td>860xx</td>
      <td>27.65</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2500.0</td>
      <td>2500.0</td>
      <td>2500.0</td>
      <td>60 months</td>
      <td>15.27</td>
      <td>59.83</td>
      <td>C</td>
      <td>C4</td>
      <td>&lt; 1 year</td>
      <td>RENT</td>
      <td>30000.0</td>
      <td>Source Verified</td>
      <td>309xx</td>
      <td>1.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2400.0</td>
      <td>2400.0</td>
      <td>2400.0</td>
      <td>36 months</td>
      <td>15.96</td>
      <td>84.33</td>
      <td>C</td>
      <td>C5</td>
      <td>10+ years</td>
      <td>RENT</td>
      <td>12252.0</td>
      <td>Not Verified</td>
      <td>606xx</td>
      <td>8.72</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10000.0</td>
      <td>10000.0</td>
      <td>10000.0</td>
      <td>36 months</td>
      <td>13.49</td>
      <td>339.31</td>
      <td>C</td>
      <td>C1</td>
      <td>10+ years</td>
      <td>RENT</td>
      <td>49200.0</td>
      <td>Source Verified</td>
      <td>917xx</td>
      <td>20.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3000.0</td>
      <td>3000.0</td>
      <td>3000.0</td>
      <td>60 months</td>
      <td>12.69</td>
      <td>67.79</td>
      <td>B</td>
      <td>B5</td>
      <td>1 year</td>
      <td>RENT</td>
      <td>80000.0</td>
      <td>Source Verified</td>
      <td>972xx</td>
      <td>17.94</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>





```python
reject_stats_df = pd.read_pickle("data/RejectStats_clean.pkl")
```




```python
reject_cols_to_keep = ['Amount Requested', 'Risk_Score', 'Debt-To-Income Ratio', 'Zip Code', 'Employment Length']
rejected_df = reject_stats_df[reject_cols_to_keep]
rejected_df = rejected_df.rename(index=str, columns={"Amount Requested": "loan_amnt", 
                                                             "Risk_Score": "risk_score", 
                                                             "Debt-To-Income Ratio": "dti", 
                                                             "Zip Code": "zip_code", 
                                                             "Employment Length": "emp_length"})
rejected_df['dti'] = rejected_df['dti'].apply(lambda x: float(x[:-1]))
rejected_df['accepted'] = 0
rejected_df.head()
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
      <th>loan_amnt</th>
      <th>risk_score</th>
      <th>dti</th>
      <th>zip_code</th>
      <th>emp_length</th>
      <th>accepted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000.000000</td>
      <td>693.000000</td>
      <td>10.000000</td>
      <td>481xx</td>
      <td>4 years</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000.000000</td>
      <td>703.000000</td>
      <td>10.000000</td>
      <td>010xx</td>
      <td>&lt; 1 year</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11000.000000</td>
      <td>715.000000</td>
      <td>10.000000</td>
      <td>212xx</td>
      <td>1 year</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6000.000000</td>
      <td>698.000000</td>
      <td>38.640000</td>
      <td>017xx</td>
      <td>&lt; 1 year</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1500.000000</td>
      <td>509.000000</td>
      <td>9.430000</td>
      <td>209xx</td>
      <td>&lt; 1 year</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>





```python
rand_ind = np.random.choice(rejected_df.shape[0], accepted_df.shape[0], replace=False)
sample_rejected_df = rejected_df.iloc[rand_ind]
```




```python
common_cols = ['loan_amnt', 'dti', 'zip_code', 'emp_length', 'accepted']
loan_df = accepted_df[common_cols].append(sample_rejected_df[common_cols])
```




```python
loan_df = loan_df.rename(index=str, columns={"zip_code": "Zip"})
joined_df = pd.merge(loan_df,census_df, on='Zip', how='left')
```




```python
level_vars = ['Population', 'Avg_median_household_inc', 'Households', 'Housing_Units']
race_vars_pct = ['White_pct', 'Black_pct', 'Native_pct', 'Asian_pct', 'Islander_pct', 'Other_pct', 'Two_pct', 'Hispanic_pct']
race_vars_count = ['White', 'Black', 'Native', 'Asian', 'Islander', 'Other', 'Two', 'Hispanic']
sex_vars_pct = ['Female_pct', 'Male_pct']
sex_vars_count = ['Female', 'Male']
education_vars = ['No_Diploma_pct', 'High_School_pct', 'Some_College_pct', 
                  'Bachelors_Degree_pct', 'Graduate_Degree_pct']
household_vars_pct = ['Families_pct', 'Non_families_pct', 'Married_couple_families_pct','Single_parent_families_pct']
household_vars_count = ['Families', 'Non_families', 'Married_couple_families', 'Single_parent_families']
```




```python
def plot_acc_rej(var_list, figwidth, figheight):
    fig, axs = plt.subplots(int(np.ceil(len(var_list)/2)),2, figsize=(figwidth,figheight))
    plt.subplots_adjust(hspace=0.5, wspace=0.5)
    plot_list = axs.ravel()

    for i in np.arange(len(var_list)):
        acc_data = joined_df[joined_df['accepted']==1][var_list[i]]
        plot_list[i].hist(acc_data.dropna(), alpha=0.5, label='Accepted')

        rej_data = joined_df[joined_df['accepted']==0][var_list[i]]
        plot_list[i].hist(rej_data.dropna(), alpha=0.5, label='Rejected')

        plot_list[i].set_title("Histogram of {} by Loan Application Status".format(var_list[i]))
        plot_list[i].legend(loc='best')


    plt.show()
```




```python
plot_acc_rej(level_vars, 15, 10)
```



![png](EthicalImplications_files/EthicalImplications_19_0.png)




```python
plot_acc_rej(sex_vars_count+sex_vars_pct, 15, 10)
```



![png](EthicalImplications_files/EthicalImplications_20_0.png)




```python
plot_acc_rej(race_vars_count+race_vars_pct, 15, 40)
```



![png](EthicalImplications_files/EthicalImplications_21_0.png)




```python
plot_acc_rej(household_vars_count+household_vars_pct, 15, 20)
```



![png](EthicalImplications_files/EthicalImplications_22_0.png)




```python
sex_df = pd.melt(joined_df[['Male_pct', 'accepted']].rename(index=str, columns={"Male_pct": "Male", "accepted": "Loan Accepted"}), 
        id_vars=['Loan Accepted'], value_vars=['Male'], 
        var_name='Sex', value_name='Percent of Total Population')
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Sex", y="Percent of Total Population", data=sex_df, hue="Loan Accepted")
```



![png](EthicalImplications_files/EthicalImplications_23_0.png)




```python
race_df = pd.melt(joined_df[race_vars_pct + ['accepted']].rename(index=str, 
                                           columns={"White_pct": "White", 
                                                    "Black_pct": "Black", 
                                                    "Native_pct": "Native", 
                                                    "Asian_pct": "Asian", 
                                                    "Islander_pct": "Islander", 
                                                    "Hispanic_pct": "Hispanic",
                                                    "Other_pct": "Other", 
                                                    "Two_pct": "Two Races", 
                                                    "accepted": "Loan Accepted"}),
        id_vars=['Loan Accepted'], value_vars=['White', 'Black', 'Native', 'Asian', 'Islander', 'Hispanic', 'Other', 'Two Races'], 
        var_name='Race', value_name='Percent of Total Population')  
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Race", y="Percent of Total Population", data=race_df, hue="Loan Accepted")
```



![png](EthicalImplications_files/EthicalImplications_24_0.png)




```python
household_df = pd.melt(joined_df[['Married_couple_families_pct', 'Single_parent_families_pct', 'Non_families_pct', 'accepted']].rename(index=str, 
                                           columns={"Married_couple_families_pct": "Married Couple Families", 
                                                    "Single_parent_families_pct": "Single Parent Families", 
                                                    "Non_families_pct": "Non-Families", 
                                                    "accepted": "Loan Accepted"}),
        id_vars=['Loan Accepted'], value_vars=['Married Couple Families', 'Single Parent Families', 'Non-Families'], 
        var_name='Household Structure', value_name='Percent of Households') 
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Household Structure", y="Percent of Households", data=household_df, hue="Loan Accepted")
```



![png](EthicalImplications_files/EthicalImplications_25_0.png)




```python
education_df = pd.melt(joined_df[education_vars + ['accepted']].rename(index=str, 
                                           columns={"No_Diploma_pct": "No Diploma", 
                                                    "High_School_pct": "High School", 
                                                    "Some_College_pct": "Some College", 
                                                    "Bachelors_Degree_pct": "Bachelors Degree", 
                                                    "Graduate_Degree_pct": "Graduate Degree", 
                                                    "accepted": "Loan Accepted"}),
        id_vars=['Loan Accepted'], value_vars=['No Diploma', 'High School', 'Some College', 'Bachelors Degree', 'Graduate Degree'], 
        var_name='Highest Education Attained', value_name='Percent of Total Population')      
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Highest Education Attained", y="Percent of Total Population", data=education_df, hue="Loan Accepted")
```



![png](EthicalImplications_files/EthicalImplications_26_0.png)


Of the loans that were accepted, is there any discrimination in terms of the loan amount, funded amount, grade, interest rate, or term?



```python
accepted_df = accepted_df.rename(index=str, columns={"zip_code": "Zip"})
accepted_joined_df = pd.merge(accepted_df,census_df, on='Zip', how='left')
```




```python
sgrade_df = pd.melt(accepted_joined_df[['Male_pct','grade']].rename(index=str, columns={"Male_pct": "Male", "grade": "Loan Grade"}), 
        id_vars=['Loan Grade'], value_vars=['Male'], 
        var_name='Sex', value_name='Percent of Total Population')
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Sex", y="Percent of Total Population", data=sgrade_df, hue="Loan Grade",
                hue_order=['A', 'B', 'C', 'D', 'E', 'F', 'G'])
```



![png](EthicalImplications_files/EthicalImplications_29_0.png)




```python
rgrade_df = pd.melt(accepted_joined_df[race_vars_pct + ['grade']].rename(index=str, 
                                           columns={"White_pct": "White", 
                                                    "Black_pct": "Black", 
                                                    "Native_pct": "Native", 
                                                    "Asian_pct": "Asian", 
                                                    "Islander_pct": "Islander", 
                                                    "Hispanic_pct": "Hispanic",
                                                    "Other_pct": "Other", 
                                                    "Two_pct": "Two Races", 
                                                    "grade": "Loan Grade"}),
        id_vars=['Loan Grade'], value_vars=['White', 'Black', 'Native', 'Asian', 'Islander', 'Hispanic', 'Other', 'Two Races'], 
        var_name='Race', value_name='Percent of Total Population')       
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Race", y="Percent of Total Population", data=rgrade_df, hue="Loan Grade",
                hue_order=['A', 'B', 'C', 'D', 'E', 'F', 'G'])
```



![png](EthicalImplications_files/EthicalImplications_30_0.png)




```python
hgrade_df = pd.melt(accepted_joined_df[['Married_couple_families_pct', 'Single_parent_families_pct', 'Non_families_pct', 'grade']].rename(index=str, 
                                           columns={"Married_couple_families_pct": "Married Couple Families", 
                                                    "Single_parent_families_pct": "Single Parent Families", 
                                                    "Non_families_pct": "Non-Families", 
                                                    "grade": "Loan Grade"}),
        id_vars=['Loan Grade'], value_vars=['Married Couple Families', 'Single Parent Families', 'Non-Families'], 
        var_name='Household Structure', value_name='Percent of Households') 
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Household Structure", y="Percent of Households", data=hgrade_df, hue="Loan Grade",
                hue_order=['A', 'B', 'C', 'D', 'E', 'F', 'G'])
```



![png](EthicalImplications_files/EthicalImplications_31_0.png)




```python
egrade_df = pd.melt(accepted_joined_df[education_vars + ['grade']].rename(index=str, 
                                           columns={"No_Diploma_pct": "No Diploma", 
                                                    "High_School_pct": "High School", 
                                                    "Some_College_pct": "Some College", 
                                                    "Bachelors_Degree_pct": "Bachelors Degree", 
                                                    "Graduate_Degree_pct": "Graduate Degree", 
                                                    "grade": "Loan Grade"}),
        id_vars=['Loan Grade'], value_vars=['No Diploma', 'High School', 'Some College', 'Bachelors Degree', 'Graduate Degree'], 
        var_name='Highest Education Attained', value_name='Percent of Total Population') 
plt.figure(figsize=(15,8))
ax = sns.boxplot(x="Highest Education Attained", y="Percent of Total Population", data=egrade_df, hue="Loan Grade",
                hue_order=['A', 'B', 'C', 'D', 'E', 'F', 'G'])
ax.legend(title="Loan Grade", loc='upper left')
plt.show()
```



![png](EthicalImplications_files/EthicalImplications_32_0.png)


## Modelling



```python
joined_df.head()
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
      <th>loan_amnt</th>
      <th>dti</th>
      <th>Zip</th>
      <th>emp_length</th>
      <th>accepted</th>
      <th>Population</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>...</th>
      <th>Married_couple_child_under_18_pct</th>
      <th>Married_couple_no_child_under_18_pct</th>
      <th>Single_parent_families_pct</th>
      <th>Single_parent_child_under_18_pct</th>
      <th>Single_parent_no_child_under_18_pct</th>
      <th>Non_families_pct</th>
      <th>Householder_living_alone_pct</th>
      <th>Householder_living_with_unrelated_pct</th>
      <th>Occupied_pct</th>
      <th>Household_size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5000.000000</td>
      <td>27.650000</td>
      <td>860xx</td>
      <td>10+ years</td>
      <td>1</td>
      <td>184274.000000</td>
      <td>85837.000000</td>
      <td>2391.000000</td>
      <td>64344.000000</td>
      <td>2553.000000</td>
      <td>...</td>
      <td>16.424996</td>
      <td>26.521836</td>
      <td>22.213048</td>
      <td>11.061802</td>
      <td>11.151246</td>
      <td>34.840119</td>
      <td>23.614910</td>
      <td>11.225209</td>
      <td>0.747368</td>
      <td>3.169651</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2500.000000</td>
      <td>1.000000</td>
      <td>309xx</td>
      <td>&lt; 1 year</td>
      <td>1</td>
      <td>204347.000000</td>
      <td>91932.000000</td>
      <td>92769.000000</td>
      <td>283.000000</td>
      <td>5308.000000</td>
      <td>...</td>
      <td>12.838156</td>
      <td>24.570782</td>
      <td>24.387649</td>
      <td>12.528446</td>
      <td>11.859203</td>
      <td>38.203412</td>
      <td>32.716157</td>
      <td>5.487255</td>
      <td>0.829328</td>
      <td>2.751666</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2400.000000</td>
      <td>8.720000</td>
      <td>606xx</td>
      <td>10+ years</td>
      <td>1</td>
      <td>2697610.000000</td>
      <td>875093.000000</td>
      <td>822035.000000</td>
      <td>3193.000000</td>
      <td>163025.000000</td>
      <td>...</td>
      <td>13.999956</td>
      <td>18.663649</td>
      <td>21.527478</td>
      <td>10.097242</td>
      <td>11.430236</td>
      <td>45.808917</td>
      <td>36.493490</td>
      <td>9.315426</td>
      <td>0.873162</td>
      <td>2.602140</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10000.000000</td>
      <td>20.000000</td>
      <td>917xx</td>
      <td>10+ years</td>
      <td>1</td>
      <td>1977114.000000</td>
      <td>393027.000000</td>
      <td>72504.000000</td>
      <td>4702.000000</td>
      <td>452147.000000</td>
      <td>...</td>
      <td>24.276769</td>
      <td>30.612848</td>
      <td>23.172234</td>
      <td>9.575353</td>
      <td>13.596881</td>
      <td>21.938149</td>
      <td>17.238609</td>
      <td>4.699540</td>
      <td>0.948437</td>
      <td>3.499106</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3000.000000</td>
      <td>17.940000</td>
      <td>972xx</td>
      <td>1 year</td>
      <td>1</td>
      <td>893646.000000</td>
      <td>644973.000000</td>
      <td>42217.000000</td>
      <td>4048.000000</td>
      <td>69569.000000</td>
      <td>...</td>
      <td>17.444413</td>
      <td>24.163792</td>
      <td>13.963949</td>
      <td>7.647337</td>
      <td>6.316612</td>
      <td>44.427846</td>
      <td>32.168502</td>
      <td>12.259344</td>
      <td>0.944561</td>
      <td>2.458543</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 72 columns</p>
</div>


