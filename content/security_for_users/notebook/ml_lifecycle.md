+++
title = "Security Objectives"
chapter = false
weight = 10
+++

A secure environment is an enabler for teams, allowing them to focus on their project and the business challenge it is trying to solve.  From the perspective of a project team the environment is intended to support the team in achieving the following objectives:

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

---

In the following labs you will work through Jupyter notebooks which show you how to support the above objectives using the environment that has been provisioned for you.

In the first lab you will work through a notebook called `01_SageMaker-DataScientist-Workflow.ipynb` which will cover a typical Data Scientist workflow and show you how to explore data, pre-process data, train an XGBoost model using an Amazon Managed container and explore feature importances for that model in a secure manner, maintaining network traffic within your team's private VPC and enforing encryption at rest and in transit. Furthermore, you will learn how to use SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save on training costs.

In the second notebook, `02_SageMaker-DevOps-Workflow.ipynb`, you will deploy the trained model from the SageMaker notebook to production and monitor the endpoint for data drift using ModelMonitor. Finally you will use SageMaker Experiments to track any model metadata, such as the Git commit ID associated with the algorithm, to trace the lineage of our models and endpoints.
