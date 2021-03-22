---
title: "[Kaggle Study] Titanic - Machine Learning from Disaster"
category: Kaggle
use_math: true
---

I started Kaggle Study alone.

I will write most of my writing in English to become familiar with English. 

The first competition is <a href="https://www.kaggle.com/c/titanic">'Titanic - Machine Learning from Disaster'</a>, It's very famous competition.

The kernel I'm currently studying is <a href="https://www.kaggle.com/ash316/eda-to-prediction-dietanic">'EDA To Prediction(DieTanic)'</a>.

Today I learned(TIL) will be updated at <a href="">my kernel</a>.

## TIL

### Sun Mar 14
1. Check competition overview and contents of the kernel.
2. Check type of features
    - Categorical Features : Sex, Embarked
    - Ordinal Features : Pclass
    - Continuous Features : Age
3. Analysis a categorical feature('Sex'), a ordinal feature('Pclass') and a continuous feature('Age').
4. Make a new categorical feature('Initial') from 'Name'.
5. Fill continuous data('Age').

### Mon Mar 15
1. Analysis categorical feature('Embarked').
2. Fill categorical data('Embarked').

### Tue Mar 16 ~ Wed Mar 18
- I Studied another topics.

### Fri Mar 19
1. Review that I studied up to now.

### Sat Mar 20
1. Analysis two discrete features('SibSip', 'Parch').
2. Pracitce Feature Enginnering for categorical values(Sum three categorical values('Pclass', 'Sex', 'Embarked') -> Make a new feature).
3. Analysis a continuous feature('Fare').
4. Summary Observations.
5. Check correlation between the features(Heatmap)
6. Feature Engineering and Data Cleaning
   - Binning 'Age' Features -> 'Age_band'
   - Summation 'Parch' and 'SibSp' -> 'Family_Size'
   - 'Family_Size' == 0 -> 'Alone'
   
### Sun Mar 21
1. Feature Engineering and Data Cleaning
   - 'Fare' -> 'Fare range' -> 'Fare_cat'
2. Label Encoding
   - 'Sex', 'Embarked', 'Initial'
3. Drop unneeded features
   - 'Name', 'Age', 'Ticket', 'Fare', 'Cabin', 'Fare_Range', 'PassengerId'
4. Heatmap
5. Predicitve Modeling
   - Logistic Regression
   - Support Vector Machines(Linear and radial)
   - Random Forest
   - K-Nearest Neighbours
   - Naive Bayes
   - Decision Tree
   - Logistic Regression
   
### Mon Mar 22
1. Cross Validation
   - K-Fold
   - Confusion Matrix
2. Hyper-Parameters Tuning
   - GridSearchCV
   - Ensembling
      - Voting Classifier