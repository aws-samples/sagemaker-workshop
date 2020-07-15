+++
title = "Tools & Knowledge Check"
chapter = false
weight = 2
+++

To work through these labs you will need:

 - **An AWS account**

     With privileges to create IAM roles, attach IAM policies, create AWS VPCs, configure Service Catalog, create Amazon S3 buckets, and work with Amazon SageMaker.

 - **Access to the [AWS web console](https://console.aws.amazon.com/console/home)**

     Many of the instructions will guide you through working with the various service consoles.  

To get the most of these labs it will be beneficial if you have prior experience working with the following technologies:

 - **[Python](https://www.python.org/)**

   Python is a programming language that is popular in data science communities.  It has been used in the labs to work with the AWS services and the data being used to train machine learning models.

 - **[Git](https://git-scm.com/)**

   The labs make use of Git to manage the work you will complete.  Git is a distributed version control system and you will use a few simple commands to interact with a Git repository during the labs.

 - **[SageMaker SDK](https://sagemaker.readthedocs.io)**

   SageMaker SDK is a high-level Python SDK wrapped around Boto3 and designed to provide a familiar interface to data science users.

 - **[Boto3](https://boto3.readthedocs.io/)**

   Boto3 is a low-level Python SDK for interacting with the AWS APIs.  Documentation on its many great classes and functionality can be found [online](https://boto3.readthedocs.io/)

 - **[Pandas](https://pandas.pydata.org/)**

   The notebooks use Pandas in many different places to load, export, and manipulate data.  If you are unfamiliar with the Pandas library it may be helpful to review some of their [Getting Started](https://pandas.pydata.org/docs/getting_started/index.html) materials.

 - **[scikit-learn](https://scikit-learn.org)** 
 
   scikit-learn is a popular open source framework for data science and machine learning.

 - **[XGBoost](https://xgboost.readthedocs.io)** 
 
   xgboost is one of the most popular and performant gradient boosting algorithms for supervised learning tasks.

 - **[Jupyter](https://jupyter.org/)** 
 
   Jupyter is a popular open source interactive computing environment with a user friendly notebook interface.  
   
   You will use Jupyter notebooks to complete these labs.  If you have not used [Jupyter](https://jupyter.org/) before you may find a [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) to be useful.  The cheat sheet walks through navigation of the Jupyter interface and how to use a notebook.
   
  - **[SageMaker Experiments](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)** 
  
    SageMaker Experiments APIs can be used to manage and track the metadata for your training, pre-processing, hyperparameter tuning jobs.
   
  - **[SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html)** 
  
    SageMaker ModelMonitor can be used to detect data drift during inference time on the payload sent to your endpoint. It can be connected to CloudWatch to send an alarm or notification when violations are detected and users need to be alerted.
   
  - **[SageMaker Processing](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_processing.html)** 
  
    SageMaker Processing can be used to run your scripts for pre-processing, feature engineering in a managed way where Amazon SageMaker sets up the underlying infrastructure needed to run your job at scale, tearing down the instances once the job is complete.

---

Now, let's get started!
