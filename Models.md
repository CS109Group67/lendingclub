---
title: Models
notebook: Models.ipynb
---

## Contents
{:.no_toc}
*  
{: toc}










## 0. Preprocessing

### Train-Test Split

Before we begin modeling, we set aside a test set that we will use later to evaluate the predictive quality of our investment strategy. We do this in a stratified fashion ensuring that the outcome classes (fully paid loans and not fully paid loans) are equally represented in the train and test sets. For the splitting algorithm, we use `sklearn`'s `train_test_split` function. This function creates random train and test subsets of the dataset. The flag `stratify` ensures that both classes are equally represented in each set. 



```python
from sklearn.model_selection import train_test_split
ls_train, ls_test = train_test_split(ls, test_size=0.15, stratify=ls['OUT_Class'])
```


### Standard Scaling

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


## 1. Logisitic Regression Classification

The first model is a logistic regression on the outcome variable `OUT_class` which is the binary classification for loans are either fully repaid (1) or charged off (0). The flag `class_weight='balanced'` ensures that both classes are equally represented in each set. 



```python
#SET TARGET VARIABLE 'OUT_Class'
target_train = outcome_train.iloc[:,1]
```




```python
#LOGISTIC REGRESSION
from sklearn.linear_model import LogisticRegression
classifier = LogisticRegression(random_state=0, solver='lbfgs', max_iter=10000, class_weight='balanced')
classifier.fit(feature_train, target_train)
target_predicted = classifier.predict(feature_test)
target_probabilities = classifier.predict_proba(feature_test)[:,1]
```



    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-9-39aaa05880b1> in <module>()
          2 from sklearn.linear_model import LogisticRegression
          3 classifier = LogisticRegression(random_state=0, solver='lbfgs', max_iter=10000, class_weight='balanced')
    ----> 4 classifier.fit(feature_train, target_train)
          5 target_predicted = classifier.predict(feature_test)
          6 target_probabilities = classifier.predict_proba(feature_test)[:,1]


    ~/anaconda3/lib/python3.6/site-packages/sklearn/linear_model/logistic.py in fit(self, X, y, sample_weight)
       1283         X, y = check_X_y(X, y, accept_sparse='csr', dtype=_dtype, order="C",
       1284                          accept_large_sparse=solver != 'liblinear')
    -> 1285         check_classification_targets(y)
       1286         self.classes_ = np.unique(y)
       1287         n_samples, n_features = X.shape


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/multiclass.py in check_classification_targets(y)
        169     if y_type not in ['binary', 'multiclass', 'multiclass-multioutput',
        170                       'multilabel-indicator', 'multilabel-sequences']:
    --> 171         raise ValueError("Unknown label type: %r" % y_type)
        172 
        173 


    ValueError: Unknown label type: 'continuous'




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


    /Users/michal/anaconda3/lib/python3.6/site-packages/sklearn/model_selection/_validation.py:542: FutureWarning: From version 0.22, errors during fit will result in a cross validation score of NaN by default. Use error_score='raise' if you want an exception raised or error_score=np.nan to adopt the behavior from version 0.22.
      FutureWarning)



    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-10-a5d7e096d108> in <module>()
          3 
          4 #ACCURACY
    ----> 5 cross_val_accuracy = cross_val_score(classifier, feature_train, target_train, scoring='accuracy', cv=5).mean()
          6 print('Accuracy: {:.4}'.format(cross_val_accuracy))
          7 


    ~/anaconda3/lib/python3.6/site-packages/sklearn/model_selection/_validation.py in cross_val_score(estimator, X, y, groups, scoring, cv, n_jobs, verbose, fit_params, pre_dispatch, error_score)
        400                                 fit_params=fit_params,
        401                                 pre_dispatch=pre_dispatch,
    --> 402                                 error_score=error_score)
        403     return cv_results['test_score']
        404 


    ~/anaconda3/lib/python3.6/site-packages/sklearn/model_selection/_validation.py in cross_validate(estimator, X, y, groups, scoring, cv, n_jobs, verbose, fit_params, pre_dispatch, return_train_score, return_estimator, error_score)
        238             return_times=True, return_estimator=return_estimator,
        239             error_score=error_score)
    --> 240         for train, test in cv.split(X, y, groups))
        241 
        242     zipped_scores = list(zip(*scores))


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self, iterable)
        981             # remaining jobs.
        982             self._iterating = False
    --> 983             if self.dispatch_one_batch(iterator):
        984                 self._iterating = self._original_iterator is not None
        985 


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in dispatch_one_batch(self, iterator)
        823                 return False
        824             else:
    --> 825                 self._dispatch(tasks)
        826                 return True
        827 


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in _dispatch(self, batch)
        780         with self._lock:
        781             job_idx = len(self._jobs)
    --> 782             job = self._backend.apply_async(batch, callback=cb)
        783             # A job can complete so quickly than its callback is
        784             # called before we get here, causing self._jobs to


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in apply_async(self, func, callback)
        180     def apply_async(self, func, callback=None):
        181         """Schedule a func to be run"""
    --> 182         result = ImmediateResult(func)
        183         if callback:
        184             callback(result)


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in __init__(self, batch)
        543         # Don't delay the application, to avoid keeping the input
        544         # arguments in memory
    --> 545         self.results = batch()
        546 
        547     def get(self):


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self)
        259         with parallel_backend(self._backend):
        260             return [func(*args, **kwargs)
    --> 261                     for func, args, kwargs in self.items]
        262 
        263     def __len__(self):


    ~/anaconda3/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in <listcomp>(.0)
        259         with parallel_backend(self._backend):
        260             return [func(*args, **kwargs)
    --> 261                     for func, args, kwargs in self.items]
        262 
        263     def __len__(self):


    ~/anaconda3/lib/python3.6/site-packages/sklearn/model_selection/_validation.py in _fit_and_score(estimator, X, y, scorer, train, test, verbose, parameters, fit_params, return_train_score, return_parameters, return_n_test_samples, return_times, return_estimator, error_score)
        526             estimator.fit(X_train, **fit_params)
        527         else:
    --> 528             estimator.fit(X_train, y_train, **fit_params)
        529 
        530     except Exception as e:


    ~/anaconda3/lib/python3.6/site-packages/sklearn/linear_model/logistic.py in fit(self, X, y, sample_weight)
       1283         X, y = check_X_y(X, y, accept_sparse='csr', dtype=_dtype, order="C",
       1284                          accept_large_sparse=solver != 'liblinear')
    -> 1285         check_classification_targets(y)
       1286         self.classes_ = np.unique(y)
       1287         n_samples, n_features = X.shape


    ~/anaconda3/lib/python3.6/site-packages/sklearn/utils/multiclass.py in check_classification_targets(y)
        169     if y_type not in ['binary', 'multiclass', 'multiclass-multioutput',
        170                       'multilabel-indicator', 'multilabel-sequences']:
    --> 171         raise ValueError("Unknown label type: %r" % y_type)
        172 
        173 


    ValueError: Unknown label type: 'continuous'




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


## Regression

## Trees and Forest

## K-Nearest Neighbors
