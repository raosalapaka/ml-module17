# Overview

This README has the study that compares different classifiers to analyze attributes that contribute to success of directed bank marketing campaigns. Success is defined as the campaign resulting in a successful subscription of a deposit by the targeted client. The approach was to follow CRISP-DM framework to find the best classifier model to classify if a targeted direct campaign contact to a client would be successful or not. 

Link to the jupyter notebook: [Assignment 17_1 Jupyter notebook](https://github.com/raosalapaka/ml-module17/blob/main/assignment17_1_RS.ipynb)

Link to github: [Github](https://github.com/raosalapaka/ml-module17)

Link to the repository: [Git repository](https://github.com/raosalapaka/ml-module17.git)

---
# **Comparing Classifiers**

## **Summary**

Following CRISP-DM framework to do the analysis.

- **Business Understanding**: main objective of this study is the find the best model that can explain the success of the bank marketing campaigns and the the attribute that lead to the success by comparing multiple classifier models

- [**Data Understanding**](#Data-Understanding): understand the Bank [dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing) that comes from the UCI Machine Learning reposiory. 
    * The dataset 20 columns (10 numeric and others object). 
    * It has a column which specifies if a particular direct contact call was successful or not. 
    * The dataset has no missing values. 
    * The target column is unbalanced (~88% unsuccessful)

- **Data Preparation**:
    * Cleaned data by [dropping columns](#Data Preparation 1) that provided little information or was added mainly for the purposes of benchmarking
    * Cleaned data by [dropping rows](#Data Preparation 2) by looking at columns with 'unknown' values which impact a low proportion of the total rows
    * Cleaned data by [imputing values](# Data Preparation 3) 
    * Transformed data using make_column_transformer and using OneHot and Ordinal encoders

- **Modeling**: Formed baseline for accuracy. 
    * As the target is un balanced, it makes sense to keep the baseline as ~88% because a random guessing classifier could achieve that
    * Split transformed data by assigning 33% data to test. Also, stratified the split to address the imbalance in the target column
    * Did a Baseline model and comparison of baseline results using default values for the sklearn classifier models of:
        1. Logistic Regression
        2. Decision Trees
        3. K Nearest Neighbors
        4. Support Vector Machines
    * Tuned hyper parameters for each of these classifiers using GridSearchCV tool in sklearn

- [**Evaluation**](#Model-evaluation): the classifier models were evaluated using the following metrics
    * accuracy of the model prediction on test data
    * accuracy of the model prediction on training data
    * ROC-AUC score of the models (to see how they performed with False and True positive rates)
    * Precision-Recall score of the models (by measuring the Average Precision value of the models)
    * time to train the model
    * clarity in explaination and resulting actionable conclusion from the results was considered

- Deployment: Identified actionable items for client based on results
    * best model was evaluated to be Logistic Regression with hyper parameters:
        {'lgr__C': 0.01, 'lgr__penalty': 'l2', 'lgr__solver': 'liblinear'}
    * this model resulted in test accurace of 0.902, roc-auc score of 0.79 and Average Precision (AP) of 0.46
    * Summarized Findings with Recommendataions in [Findings and Recommendations](#Findings-and-Recommendations) section
  
- Added [Future Work](#Future-work)



## Details

## Data Understanding

### Initial exploration 

Look at all the categorical columns and see if there is something actionable to prepare the data

[Add picture with data]

#### Conclusion from data exploration

From the initial exploration of data, we can drop:

    **poutcome**: it looks like poutcome column has mostly 'nonexistant' value. This does not add much relevant information to train the data on. This column can be safely dropped

    **duration**: this column is mainly for benchmarking and the guideline is this should be dropped

### Data Preparation

#### Data Prepration 1 

There are unknown values for each of the attributes which do not provide any information. Additionally, the proportion of these rows are comparatively quite small. Drop the rows which have 'unknown' values for 'job' and 'marital' columns. There is added advantage with this as some of these columns will become binary resuting in complexity (fewer features)

#### Data Preparation 2

map unknown values in default to 'no' with assumption that defaulted accounts are important and would definitely be recorded as 'yes'. This is a corner case as the number of 'yes' values is quite small (0.000075). Perhaps a candidate to drop this column

#### Data Transormation

Transform the data in the following way:

    * Use OrdinaEncoder for the education column as there is ineherent ordering in the qualifications
    * Use OneHotEncoder for \['job', 'marital', 'default','housing', 'loan','contact', 'month', 'day_of_week', 'y'\] columns
    * Fix the column names after transformation to make them more readable
    * this results in 44 columns

## Training and Test split
    * Split the data with test size 0.33 after dropping 'y' column
    * set 'y' column as target data
    * this results in 26668 rows for training data and 13135 rows for test data

## Modeling

Following modes were analyzed:

- Logistic Regression
- Decision Trees
- K Nearest Neighbor
- Support Vector Machine (SVM)

1. ### Initial model Comparisons

    LogisticRegression model with default parameters was used for simple model

        Results:
        Training time: 0.0904s, 
        Training accuracy: 0.899
        Test accuracy: 0.902

    The other models were also run with default parameters. Following data frame records the results:

      - add picture --

2. ### Improving models

#### Logistic Regression

  Ran grid search for Logistic Regression model with the following parameters:
    lgr_params = {'lgr__penalty': ['l1', 'l2'],
              'lgr__solver': ['liblinear'],
              'lgr__C': [0.001, 0.01, 0.1]}


#### Decision Trees

Ran grid search for Decision Tree with the following parameters:
    dtree_params = {
         'dtree__max_depth': range(2,11),
         'dtree__min_samples_split': [2, 4, 6],
         'dtree__criterion': ['gini', 'entropy', 'log_loss'],
         'dtree__min_samples_leaf': [1, 2, 3]
         }

#### K Nearest Neighbors

Ran grid search for knn with the following parameters:
    knn_params = {'knn__n_neighbors': list(range(1,30,2))}

#### Support Vector Machines

Ran grid search for rbf kernel with following parameters:
    svm_rbf_params = {'svc__kernel': ['rbf'],
              'svc__C': [0.001, 0.01, 0.1, 1, 10]}

Ran grid search for poly kernel with following parameters:
    svm_poly_params = {'svc__kernel': ['poly'],
              'svc__degree': [2, 3]}

Used StandardScaler with Pipeline for all of the analysis above

### Model Evaluation

#### Criteria

- Test Accuracy
- Time to train
- ROC-AUC scorewas Logistic Regression for this metric with a score of 0.79
- Precision-Recwas Logistic Regression for this metric with a score of 0.45
-  Explainability 

#### Evaluation

**Summary**
-
___________

Based on all metrics, Logistic Regression model with hyper parameters: {'lgr__C': 0.01, 'lgr__penalty': 'l2', 'lgr__solver': 'liblinear'} is the best model for this classification problem.

Following table summarizes the metrics for the classifiers with the best metric in bold

| Classifier      | training time | training accuracy     | test accuracy | roc_auc score | average precision score |
| :---        |    :----:   |    :----:   |    :----:   |    :----:   |          ---: |
| Logistic Regression      | **3.638389**  | 898568   | **0.902246** | **0.786010** | **0.456493** |
| Decision Tree   | 36.895623 |	0.899318 |	0.900952 |	0.753808 |	0.385566 |
| K Nearest Neighbor | 5.760976 |	0.899805 |	0.898515 |	0.761196 |	0.407148 |
| SVM gaussian | 728.213049	| **0.908692**	| 0.899505	| 0.715840	| 0.410711
| SVM linear | 359.736290 |	0.901117 |	0.900266 |	0.695119 |	0.406281 |

**Details**
-
___________

***Overall note***

The accuracies, both training and test did not vary much between the classifiers. Other important metrics like roc-auc score and average precision score led to a clear difference in performance. Additionally, train time for training this model was a fraction of the others which was an added benefit

**LogisticRegression**: As mentioned above logistic regression performed best in most of the metrics. It performed the best in all metrics except the training accuracy metric. The parameter grid searched over following:

    lgr_params = {'lgr__penalty': ['l1', 'l2'],
                  'lgr__solver': ['liblinear'],
                  'lgr__C': [0.001, 0.01, 0.1]}

**Decision Tree** 

Did a grid search was done over the following:

    dtree_params = {
             'dtree__max_depth': range(2,11),
             'dtree__min_samples_split': [2, 4, 6],
             'dtree__criterion': ['gini', 'entropy', 'log_loss'],
             'dtree__min_samples_leaf': [1, 2, 3]
             }
As is clear from the table, well and its scores were close to the Logistic Regression though some metrics like average precision was much lower, in fact the minimum of all the classifiers

**K Nearest Neighbor**

Did a grid search over following hyper parameters:

    knn_params = {'knn__n_neighbors': list(range(1,30,2))}

From the table below, it is clear that this classifier performed well in most metrics did not perform as well as Logistic Regression in any metric

**Support Vector with rbf (gaussian) kernel**

Did a grid search over following hyper parameters:

    svm_rbf_params = {'svc__kernel': ['rbf'],
                  'svc__C': [0.001, 0.01, 0.1, 1, 10]}

Took a long time to train (longest among all classifiers) over this grid but did not find a model better than the Logistic Regression with respect to any of the metrics except training accuracy by a very small margin

**Support Vector with polynomial kernel**

Did a grid search with following hyper parameters:

    svm_poly_params = {'svc__kernel': ['poly'],
                  'svc__degree': [2, 3]}

Took long time to train (2nd longest) over this grid and did not find a model that performed better than Logistic Regression in any of the metrics.

*Note*: tried doing a grid search with all kernels to find the best kernel and different hyperparameters. Took too much time did not complete even after running for more than 20 hours. Abandoned that approach and selectively analyzed rbf and linear kernels with different hyper parameters


## Deployment

Findings and Recommendations
-


Summary
-

The success of direct bank marketing campains was **most sensitive** to the following attributes (in the order of decreasing impact):



| Attribute name  | Description     |
| :---        |    ----:   | 
| **pdays** | number of days that passed by after the client was last contacted from a previous campaign |
| **nr.employed** | number of employees - quarterly indicator (numeric)|
|**cons.price.idx** | consumer price index - monthly indicator (numeric)|
| **emp.var.rate** | employment variation rate - quarterly indicator (numeric) |
| **contact_telephone** | contact communication type (categorical: 'cellular','telephone') |
| **euribor3m** | euribor 3 month rate - daily indicator (numeric) |
| **previous** | number of contacts performed before this campaign and for this client (numeric) |
| **month_jul**| last contact month of year was July |
| **month_mar**| last contact month of year was March |
| **job_retired** | people who are retired |
| **month_oct** | last contact month of year was October |

_____

**Top 10 attributes** that had the most **postive** impact on the success of campaigns, meaning put effort to create conditions/actions increase these metrics (order of decreasing impact):



| Attribute name  | Description     |
| :---        |    ----:   | 
| **cons.price.idx**|  consumer price index - monthly indicator (numeric)| 
| **month_mar**|  	last contact month of year was March (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')| 
| **month_jul**| 	last contact month of year was July (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')| 
| **cons.conf.idx**|  consumer confidence index - monthly indicator (numeric)| 
| **month_jun**|  last contact month of year was June (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')| 
| **job_retired**|  people who are retired| 
| **job_student**| people who are students| 
| **day_of_week_wed**|  last contact day of the week was Wednesday| 
| **month_apr**| last contact month of year was April | 
| **job_admin.**|  people who have admin jobs | 


___

**Top 10 attributes** that had most **negative** impact on the success of the campaigns, meaning put effort to create conditions/take actions to decrease these metrics (order of decreasing impact):



| Attribute name  | Description     |
| :---        |    ----:   | 
| **emp.var.rate**|  employment variation rate - quarterly indicator (numeric)| 
| **nr.employed**|  number of employees - quarterly indicator (numeric)| 
| **pdays**| number of days that passed by after the client was last contacted from a previous campaign | 
| **contact_telephone**| communicated over telephone (instead of cellular)| 
| **month_may** | last contact was May| 
| **previous**| number of contacts performed before this campaign and for this client (numeric)| 
| **euribor3m**|  euribor 3 month rate - daily indicator (numeric)| 
| **campaign**| number of contacts performed during this campaign and for this client (numeric, includes last contact)| 
| **day_of_week_mon**| last contact day of the week was Monday| 
| **job_blue-collar**|  blue collar job| 

___

Details
-



In addition to the top 10 attributes, the following **attributes** also led to **positive** results in the campaign (in decreasing order of impact)

**education**: Higher education led to more success

**month_aug****: More calls in month of August were successful

**day_of_week_thu**: Calling on thursdays led to more successful calls

**month_dec**: calling in the month of december led to more successful calls

**marital_single**: campain was more successful with single people

**loan_yes**: campaign was more successful with people who had loans

**month_oct**: campaigns were more successful in the month of octoer

**job_management**: campaigns were more successful with people in management jobs

**day_of_week_tue**: campaigns were more successful when contacted on Tuesdays and Fridays

**job_entrepreneur**: campaigns were more successful with entrepreneurs

**age**: campaigns were more successful with people with higher age

**job_technician**: campaigns were more successful with people in technician jobs

**job_unemployed**: campaigns were successful with unemployed

**marital_married**: campaigns were more successful with married people

___

In addition to the top 10 negative attributes, the following **attributes** led to **negative impact** on the outcome of the campaign (in decreasing order of impact) :

**month_nov**: calling in the month of december led to fewer successful calls

**marital_divorced**: campaigns were less successful with divorced people

**job_services**: campaigns were less successful with people in services industry 

**job_housemaid**: campaigns were less successful with housemaids

**housing_yes**: campaigns were less successful with people how had housing

**job_self-employed**: campaigns were less successful with people who were self-employed

**default_yes**: campaigns were less successful with people who had defaulted

**month_sep**: campaigns were less successful in the month of september


## Future work

Identified the following work for future:

1. The grid search for the models done was very basic. Do some more research (especially with sigmoid and linear kernels of svm to find a better model)
2. SVM model training takes too long. Research any public literature on how to speed this up
3. Was expecting Decision Trees to do better than how it performed. Research more on how to optimize this model to do better
4. In the recommendation section, it would be good to find the actual numbers to help the banks. For example, would be good to know how the campaigns performed with a particular age group
