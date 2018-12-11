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

Lending Club publishes funded loan data (`LoanStats`) each quarter. We accessed the data as of the 2018Q2 update. These quarterly datasets were merged to create a DataFrame which initially contained 145 features for 2,004,063 approved loans. The issue dates range from June 2007 to June 2018 and the loan terms are 36 months or 60 months. After cleaning, the dataset was reduced to 359,377 term-complete loans with 3 outcome features, 23 dummy variables and  60 numeric variables. See the [EDA & Cleaning](https://cs109group67.github.io/lendingclub/EDA.html) section for more details on the `loanstats` data.








![png](DataDescription_files/DataDescription_4_0.png)



## 2. RejectStats

Rejected loan datasets (`RejectStats`) were merged to create a DataFrame with 9 features of 22,469,074 loan applications that failed to meet LC underwriting policy. These loan applications have less relevance to prospective investors since they were not approved for funding on the platform. 








![png](DataDescription_files/DataDescription_7_0.png)



## 3. Census Data

To understand whether there is discrimination in LC’s practices and ultimately our proposed investing strategy, we obtained the U.S. Census Bureau’s demographic data and leveraged zip codes to tie the demographic information to the loan data. This data reports the total population and population by different demographic groups (i.e., race, income, education, sex, household structure) and other characteristics like poverty and unemployment rate by zip code. Because LC data is reported at the 3-digit zip code level, we aggregated up to the 3-digit zip code level and calculated percentage of total population and percentage of total household values. We also derived some additional variables such as household size (from population and number of households) and occupied rate (from number of households and housing units). We dropped the variables related to age, as we felt that discrimination related to age is appropriate because it is highly correlated with length of credit history, a valid criterion on which loan applications should be evaluated but is not available in the LendingClub data. Post processing, our census dataset had 891 3-digit zip codes and 68 features. See the [Ethical Implications](https://cs109group67.github.io/lendingclub/EthicalImplications.html) section for more details on the Census Data.
