+++
title = "Lab 03: Data Science Workflow"
chapter = false
weight = 20
+++

The ML lifecylce has many stages and steps, and often requires revisiting previous steps as you tune your model.  This lab is intended to highlight how your project team can work through the ML lifecycle and achive the objectives outlined earlier through supporting services such as experiment tracking.

---

## The notebook explained

`01_SageMaker-DataScientist-Workflow.ipynb` will cover a typical Data Scientist workflow of data exploration, model training, extracting model feature importances and committing your code to Git. 

You will look at a credit card dataset to predict whether a customer will default on their credit card payments based on prior payment data. This is a binary classification problem and you will train a XGBoost model using an Amazon managed container and explore feature importances for that model.

Throughout this notebook you will keep network traffic within your private VPC and enforce encryption at rest and in transit. Furthermore, you will learn how to use SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save up to 90% on training costs.

Finally you will commit the code to our CodeCommit repository and demonstrate code versioning, tagging and source control. 

---

After you have completed the notebook please proceed to [Lab 04: DevOps Workflow](notebook_lab_02.html) where you will deploy and monitor the trained model.
