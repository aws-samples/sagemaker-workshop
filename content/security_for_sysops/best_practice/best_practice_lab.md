+++
title = "Lab 1: Best Practice as Code"
chapter = false
weight = 25
+++

As the cloud platform engineering team begin by deploying a shared services VPC which will host a PyPI mirror of approved Python packages, for consumption by data science project teams.  Next create a Service Catalog Portfolio which the project administrators can use to easily deploy data science environments in support of new projects.

{{% notice info %}}
This lab assumes other recommended security practices such as enabling AWS CloudTrail and capturing VPC Flow Logs.  The contents of this lab focus soley on controls and guard rails directly related to data science resources.
{{% /notice %}}

---

## Enable the project administrators

As a cloud platform engineering team member deploy the CloudFormation template linked below to provision a shared service VPC and Service Catalog portfolio.

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

---
## Review team resources

You have now created two AWS resources to support the project administration team.  Please take a moment and review these resources and their configuration.

- **Amazon S3 buckets for training data and trained models**

    [Visit the S3 console](https://console.aws.amazon.com/s3/home) and see the Amazon S3 buckets that have been created for the team.  Take note of the bucket policy that has been applied to the data bucket.

- **AWS KMS key for encrypting data at rest**

    A KMS key to encrypt data at rest in the data science environment. [Visit the console](https://console.aws.amazon.com/kms/home?#/kms/home), who is allowed to take what actions on the keys created?

- **Parameters added to Parameter Store**

    A [parameter](https://console.aws.amazon.com/systems-manager/parameters) has been added to the collection in Parameter Store.  Can you see what parameter has been added?  How would you use this value?

- **Service Catalog Jupyter Notebook product**

    A [Service Catalog Portfolio](https://console.aws.amazon.com/servicecatalog/console?#portfolios) containing a best practice Jupyter notebook product has been configured to give the data science team members the ability to create resources on demand.

---

With the resources created let's move on to Lab 2 where we will, as a project administrator, deploy a secure data science environment for a new project team.
