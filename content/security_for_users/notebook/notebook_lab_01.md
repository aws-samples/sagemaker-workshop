+++
title = "Notebook 01: Data Science Workflow"
chapter = false
weight = 2
+++

With your notebook provisioned, from the Service Catalog Provisioned Products console you can click on the NotebookUrl link in the product detail for your notebook to access the Jupyter notebook server.

![Provisioned Notebook Product](/images/catalog_notebook_url.png)

Clicking the NotebookUrl link will redirect your browser to the JupyterLab environment.  Before you open the notebook kernel `01_SageMaker-DataScientist-Workflow.ipynb` lets take a brief moment and review the contents of the notebook and the key takeaways for this lab.

## Objectives

Security is everyone's responsibility.  The cloud infrastructure team have taken steps to protect data and enable data science teams but equally the data science teams, especially teams in regulated industries, have a responsibility to operate in a secure fashion.  To do this it helps to follow best practices in 8 key areas.  These areas are:

1. **Compute and Network Isolation**

    In working with the Amazon SageMaker service you will need to configure training jobs and similar resources using practices inline with security policy.  This means provisioning resources in the same secure network environment in which your JupyterLab server is operating.  This lab will show you how to configure SageMaker to provision resources in accordance with your policies.

1. **Authentication and Access Management**

    An AWS IAM role has been provisioned for you and for the resources you create, this lab will show you how to pass that role on to Amazon SageMaker for use when acting on your behalf.

1. **Artifact Management**

    Your team has been provided with resources and technology.  Your team will now produce artifacts such as features derived from data, notebooks to explore and conduct experimentation, algorithms to produce ML models, and other forms of intellectual property.  This lab will show you how to manage these assets using services like Amazon S3 and an AWS CodeCommit Git repository. 

1. **Data Encryption**

    You will learn how to ensure that all training/processing volumes are passed appropriate encryption keys to ensure data is encrypted end to end, in transit and at rest. Also we will see how to use network configurations to ensure that data is encrypted in transit and doesn't traverse the internet.

1. **Traceability and Auditability**

    You will learn how to use SageMaker Experiments to easily maintain the lineage of your trained models and training jobs. SageMaker Experiments will automatically manage the metadata associated with your training jobs and make it easily accessible via a Pandas DataFrame. 

1. **Explainability and Interpretability**

    There are different methods, tools, and techniques to explain an ML model's inference.  In this lab you will use SHAP values to derive feature importances and gain insight into how your model is weighting different inputs to produce a prediction.

1. **Real Time Model Monitoring**

    Once a model has been produced ensuring that it is producing predictions that are still accurate and relevant for the current state of the world is a must to avoid drift in original accuracy, F1 scores, or other objective metrics.  In this lab you will learn how to monitor your production endpoints in real time to detect concept drift and alert when inputs to your model or its outputs are no longer in keeping with the model's training sets.

1. **Reproducibility**

    For audit reasons, you may need to reproduce your work at a later time. To ensure reproducibility, you will see how to use SageMaker experiments in combination with CodeCommit to version your code and track the entire lineage of your models.

## The notebook explained

`01_SageMaker-DataScientist-Workflow.ipynb` will cover a typical Data Scientist workflow of data exploration, model training, extracting model feature importances and committing your code to Git. 

You will look at a credit card dataset to predict whether a customer will default on their credit card payments based on prior payment data. This is a binary classification problem and you will train a XGBoost model using an Amazon managed container and explore feature importances for that model.

Throughout this notebook you will keep network traffic within your private VPC and enforce encryption at rest and in transit. Furthermore, you will learn how to use SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save up to 90% on training costs.

Finally you will commit the code to our CodeCommit repository and demonstrate code versioning, tagging and source control. 

---

After you have completed the notebook please proceed to [Notebook 02: DevOps Workflow](notebook_lab_02.html) where you will deploy and monitor the trained model.
