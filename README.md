# Overview

This README has the study that compares different classifiers to analyze attributes that contribute to success of directed bank marketing campaigns. Success is defined as the campaign resulting in a successful subscription of a deposit by the targeted client. The approach was to follow CRISP-DM framework to find the best classifier model to classify if a targeted campain contact to a client would be successful or not. 

Link to the jupyter notebook: [Assignment 17_1 Jupyter notebook](https://github.com/raosalapaka/ml-module17/blob/main/assignment17_1_RS.ipynb)

Link to github: [Github](https://github.com/raosalapaka/ml-module17)

Link to the repository: [Git repository](https://github.com/raosalapaka/ml-module17.git)

---
# **Comparing Classifiers**

## **Summary**

Following CRISP-DM framework to do the analysis.

- Cleaned data by [dropping columns](#Initial-exploration) that provided little information
- Transformed data using make_column_transformer and using OneHot and Ordinal encoders
- Formed baseline for accuracy. As the target is un balanced, it makes sense to keep the baseline as ~88% because a random guessing classifier could achieve that
- [Evaluated](#Model-evaluation) multiple models: logistic regression, decision tree, K nearest neighbor and support vector machines
- Found the best model to be 
- Summarized findings 
- Added [Future Work](#Future-work)



## Details

## Data Exploration

### Initial exploration 

### Further exploration

### Further exploration (conclusion)

### Final remark on data exploration

### Data Preparation

## Training and Test split

## Modeling

Following modes were analyzed:

1. ### Initial model Comparisons
    
2. ### Improving models

#### Logistic Regression

#### Decision Trees

#### K Nearest Neighbors

#### Support Vector Machines

Used StandardScaler with Pipeline for all of the analysis above

### Model Evaluation

### Criteria

#### Accuracy

#### Time to train

#### ROC-AUC score

#### Precision-Recall (Average Precision value)

#### Explainability 

### Summary table


Discussion:


## Feature analysis with best model


### Deployment


**Findings/Recommendations**

**Summary**

**Following features impact target prices *negatively*** (decreases price):


**Detailed Report**

After evaluating multiple models, the following features of the car were found to be **statistically significant** with respect to the car price (in decreasing order of siginificance). This does not specify if the impact was positive or negative just that these features were important the the target price:

## Future work
