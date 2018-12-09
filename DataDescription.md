---
title:  Data Description
notebook: DataDescription.ipynb
nav_include: 1
---

## Contents
{:.no_toc}
*  
{: toc}






## 1. LoanStats

Lending Club publishes funded loan data (`LoanStats`) each quarter. We accessed the data as of the 2018Q2 update. These quarterly datasets were merged to create a DataFrame which initially contained 145 features for 2,004,063 approved loans.








![png](DataDescription_files/DataDescription_4_0.png)



After cleaning `loanstats` the dataset contains 3 outcome features, 23 dummy variables and  60 numeric variables (see [EDA & Cleaning](https://cs109group67.github.io/lendingclub/EDA.html)). The loan issue dates range from June 2007 to June 2018. The loan terms are 36 months or 60 months, which means 'term-complete' loans are only those issued prior to 2013-Q2 for 60-month terms and prior to 2015-Q2 for 36-month terms. The number and notional of LC loan issuances over time is visualized below.








![png](DataDescription_files/DataDescription_6_0.png)



## 2. RejectStats

Rejected loan datasets (`RejectStats`) were merged to create a DataFrame with 9 features of 22,469,074 loan applications that failed to meet LC underwriting policy








![png](DataDescription_files/DataDescription_9_0.png)



## 3. Census Data

To understand whether there is discrimination in LC’s practices and ultimately our proposed investing strategy, we obtained the U.S. Census Bureau’s demographic data and leveraged zip codes to tie the demographic information to the loan data. This data reports the total population and population by different demographic groups (i.e., race, income, education, sex, household structure) and other characteristics like poverty and unemployment rate by zip code. Because LC data is reported at the 3-digit zip code level, we aggregated up to the 3-digit zip code level and calculated percentage of total population and percentage of total household values. We also derived some additional variables such as household size (from population and number of households) and occupied rate (from number of households and housing units). We dropped the variables related to age, as we felt that discrimination related to age is appropriate because it is highly correlated with length of credit history, a valid criterion on which loan applications should be evaluated. Post processing, our census dataset had 891 3-digit zip codes and 68 features.

There are five records with missing data, likely originating from sparsely populated zip code areas. At least one 3-digit zip code area has a recorded population of zero. On average, the population is ~350k, with a household size of ~2.6 and a median income of $55k. For the average zip code area, the population is about evenly split between male and female, with the majority of the population White and having at least a High School education.
