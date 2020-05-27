+++
title = "Overview of Notebook 01"
chapter = false
weight = 2
+++

Now that your notebook environment is provisioned, click on the NotebookUrl link in the service catalog product as shown below.

![Provisioned Notebook Product](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/provisioned_notebook.png)

This will launch a Jupyter lab environment. Before jumping into the notebook, let's take a look at the key takeaways from this workshop.

As a data scientist working in a regulated industry such as financial services, your Cloud Platform engineering team has created a secure environment for you. That said, data scientists need to ensure that their notebooks follow best practices in 8 key areas:

1. **Compute and Network Isolation**

The platform team has already done this for us.

2. **Authentication and Access Management**

The platform team has already provisioned a role for you, we will look at it in more detail in the notebooks. 

3. **Artifact Management**

It is up to the data scientist to ensure that the notebooks, data is being stored in the proper buckets that have been provisioned for us. Bucket access policies and security has been set by the platform team so data scientists can focus on the science.

4. **Data Encryption**

You will learn how to ensure that all training/processing volumes are passed appropriate encryption keys to ensure data encryption at rest. Also we will see how to use Network configurations to ensure that data is encrypted in transit and doesn't traverse the internet.

5. **Traceability and Auditability**

You will learn how to use SageMaker Experiments to easily maintain the lineage of your trained models and training jobs. SageMaker Experiments will automatically manage the metadata associated with your training jobs and surface it in a data frame. Within SageMaker Studio, this is natively integrated into the Studio UI.

6. **Explainability and Interpretability**

Once you train your ML model, we will use SHAP values to derive feature importances. 

7. **Real Time Model Monitoring**

You will learn how to monitor your production endpoints in real time for data drift

8. **Reproducibility**

For audit reasons, you may need to reproduce your work at a later time. To ensure reproducibility, you will see how to use SageMaker experiments in combination with CodeCommit to version your code and track the entire lineage of your models.

Now let's get started.

01_SageMaker-DataScientist-Workflow.ipynb will cover a typical Data Scientist workflow of data exploration, model training, extracting model feature importances and committing your code to Git. 

Here we look at a credit card dataset to predict whether a customer will default on their credit card payments based on prior payment data. This is a binary classification problem and we will train a XGBoost model using an Amazon managed container and explore feature importances for that model.

Throughout we will maintain network traffic via our private VPC and enforce encryption at rest and in transit. Furthermore, you will learn how to using SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save upto 90% on training costs.

Finally we will commit the code to our CodeCommit repository and demonstrate code versioning, tagging and source control. Once the model is trained proceed to 02_SageMaker-DevOps-Workflow.ipynb where we will deploy and monitor the model. 
