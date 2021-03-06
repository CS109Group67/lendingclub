---
title: Ethical Implications
notebook: EthicalImplications.ipynb
nav_include: 5
---

## Contents
{:.no_toc}
*  
{: toc}



## 1. Lending and Discrimination

The [Equal Credit Opportunity Act (ECOA)](https://www.consumer.ftc.gov/articles/0347-your-equal-credit-opportunity-rights) is a federal law that prohibits lending entities (both institutions and people) from discriminating "on the basis of race, color, religion, national origin, sex, marital status, age," or because the prospective borrower receives public assistance. Lenders are, however, permitted to consider an applicant's "income, expenses, debts, and credit history" in evaluating the applicant's probability of repaying the debt to decide whether to accept or reject loan applications and to determine loan terms. Nevertheless, differential treatment by race, color, religion, national origin, sex, etc can still arise even when an institution or person is not explicitly discriminating based on those characteristics. Thus, we want to assess whether we see evidence of discrimination in LendingClub's acceptances and rejections of loan applications or in the terms it sets for accepted loans. In addition, we want to evaluate whether our proposed investing strategy results in differential treatment.

## 2. Literature Review

To inform our approach, we surveyed existing research of how predictive algorithms can result in discrimination and methods for addressing it.

**2A. Sweeney, L. "Discrimination in Online Ad Delivery". Communications of the ACM, May 2013, Vol. 56 No. 5, Pages 44-54.**

Sweeney's research presents evidence of discrimination in ad delivery on Google.com and Reuters.com on the basis of racially associated names. Using a chi-squared analysis, Sweeney found that running a search on a black-associated name was 25% more likely to get arrested-related ads compared to running a search on a white-associated name. All names used in the study were the full names of real people, roughly evenly split between professionals and "netizens" (i.e. people active on the internet - social media and blogs), and about one-third black and two-thirds white.

While disentangling the root cause of the discrimination fell outside the scope of the study, Sweeney's research clearly showcases the need to actively evaluate possibly unintended societal implications of the algorithms we put in place.

**2B. Datta, A., Tschantz, M.C., and Datta, A. "Automated Experiments on Ad Privacy Settings: A Tale of Opacity, Choice, and Discrimination". Proceedings on Privacy Enhancing Technologies 2015; 2015 (1):92–112.**

Datta et al. found evidence of gender discimination in employment-related searches by experimentally manipulating whether a simulated user specified male or female in Google's ad settings. Their results revealed that simulated male users were shown ads regarding coaching for high-paying careers more often than they were shown to simulated female users. They collected the ads results of 1000 simulated users - half male and half female - and trained a classifier predicting gender using the ad URLs and titles as the feature set. Their classifier used a 90-10 train-test split, and achieved 93% accuracy on the test set. The high predictive power of the classifier suggests gender discrimination in Google's employment-related ad results, and such discrimination can exacerbate the current gender pay gap.

With limited visibility into the details of Google's ad vending algorithm, the authors of this paper were also unable to isolate the mechanisms through which the discrimination is arising. But again, like Sweeney's research, it demonstrates algorithms, while not inherently subjective, can be discriminatory as well.

**2C. Angwin, J., Larson, J., Mattu, S., and Kirchner, L. "Machine Bias". ProPublica. May 23, 2016.**

In an article published on ProPublica, the authors found that COMPAS (Correctional Offender Management Profiling for Alternative Sanctions), one of the nation's most popular tools in criminal risk assessment, discriminates unfavorably toward black defendants but favorably toward white defendants. Although the prediction accuracy was about the same as the overall accuracy (61%) in both groups, COMPAS makes a systematically different kind of error depending on the defendant's race group. Specifically, black defendants were twice as likely as black defendants to fall victim to a false positive result, i.e. they did not reoffend within the next two years but were wrongly labelled as highly likely to reoffend (i.e. high risk). On the other hand, white defendants were twice as likely as black defendants to receive a false negative assessment, i.e. they did reoffend within the next two years but were incorrectly labelled as not very likely to reoffend (i.e. low risk). Using a logistic regression model, Angwin et al. found that bias against black defendants in COMPAS's predictions (higher risk scores) remains even after controlling for age, criminal history, future (actual) recidivism, charge degree, and gender.

## 3. Census Data Summary

Especially when machine-generated predictions carry significant life consequences, developers and scientists have a responsibility to ensure their algorithms do not create or exacerbate societal problems through disparate outcomes and impacts on different groups.

While LendingClub does not have the demographic data associated with each loan or loan application (either they don't collect it or they just don't make it publicly available), LendingClub does share the 3-digit zip codes of the borrowers and applicants. We leveraged these zip codes to tie high-level demographic information to the loan data.

After data processing (as described previously in the [Data Description](https://cs109group67.github.io/lendingclub/DataDescription.html) section), there are five records with missing data, likely originating from sparsely populated zip code areas. At least one 3-digit zip code area has a recorded population of zero. On average, the population is ~350k, with a household size of ~2.6 and a median income of $55k. For the average zip code area, the population is about evenly split between male and female, with the majority of the population White and having at least a High School education. (See **Table 1** below.)













**Table 1. Selected Summary Statistics for Census Data**








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



## 4. Exploratory Data Analysis

Leveraging zip codes, we explored whether there were demographic differences between loans that LendingClub accepted or rejected for its marketplace. We looked at the distributions of acceptances and rejections by demographic groups, on the basis of which federal law prohibits discrimination, in terms of both population count and as a percentage of the total population.

Visually, our exploratory data analysis did not suggest discrimination in either loan status or the grade assigned to issued loans. Between different sexes, races, household structures, and educational attainments, there was not a significant difference between either the accept-reject decision of loan applications or the resulting loan grade of the issued loans, as suggested by the large overlap in the histograms and boxplots in **Figures 1-5**.





































**Figure 1. Distributions of Loan Acceptances and Rejections by Populousness and Income**






![png](EthicalImplications_files/EthicalImplications_21_0.png)


Looking at the distributions of acceptances versus rejections by Population size, count of Households, and count of Housing Units, we observe a slight favoring of more populous areas for loan acceptances. Unsurprisingly, we also see that compared to the rejected loans, the accepted loans came from applicants in areas with higher median income.

**Figure 2A. Histograms: Distributions of Loan Acceptances and Rejections by Sex (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_24_0.png)


**Figure 2B. Boxplots: Distributions of Loan Acceptances and Rejections by Sex (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_26_0.png)


**Figure 3A. Histograms: Distributions of Loan Acceptances and Rejections by Race (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_28_0.png)


**Figure 3B. Boxplots: Distributions of Loan Acceptances and Rejections by Race (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_30_0.png)


**Figure 4A. Histograms: Distributions of Loan Acceptances and Rejections by Household Structure and Marital Status (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_32_0.png)


**Figure 4B. Boxplots: Distributions of Loan Acceptances and Rejections by Household Structure and Marital Status (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_34_0.png)


**Figure 5. Boxplots: Distributions of Loan Acceptances and Rejections by Educational Attainment (Count and Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_36_0.png)


Further, of the loans that were accepted, we checked to see if there were noticeable differences in assigned loan grade by demographic groups. Loan grade will influence the interest rate the borrower is charged and whether or not investors want to fund the loan. Again, the data visualizations did not suggest discrimination. (See **Figures 6-9** below.)





**Figure 6A. Distributions of Loan Grade by Sex (Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_40_0.png)


**Figure 6B. Distributions of Loan Grade by Sex (Population Count)**






![png](EthicalImplications_files/EthicalImplications_42_0.png)


**Figure 7A. Distributions of Loan Grade by Race (Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_44_0.png)


**Figure 7B. Distributions of Loan Grade by Race (Population Count)**






![png](EthicalImplications_files/EthicalImplications_46_0.png)


**Figure 8A. Distributions of Loan Grade by Household Structure and Marital Status (Percent of Total Households)**






![png](EthicalImplications_files/EthicalImplications_48_0.png)


**Figure 8B. Distributions of Loan Grade by Household Structure and Marital Status (Household Count)**






![png](EthicalImplications_files/EthicalImplications_50_0.png)


**Figure 9. Distributions of Loan Grade by Educational Attainment (Percent of Total Population)**






![png](EthicalImplications_files/EthicalImplications_52_0.png)


Going forward in the modelling, we chose to focus on the demographic measures as a percentage of the total population, because demographic information by zip code gets muddled by how populous the zip code is when using population counts. As we can see in the histograms in **Figures 1-4**, using counts makes the acceptance distributions slightly to the right of the rejection distributions for all demographic groups because accepted loans tended to come from more populous zip codes as shown in **Figure 1**, in the distributions by Population size.

## 5. Is there evidence of discrimination in LendingClub loans?

To evaluate whether certain demographic features might have influenced whether LendingClub accepted or rejected a loan application, we use random forests. For each tree in the forest, we recorded what the most important feature was, i.e. the feature on which the first split of that tree was made. Across all the trees in the forest, we created a frequency chart to assess relative feature importance (**Figure 10**).

















    The test accuracy of the RandomForestClassifier with max_depth 25 and 50 trees is 0.91.






**Figure 10. Feature Importance in Loan Acceptances vs. Rejections**






![png](EthicalImplications_files/EthicalImplications_62_0.png)


The random forest was able to predict whether the loan decision was accept or reject with 91% accuracy on a separate test set. As we can see from the feature importance chart above, many of the most important features are related to income and ability to pay back, such as debt-to-income ratio, the percentage of the population that falls in certain income brackets, employment length (indicating a stable source of income), and the percentage of the population that has at least a Bachelor's degree (which improves earning potential). Interestingly, the percentage of the population that is Asian is the fourth most important feature. Looking back at **Figure 3**, it seems that a higher Asian percentage of the population is favorable towards a loan acceptance.





The interest rate that a borrower is charged depends on the grade and sub-grade LendingClub assigns their loan. LendingClub determines the grade and sub-grade of the loan using a formula that considers credit score and credit risk. By the Equal Credit Opportunity Act (ECOA), lenders are also not permitted to discriminate loan terms on the basis of certain demographic group memberships. Thus, we investigated whether the demographic features in our dataset revealed a significant influence on loan sub-grade by conducting a linear regression. We removed some of the percent variables to reduce multi-collinearity, because summing all the percent variables of a certain demographic characteristic results in 100% (e.g. Male percent + Female percent). We replaced the income related variables in the Census dataset with the income data that was specific to each loan from the LoanStats dataset. We also included a select few variables that are related to credit score and credit risk, which LendingClub factors into loan grade and sub-grade assignment.













    The R^2 on the test set is 0.04.









<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>        <td>sub_grade</td>    <th>  R-squared:         </th>  <td>   0.043</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.042</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   917.5</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Wed, 12 Dec 2018</td> <th>  Prob (F-statistic):</th>   <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>19:27:50</td>     <th>  Log-Likelihood:    </th> <td>-1.1520e+06</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>371729</td>      <th>  AIC:               </th>  <td>2.304e+06</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td>371710</td>      <th>  BIC:               </th>  <td>2.304e+06</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>    18</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
               <td></td>                  <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th>                       <td>   15.3605</td> <td>    0.895</td> <td>   17.163</td> <td> 0.000</td> <td>   13.606</td> <td>   17.115</td>
</tr>
<tr>
  <th>annual_inc</th>                  <td>-5.496e-06</td> <td> 1.73e-07</td> <td>  -31.778</td> <td> 0.000</td> <td>-5.83e-06</td> <td>-5.16e-06</td>
</tr>
<tr>
  <th>earliest_cr_line</th>            <td>   -0.0003</td> <td> 3.38e-06</td> <td>  -76.958</td> <td> 0.000</td> <td>   -0.000</td> <td>   -0.000</td>
</tr>
<tr>
  <th>emp_length</th>                  <td>   -0.0095</td> <td>    0.002</td> <td>   -3.963</td> <td> 0.000</td> <td>   -0.014</td> <td>   -0.005</td>
</tr>
<tr>
  <th>loan_amnt</th>                   <td> 1.309e-05</td> <td> 1.24e-06</td> <td>   10.524</td> <td> 0.000</td> <td> 1.06e-05</td> <td> 1.55e-05</td>
</tr>
<tr>
  <th>dti</th>                         <td>    0.0910</td> <td>    0.001</td> <td>   79.384</td> <td> 0.000</td> <td>    0.089</td> <td>    0.093</td>
</tr>
<tr>
  <th>Population</th>                  <td>-9.455e-09</td> <td> 1.72e-08</td> <td>   -0.549</td> <td> 0.583</td> <td>-4.32e-08</td> <td> 2.43e-08</td>
</tr>
<tr>
  <th>High_School_pct</th>             <td>    0.0123</td> <td>    0.005</td> <td>    2.407</td> <td> 0.016</td> <td>    0.002</td> <td>    0.022</td>
</tr>
<tr>
  <th>Some_College_pct</th>            <td>    0.0050</td> <td>    0.004</td> <td>    1.140</td> <td> 0.254</td> <td>   -0.004</td> <td>    0.014</td>
</tr>
<tr>
  <th>Bachelors_Degree_pct</th>        <td>   -0.0146</td> <td>    0.005</td> <td>   -2.803</td> <td> 0.005</td> <td>   -0.025</td> <td>   -0.004</td>
</tr>
<tr>
  <th>Graduate_Degree_pct</th>         <td>   -0.0036</td> <td>    0.005</td> <td>   -0.664</td> <td> 0.507</td> <td>   -0.014</td> <td>    0.007</td>
</tr>
<tr>
  <th>Family_Poverty_pct</th>          <td>   -0.0143</td> <td>    0.005</td> <td>   -2.737</td> <td> 0.006</td> <td>   -0.025</td> <td>   -0.004</td>
</tr>
<tr>
  <th>Unemployment_Rate_pct</th>       <td>    0.0396</td> <td>    0.007</td> <td>    5.470</td> <td> 0.000</td> <td>    0.025</td> <td>    0.054</td>
</tr>
<tr>
  <th>White_pct</th>                   <td>   -0.0137</td> <td>    0.003</td> <td>   -4.348</td> <td> 0.000</td> <td>   -0.020</td> <td>   -0.007</td>
</tr>
<tr>
  <th>Black_pct</th>                   <td>   -0.0044</td> <td>    0.003</td> <td>   -1.379</td> <td> 0.168</td> <td>   -0.011</td> <td>    0.002</td>
</tr>
<tr>
  <th>Asian_pct</th>                   <td>    0.0036</td> <td>    0.004</td> <td>    0.864</td> <td> 0.388</td> <td>   -0.005</td> <td>    0.012</td>
</tr>
<tr>
  <th>Hispanic_pct</th>                <td>   -0.0051</td> <td>    0.003</td> <td>   -1.581</td> <td> 0.114</td> <td>   -0.011</td> <td>    0.001</td>
</tr>
<tr>
  <th>Male_pct</th>                    <td>   -0.0915</td> <td>    0.012</td> <td>   -7.706</td> <td> 0.000</td> <td>   -0.115</td> <td>   -0.068</td>
</tr>
<tr>
  <th>Married_couple_families_pct</th> <td>    0.0068</td> <td>    0.002</td> <td>    3.635</td> <td> 0.000</td> <td>    0.003</td> <td>    0.011</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>15005.882</td> <th>  Durbin-Watson:     </th> <td>   2.001</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>16351.077</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.499</td>   <th>  Prob(JB):          </th> <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 2.759</td>   <th>  Cond. No.          </th> <td>1.12e+08</td> 
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[2] The condition number is large, 1.12e+08. This might indicate that there are<br/>strong multicollinearity or other numerical problems.



Of the demographic features, `Bachelors_Degree_pct`, `Unemployment_Rate_pct`, `White_pct`, `Male_pct`, and `Married_couple_families_pct` are the only ones that have coefficients that are statistically significant at the 1% level. Loans from a 3-digit zip code areas that have a higher percentage of the population that is educated to the Bachelors level, a higher percentage of the population that is White, and a higher percentage of the population that is Male tend to have more favorable loan grade assignments (negative coefficients). On the other hand, loans from a 3-digit zip code areas that have a higher percentage of the population that is unemployed and a higher percentage of households with a married couple tend to have less favorable loan grade assignments (positive coefficients). Despite the statistical significance of some of the coefficients, the $R^2$ of the model is only 4%, so from these results, I would not conclude that there is discrimination in determination of loan sub-grade (which has implications for the loan terms).





    The R^2 on the test set is 0.00.









<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>        <td>sub_grade</td>    <th>  R-squared:         </th>  <td>   0.003</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th>  <td>   0.003</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>  <td>   88.55</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Wed, 12 Dec 2018</td> <th>  Prob (F-statistic):</th>  <td>1.39e-237</td> 
</tr>
<tr>
  <th>Time:</th>                 <td>19:27:53</td>     <th>  Log-Likelihood:    </th> <td>-1.1592e+06</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>371729</td>      <th>  AIC:               </th>  <td>2.318e+06</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td>371715</td>      <th>  BIC:               </th>  <td>2.319e+06</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>    13</td>      <th>                     </th>      <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>      <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
               <td></td>                  <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th>                       <td>   14.4362</td> <td>    0.912</td> <td>   15.824</td> <td> 0.000</td> <td>   12.648</td> <td>   16.224</td>
</tr>
<tr>
  <th>Population</th>                  <td>-2.876e-08</td> <td> 1.76e-08</td> <td>   -1.635</td> <td> 0.102</td> <td>-6.32e-08</td> <td> 5.71e-09</td>
</tr>
<tr>
  <th>High_School_pct</th>             <td>    0.0258</td> <td>    0.005</td> <td>    4.954</td> <td> 0.000</td> <td>    0.016</td> <td>    0.036</td>
</tr>
<tr>
  <th>Some_College_pct</th>            <td>    0.0193</td> <td>    0.004</td> <td>    4.341</td> <td> 0.000</td> <td>    0.011</td> <td>    0.028</td>
</tr>
<tr>
  <th>Bachelors_Degree_pct</th>        <td>   -0.0148</td> <td>    0.005</td> <td>   -2.795</td> <td> 0.005</td> <td>   -0.025</td> <td>   -0.004</td>
</tr>
<tr>
  <th>Graduate_Degree_pct</th>         <td>   -0.0009</td> <td>    0.006</td> <td>   -0.167</td> <td> 0.868</td> <td>   -0.012</td> <td>    0.010</td>
</tr>
<tr>
  <th>Family_Poverty_pct</th>          <td>    0.0062</td> <td>    0.005</td> <td>    1.165</td> <td> 0.244</td> <td>   -0.004</td> <td>    0.017</td>
</tr>
<tr>
  <th>Unemployment_Rate_pct</th>       <td>   -0.0009</td> <td>    0.007</td> <td>   -0.125</td> <td> 0.900</td> <td>   -0.015</td> <td>    0.014</td>
</tr>
<tr>
  <th>White_pct</th>                   <td>   -0.0150</td> <td>    0.003</td> <td>   -4.683</td> <td> 0.000</td> <td>   -0.021</td> <td>   -0.009</td>
</tr>
<tr>
  <th>Black_pct</th>                   <td>   -0.0051</td> <td>    0.003</td> <td>   -1.537</td> <td> 0.124</td> <td>   -0.011</td> <td>    0.001</td>
</tr>
<tr>
  <th>Asian_pct</th>                   <td>    0.0048</td> <td>    0.004</td> <td>    1.135</td> <td> 0.256</td> <td>   -0.004</td> <td>    0.013</td>
</tr>
<tr>
  <th>Hispanic_pct</th>                <td>   -0.0054</td> <td>    0.003</td> <td>   -1.621</td> <td> 0.105</td> <td>   -0.012</td> <td>    0.001</td>
</tr>
<tr>
  <th>Male_pct</th>                    <td>   -0.0875</td> <td>    0.012</td> <td>   -7.224</td> <td> 0.000</td> <td>   -0.111</td> <td>   -0.064</td>
</tr>
<tr>
  <th>Married_couple_families_pct</th> <td>    0.0044</td> <td>    0.002</td> <td>    2.296</td> <td> 0.022</td> <td>    0.001</td> <td>    0.008</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>15383.027</td> <th>  Durbin-Watson:     </th> <td>   1.994</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>15270.137</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.458</td>   <th>  Prob(JB):          </th> <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 2.616</td>   <th>  Cond. No.          </th> <td>1.12e+08</td> 
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[2] The condition number is large, 1.12e+08. This might indicate that there are<br/>strong multicollinearity or other numerical problems.



We also ran the linear regression with demographic predictors only (i.e. not controlling for the selected loan features included in the first regression). While similar coefficients are statistically significant, the $R^2$ is close to 0%, so from these results, I also would not conclude that there is discrimination in determination of loan sub-grade (which has implications for the loan terms).

## 6. Is there evidence of discrimination in our proposed investment strategy?









    The test accuracy of the RandomForestClassifier with max_depth 25 and 50 trees is 0.88.






**Figure 11. Feature Importance in Our Proposed Investment Decision Strategy**






![png](EthicalImplications_files/EthicalImplications_79_0.png)


The random forest, using the same predictors as we used in the acceptance versus rejection classifcation, was able to predict our strategy's invest or not invest decision with 89% accuracy on a separate test set. As we can see from the feature importance chart above, employment length and debt-to-income ratio are the two top features, which makes sense given that our proposed investment strategy incorporates those two variables (among other variables) in its investment decision. While the strategy does not explicitly consider demographic variables to make an investment decision, it appears our investment strategy is inadvertently impacted by demographic features; `White_pct`, `Married_couple_families_pct`, `Asian_pct`, and `Hispanic_pct` show up toward the top of the feature importance list. As NYU Professor Julia Stoyanovich suggested in her lecture, unfair model outcomes can sometimes be the result of actual societal unfairness playing out in the data, and this would be a logical area of further research or next steps for this project. In the context of our project, since we are using `emp_length` and `dti` to make investment decisions, we could look into whether certain racial groups are more or less likely to have longer employment histories or differing debt-to-income ratios.
