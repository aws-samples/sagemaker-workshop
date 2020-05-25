+++
title = "Lab 2: Secure Environment"
chapter = false
weight = 11
+++

The data science team have requested a cloud environment to complete their project.  The project administrator will use the Service Catalog portfolio, managed by the Cloud Platform Engineering team, to provision a secure VPC and related resources for the data science team.  In this lab, you will use Amazon Service Catalog to provision this data science environment.  Following the steps below create an environment which contains:

 - AWS VPC with no IGW
 - VPC endpoints to Amazon S3, Amazon SageMaker, CloudWatch, STS
 - IAM roles for the data science administrator and the data scientist
 - Store AWS resource names/identifiers in AWS SSM Parameter Store for future reference
 - Service Catalog Portfolio of products for the data science users
 - AWS KMS key for encrypting data at rest

## Deploy a project environment

Using the Amazon Service Catalog locate and launch a Data Science Project.

{{% notice info %}}
If you wish to see the contents of these CloudFormation templates you can view them on the CloudFormation console or copy them locally for review using a command such as the below.
{{% /notice %}}

```bash
aws s3 sync s3://sagemaker-workshop-cloudformation-us-east-1/quickstart ./sagemaker-workshop-cloudformation
```

---
## Review base environment

After the CloudFormation stack has been successfully created review the **Resources** tab of the CloudFormation stack and the resources that were created.  You'll notice that it has provisioned:

 - **Service Catalog Portfolio**

     A [service catalog portfolio](https://console.aws.amazon.com/servicecatalog/home) and products have been configured to give the data science teams tailored products they can deploy easily.

 - **IAM roles**

     [Roles and permissions](https://console.aws.amazon.com/iam/home) have been created so that the data science teams can manage themselves and create the resources they need.

 - **SSM parameters**

     A collection of parameters have been stored so they can be referenced by the data science teams.  [Visit the console](https://console.aws.amazon.com/systems-manager/parameters), what parameters have been created?

 - **KMS key**

     A KMS key to encrypt data at rest in the data science environment.  [Visit the console](https://console.aws.amazon.com/kms/home), what is the KMS key being used to encrypt?

 - **Virtual Private Cloud (VPC)**

     The template has created a VPC with no Internet connectivity but with VPC endpoints for accessing AWS services like Amazon S3 and Amazon SageMaker.  [Visit the console](https://console.aws.amazon.com/vpc/home), what services are accessible from within the VPC?  Do any endpoints have Endpoint Policies governing them?

---

You have now created a secure environment for the data science team. Lets now hand things back to the project team and let them support themselves.
