+++
title = "Scenario"
chapter = false
weight = 1
+++

You work at a company that wishes to enable their data scientists to deliver machine learning-based projects that are trained on highly sensitive company data.  The project teams are constrained by shared on-premise resources so you have been tasked with determining how the business can leverage the cloud to provision environments for the data science teams.  The environment must be secure, protecting the sensitive data while also enabling the data science teams to self-service.

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

 - **Lab 4: Create a training job in line with security policy**

     As a data scientist, observe your training job's performance as security controls respond to incorrect configuration parameters.

 - **Lab 5: Improve security controls**

     As the data science administrator, alter the IAM policies governing the data science environment to deliver preventive controls to guard your sensitive data.

Next, let's review the tools you'll need to complete these labs.