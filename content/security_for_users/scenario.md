+++
title = "Scenario"
chapter = false
weight = 1
+++

You are a data scientist or ML engineer who works at a company that wishes to enable their data scientists to deliver machine learning-based projects that are trained on highly sensitive company data.  The project teams are constrained by shared on-premise resources so your sysops admins have created all the infrastructure-as-code templates needed to provision a secure environment, which protects the sensitive data while also enabling the data science teams to self-service.

---

You as a data scientist or engineer will need to quickly set this environment up for yourself, so you can start working on exploring your data, training, deploying and monitoring your models. The key takeaways of this workshop are:

1. **Compute and Network Isolation**

    Deploy Amazon SageMaker in a secure, customer-managed VPC.

1. **Authentication and Authorization**

    Provide single user access to Jupyter over IAM.

1. **Artifact Management**

    Enable private Git integration, lifecycle config, and versioning.

1. **Data Encryption**

    Encrypt data in motion and at rest across the entire ML workflow.

1. **Traceability and Auditability**

    Trace model lineage, and audit all API calls and data events.

1. **Explainability and Interpretability**

    Explain predictions with feature importance and SHAP values.

1. **Real-time Model Monitoring**

    Monitor the performance of a productionized model.

1. **Reproducibility**

    Reproduce the model and results based on saved artifacts.

In these notebooks you will see some recommended practices on how to implement these requirements using Amazon SageMaker. Note that while these are recommended practices and guidelines, the information included in these notebooks is for illustrative purposes only. Nothing in this notebook is intended to provide you legal, compliance, or regulatory guidance. 

The specific features and functionalities that you will become familiar with are:

 - **Importing custom libraries using pip** without having public internet connectivity

 - Training a model with and without VPC and implementation of preventative controls to avoid training without VPC.

 - Importing networking configurations, KMS keys directly in the notebook without data scientist having to know what they are.

 - Using **SageMaker Processing** to run scikit-learn data pre-processing jobs in Python.

 - Using **SageMaker training on spot instances** to save on cost

 - Model Explainability using SHAP

 - Pushing/pulling code to private AWS CodeCommit Repository

 - Deploying a trained model and monitoring it for data drift with **SageMaker ModelMonitor**

 - Securely running training, processing jobs using KMS keys to ensure encryption at rest and PrivateLink to support encryption in transit.

 - Using **SageMaker Experiments** to maintain lineage and traceability of model artifacts.

---

In the following labs you will quickly provision a collection of mechanisms and guard rails to enable you, as a project team member, to provisoin infrastructure and tools to support your project.  You will then work through the machine learning lifecycle in the secure environment to see how project teams can be enabled to work in an agile manner at speed.
