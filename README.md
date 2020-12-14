# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
This dataset contains data pertaining to direct marketing campaigns of a banking institution. The marketing campaigns were based on phone calls, to convince potenitial clients to subscrobe to bank's term deposit.  we seek to predict to predict whether the potential client would accept to make a term deposit at the bank or not.


The best performing model found using AutoML was a Voting Ensemble with 91.6% accuracy, while the accuarcy of Logistic classifier implemented using hyperdrive was 90.7%

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

* The data is accessed from url - "https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv", and a Tabular Dataset is created using TabularDatasetFactory 

* Data is then cleaned and pre-processed using clean_data function, this includes Binary encoding and One Hot Encoding of categorical features

* Data is then split into 80:20 ratio for training and testing

* Define a Scikit-learn based Logistic Regression model and set up a parameter sampler. We define 2 hyperparameters to be tuned, namely C and max_iter. C represents the inverse regularization parameter and max_iter represents the maximum number of iterations

* HyperDrive configuration is created using a SKLearn estimator and parameter sampler

* Accuracy is calculated on the test set for each run and the best model is saved

**What are the benefits of the parameter sampler you chose?**

Performed random sampling over the hyperparameter search space using RandomParameterSampling in our parameter sampler, this drastically reduces computation time and we are still able to find reasonably good models when compared to GridParameterSampling methodology where all the possible values from the search space are used, and it supports early termination of low-performance runs

**What are the benefits of the early stopping policy you chose?**

BanditPolicy is used here which is an "aggressive" early stopping policy. It cuts more runs than a conservative policy like the MedianStoppingPolicy, hence saving the computational time significantly. Configuration Parameters:-

* slack_factor/slack_amount : (factor)The slack allowed with respect to the best performing training run.(amount) Specifies the allowable slack as an absolute amount, instead of a ratio. Set to 0.1.

* evaluation_interval : (optional) The frequency for applying the policy. Set to 1.

* delay_evaluation : (optional) Delays the first policy evaluation for a specified number of intervals. Set to 5.



## AutoML

AutoML provided the ability to run multiple experiments and choose best clasfication model. Overall 15 classification models were run as can be seen in the below snapshot. Voting Ensemble algorithm proved to be the best model with an accuracy of 91.6%

![Screenshot (313)](https://user-images.githubusercontent.com/6285945/102023134-5933ba00-3db1-11eb-9f83-c44c6b94a6cf.png)

## Pipeline comparison

* With AutoML the accuracy of best model (Voting Ensemble) was 91.6% and accuracy of HyperDrive model (Logistic Classifier) was 90.7% 

* Using AutoML one could try several classification models, this activity if performed using HyperDrive will entail some effort as it would require configuring pipeline for each model.

## Future work

* While running AutoML pipeline **Class Imbalance** alert was generated, this is one of the future improvements that should be implemented. We can look at different ways to combat class imbalance, such as - resampling training data, generate synthetic samples using SMOTE or the Synthetic Minority Over-sampling Technique, etc.

* Look at other performance metric such as Precision, Recall and F1 Score as Accuracy metric can be misleading while working with class imbalanced dataset 

## Proof of cluster clean up

![Screenshot (316)](https://user-images.githubusercontent.com/6285945/102023095-1f62b380-3db1-11eb-8ddf-c46b3ff9a644.png)

![Screenshot (315)](https://user-images.githubusercontent.com/6285945/102023108-343f4700-3db1-11eb-902d-c3a17af464a0.png)
