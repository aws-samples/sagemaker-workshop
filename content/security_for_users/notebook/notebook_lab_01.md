+++
title = "Overview of Notebook 01"
chapter = false
weight = 2
+++

01_SageMaker-DataScientist-Workflow.ipynb will cover a typical Data Scientist workflow of data exploration, model training, extracting model feature importances and committing your code to Git. 

Here we look at a credit card dataset to predict whether a customer will default on their credit card payments based on prior payment data. This is a binary classification problem and we will train a XGBoost model using an Amazon managed container and explore feature importances for that model.

Throughout we will maintain network traffic via our private VPC and enforce encryption at rest and in transit. Furthermore, you will learn how to using SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save upto 90% on training costs.

Finally we will commit the code to our CodeCommit repository and demonstrate code versioning, tagging and source control. Once the model is trained proceed to 02_SageMaker-DevOps-Workflow.ipynb where we will deploy and monitor the model. 
