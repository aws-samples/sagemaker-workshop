+++
title = "Scenario"
chapter = false
weight = 1
+++

You are a data scientist or ML engineer who works at a company that wishes to enable their data scientists to deliver machine learning-based projects that are trained on highly sensitive company data.  The project teams are constrained by shared on-premise resources so your sysops admins have created all the infrastructure-as-code templates needed to provision a secure environment, which protects the sensitive data while also enabling the data science teams to self-service.

You as a data scientist or engineer will need to quickly set this environment up for yourself, so you can start working on exploring your data, training, deploying and monitoring your models. The key takeaways of this workshop are illustrated in the diagram below:

![Workshop takeaways](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/sec-takeaways.png)

In these notebooks we will see some best practices on how to implement these requirements using Amazon SageMaker. Note that while these are best practices and guidelines, the information included in these notebooks is for illustrative purposes only. Nothing in this notebook is intended to provide you legal, compliance, or regulatory guidance. 

The specific features and functionalities that you will become familiar with are:

 - **Importing custom libraries using pip** without having public internet connectivity

 - Training a model with and without VPC and implementation of preventative controls to avoid training without VPC.

 - Importing networking configurations, KMS keys directly in the notebook without data scientist having to know what they are.

 - Using **SageMaker Processing** to run scikit-learn data pre-processing jobs in Python.

 - Using **SageMaker training on spot instances** to save on cost

 - Model Explainability using SHAP

 - Pushing/pulling code to private AWS CodeCommit Repository

 - Deploying a trained model and monitoring it for data drift

 - Securely running training, processing jobs using KMS keys to ensure encryption at rest and PrivateLink to support encryption in transit.

 - Using **SageMaker Experiments** to maintain lineage and traceability of model artifacts.






Let's get started!
---

During this series of labs, you will be creating a secure environment for a team of data scientists with self-service tools to manage the environment and deliver an ML project.

There are three roles involved across these labs:

 - **Cloud Platform Engineering** - responsible for managing the cloud environments 
 - **Data Science Administrator** - responsible for managing resources to support the data science teams
 - **Data Scientist** - a member of a project team tasked with delivering an ML or data science project

These 3 roles will work together to create a secure cloud environment with appropriate guard rails, provision a secure data science environment, and deliver an ML project working with sensitive data.  You will start as a member of the **cloud platform engineering** team to build a secure network and provision a service catalog for use by the **data science administration** team.  Then, as a **data science administrator**, you will use the service catalog along with Cloudformation to provide the **data science** team with the tools they need to deliver their project.  Finally, as a member of the **data science** team, you will self-service and provision a Notebook server.  Using that server you will then develop and train a model while exploring the security controls in the data science environment.

To do this, the roles will work together to configure environments and iterate to improve the security posture across 5 labs.

 - **Lab 1: Deploy the base infrastructure**

     As the cloud platform engineer, create a VPC with no Internet connectivity and an AWS Service Catalog portfolio to support the data science administration team.

 - **Lab 2: Deploy the first version of the team resources**

     As a data science administrator, create an IAM role for the data science team and provision a Service Catalog portfolio so the data scientists can self-service.

 - **Lab 3: Deploy an Amazon SageMaker notebook**

     As a data scientist, use Service Catalog to deploy an Amazon SageMaker notebook.
