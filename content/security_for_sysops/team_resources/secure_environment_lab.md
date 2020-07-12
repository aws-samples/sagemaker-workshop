+++
title = "Lab 2: Secure Environment"
chapter = false
weight = 11
+++

A data science project team have requested a cloud environment to begin their project.  As a project administrator you will use the Service Catalog portfolio, managed by the Cloud Platform Engineering team, to provision a secure VPC and related resources for the data science team.  In this lab, you will use AWS Service Catalog to provision this data science environment.  By following the steps below you will create an environment which contains:

 - AWS VPC with no IGW
 - VPC endpoints to Amazon S3, Amazon SageMaker, CloudWatch, STS
 - IAM roles for the project's data science administrator and the data scientists
 - Store AWS resource names/identifiers in AWS SSM Parameter Store for future reference
 - Service Catalog Portfolio of products for the data science team
 - AWS KMS key for encrypting data at rest
 - Amazon S3 buckets dedicated to the project
 - Amazon CodeCommit Git repository for project source code

## Deploy a project environment

In the previous lab you deployed a CloudFormation template which created a project administrator role.  Details of this role can be found in the CloudFormation Outputs for the stack you deployed.

After assuming the project administrator role, visit the AWS Service Catalog console and provision a project environment for the data science team.

Environment creation will take approximately 10 minutes.

{{% expand "Step-by-step instructions" %}}
1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home#/stacks) and select the stack you deployed in the previous lab.
1. Click the `Outputs` tab on the stack's detail page and notice the `AssumeProjectAdminRole` hyperlink to assume the project administrator role created by the stack.
1. Click the hyperlink to assume the project administrator role.
1. On the resulting screen leave the values unchanged and click `Switch Role`.

**As the project administrator:**
1. Visit the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog/home#/products).
1. Click the context menu button next to the product `Data Science Project Environment` and click `Launch product`.
1. Give the product deployment a name, such as `example-project-dev-environment` and click `Next`.
1. Provide a `ProjectName` such as `example-project`.
1. You can leave the remaining values unchanged if you like.
1. Click `Next` through the next few screens to get to the `Review` page.
1. Click `Launch`.

{{% /expand %}}

{{% notice info %}}
If you wish to see the contents of these CloudFormation templates you can view them on the CloudFormation console or copy them locally for review using a command such as the below.
{{% /notice %}}

```bash
aws s3 sync s3://sagemaker-workshop-cloudformation-us-east-1/quickstart ./sagemaker-workshop-cloudformation
```

---
## Review base environment

After the CloudFormation stack has been successfully created review the **Resources** tab of the CloudFormation stack and the resources that were created.  You'll notice that it has provisioned:

 - **Project-specific Service Catalog Portfolio**

     A [service catalog portfolio](https://console.aws.amazon.com/servicecatalog/home) and products have been configured to give the data science team a tailored set of products they can deploy easily.

 - **IAM roles**

     [Roles and permissions](https://console.aws.amazon.com/iam/home) have been created so that the data science teams can manage themselves and create the resources they need.

 - **SSM parameters**

     A collection of parameters have been stored so they can be referenced by the data science teams.  [Visit the console](https://console.aws.amazon.com/systems-manager/parameters), what parameters have been created?

 - **KMS key**

     A KMS key to encrypt data at rest in the data science environment.  [Visit the console](https://console.aws.amazon.com/kms/home), what is the KMS key being used to encrypt?

 - **Virtual Private Cloud (VPC)**

     The template has created a VPC with no Internet connectivity but with VPC endpoints for accessing AWS services like Amazon S3 and Amazon SageMaker.  [Visit the console](https://console.aws.amazon.com/vpc/home), what services are accessible from within the VPC?  Do any endpoints have Endpoint Policies governing them?

---

You have now created a secure environment for the data science team. Now hand things over to the project team and let them support themselves.
