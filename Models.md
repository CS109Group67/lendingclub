---
title: Models
notebook: Models.ipynb
nav_include: 3
---

## Contents
{:.no_toc}
*  
{: toc}










## 1. Preprocessing

### 1A. Train-Test Split

Before we begin modeling, we set aside a test set that we will use later to evaluate the predictive quality of our investment strategy. We do this in a stratified fashion ensuring that the outcome classes (fully paid loans and not fully paid loans) are equally represented in the train and test sets. For the splitting algorithm, we use `sklearn`'s `train_test_split` function. This function creates random train and test subsets of the dataset. The flag `stratify` ensures that both classes are equally represented in each set. 



```python
from sklearn.model_selection import train_test_split
ls_train, ls_test = train_test_split(ls, test_size=0.15, stratify=ls['OUT_Class'])
```


### 1B. Standard Scaling

The models used in the next sections assume that the features are on similar scales. To achieve this, we transform the numeric variables to a standard scale with mean 0 and standard deviation 1 using sklearn's `StandardScaler` function.



```python
#IDENTIFY THE OUTCOME, DUMMY AND NUMERIC VARIABLES
var_list = set(ls.columns)
outcome_var_list = set(out_var for out_var in var_list if "OUT_" in out_var)
dummy_var_list = set(dummy for dummy in var_list if "D_" in dummy)
numeric_var_list = var_list - outcome_var_list - dummy_var_list
```




```python
#STANDARD SCALER
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


## 2. Logistic Classification

The first model is a logistic regression on the outcome variable `OUT_class` which is the binary classification for loans are either fully repaid (1) or charged off (0). The flag `class_weight='balanced'` ensures that both classes are equally represented in each set. 



```python
#SET TARGET VARIABLE 'OUT_Class'
target_train = outcome_train.iloc[:,0]
target_test = outcome_test.iloc[:,0]
```




```python
#LOGISTIC REGRESSION
from sklearn.linear_model import LogisticRegression
classifier = LogisticRegression(random_state=0, solver='lbfgs', max_iter=10000, class_weight='balanced')
classifier.fit(feature_train, target_train)
target_predicted = classifier.predict(feature_test)
target_probabilities = classifier.predict_proba(feature_test)[:,0]
```




```python
#MODEL EVALUATION
from sklearn.model_selection import cross_val_score

#ACCURACY
cross_val_accuracy = cross_val_score(classifier, feature_train, target_train, scoring='accuracy', cv=5).mean()
print('Accuracy: {:.4}'.format(cross_val_accuracy))

#PRECISION: true good loans / total predicted good loans
precision = cross_val_score(classifier, feature_train, target_train, scoring='precision', cv=5).mean()
print('Precision: {:.4}'.format(precision))

#RECALL: true good loans / total actual good loans
recall = cross_val_score(classifier, feature_train, target_train, scoring='recall', cv=5).mean()
print('Recall: {:.4}'.format(recall))
```


    Accuracy: 0.6309
    Precision: 0.9153
    Recall: 0.6307




```python
#CONFUSION MATRIX
from sklearn.metrics import confusion_matrix

matrix = pd.DataFrame(confusion_matrix(target_test, target_predicted),
                      index=['Fully Repaid', 'Not Fully Repaid'],
                      columns=['Fully Repaid', 'Not Fully Repaid'])

fig, ax = plt.subplots()
sns.heatmap(matrix, annot=True, fmt='g', cbar=None, cmap='Blues')
plt.title('Confusion Matrix of Binary Classification Predictions')
plt.tight_layout()
plt.ylabel('True Class')
plt.xlabel('Predicted Class')
ax.set_xticklabels(['Charged Off', 'Fully Repaid'], va='center')
ax.set_yticklabels(['Charged Off', 'Fully Repaid'], va='center')
plt.show()
```



![png](Models_files/Models_18_0.png)




```python
#ROC CURVE
from sklearn.metrics import roc_curve, roc_auc_score
false_positive_rate, true_positive_rate, threshold = roc_curve(target_test, target_probabilities)
plt.title('Reciever Operating Characterisic')
plt.plot(false_positive_rate, true_positive_rate, label='ROC')
plt.plot([0,1], ls='--',label='random')
plt.plot([0,0],[1,0], c='.7', )
plt.plot([1,1], c='.7', label='perfect')
plt.legend()
plt.ylabel('True Positive Rate')
plt.xlabel('False Positive Rate')
plt.show()
```



![png](Models_files/Models_19_0.png)


## 3. Linear Regression

## 4. Trees and Forest

## 5. K-Nearest Neighbors
