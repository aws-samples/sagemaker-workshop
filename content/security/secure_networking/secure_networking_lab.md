+++
title = "Lab 1: Secure Networking"
chapter = false
weight = 11
+++

The data science team have requested a cloud environment to complete their project.  The data science administrator has in turn asked the Cloud Platform Engineering team to provision a secure VPC for the data science team.  In this lab, you will use [AWS CloudFormation](https://aws.amazon.com/cloudformation/) to provision this data science environment.  Following the steps below create an environment which contains:

 - AWS VPC with no IGW
 - VPC endpoints to Amazon S3, Amazon SageMaker, CloudWatch, STS
 - IAM roles for the data science administrator and the data scientist
 - Store AWS resource names/identifiers in AWS SSM Parameter Store for future reference
 - Service Catalog Portfolio of products for the data science administrator
 - AWS CloudTrail configured to log AWS API calls
 - AWS KMS key for encrypting CloudTrail logs
 - Remediating detective control to ensure SageMaker resources are only deployed into a VPC (CloudWatch Event Rule + Lambda Function)

{{% notice info %}}

As you work through these labs make a note of your **AWS account ID** and the **team name** you choose in Lab 2.  You will need your 12 digit AWS account number to quickly assume the 3 roles you will create.  You will also, in Lab 2, create a **team name** that you will then need again later in other labs.

{{% /notice %}}

## Create a Cloud Platform Engineering IAM role 

{{% notice warning %}}
Skip ahead to the next step (**Deploy base environment**) if you are at an AWS-hosted event using an Event Engine account.  
ONLY perform this step (**Create a Cloud Platform Engineering IAM role**) if using your own AWS account.
{{% /notice %}}

The first step to complete these labs is to create a Cloud Platform Engineering IAM role to be used by the cloud platform engineering team to deploy the base infrastructure.  To do this use one of the links below based on the region you want to work in:

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=sagemaker-security-ccoe-role&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/event_engine_initial.json" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

## Deploy base environment

Your first task as the cloud platform engineer is to create a base environment for the data science teams.  Start by [assuming the role of the Cloud Platform Engineering team](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=CloudCOE&displayName=CloudCOE) via the AWS console.  Then, as the cloud platform engineer, use CloudFormation to provision a secure VPC environment to support the data science team.  Choose a region below and click **Deploy to AWS** to get started.

All of the parameters in this CloudFormation template should have reasonable defaults but you can change them to your preference if you wish.  One parameter that DOES NOT have a default and that you MUST define is the **suffix for the CloudTrail logging bucket**.  Please set this to a value such as `<YOUR-NAME>-smlab`.

{{% expand "Step-by-step instructions" %}}
 1. Click **Deploy to AWS** for one of the AWS regions below.
 1. Enter a value such as `<YOUR-NAME>-smlab` for the **suffix for the CloudTrail logging bucket**.
 1. Click the box labeled **I acknowledge that AWS CloudFormation might create IAM resources with custom names** to confirm that CloudFormation can create IAM resources on your behalf.
 1. Finally click **Create Stack**.

You will be taken to a summary screen that shows the resources being provisioned for you.  When the stack on the left shows **CREATE_COMPLETE** in green then you are finsihed with this step and can proceed.
{{% /expand %}}

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-west-2&" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-east-2&" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-east-1&" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-eu-west-1&" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-eu-west-2&" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=sagemaker-security-base&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/event_engine_base.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-ap-southeast-2&" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

{{% notice info %}}
If you wish to see the contents of these CloudFormation templates you can view them on the CloudFormation console or copy them locally for review using a command such as the below.
{{% /notice %}}

```bash
aws s3 sync s3://sagemaker-workshop-cloudformation-us-east-1 ./sagemaker-workshop-cloudformation
```

---
## Review base environment

After the CloudFormation stack has been successfully created review the **Resources** tab of the CloudFormation stack and the resources that were created.  You'll notice that it has provisioned:

 - **CloudTrail**

     CloudTrail has been configured to log all activity in the environment and to serve as an event source for a CloudWatch Event Rule as part of a detective control you will test later.  [Visit the console](https://console.aws.amazon.com/cloudtrail/home), what records can you see already present in the logs?

 - **Service Catalog Portfolio**

     A [service catalog portfolio](https://console.aws.amazon.com/servicecatalog/home) and products have been configured to give the data science teams tailored products they can deploy easily.

 - **IAM roles**

     [Roles and permissions](https://console.aws.amazon.com/iam/home) have been created so that the data science teams can manage themselves and create the resources they need.

 - **SSM parameters**

     A collection of parameters have been stored so they can be referenced by the data science teams.  [Visit the console](https://console.aws.amazon.com/systems-manager/parameters), what parameters have been created?

 - **KMS key**

     A KMS key to encrypt data at rest in the data science environment.  [Visit the console](https://console.aws.amazon.com/kms/home), what is the KMS key being used to encrypt?

 - **Lambda function**

     The combination of the [CloudWatch event rule](https://console.aws.amazon.com/cloudwatch/home?#rules:) and the [Lambda function](https://console.aws.amazon.com/lambda/home?#/functions/SagemakerTrainingJobVPCEnforcer?tab=configuration) acts as a detective and corrective control inspecting training jobs that are launched to ensure they remain compliant with your defined security requirements.  [Visit the console](https://console.aws.amazon.com/cloudwatch/home?#rules:), what event will cause the Lambda function to execute?

 - **Virtual Private Cloud (VPC)**

     The template has created a VPC with no Internet connectivity but with VPC endpoints for accessing AWS services like Amazon S3 and Amazon SageMaker.  [Visit the console](https://console.aws.amazon.com/vpc/home), what services are accessible from within the VPC?  Do any endpoints have Endpoint Policies governing them?

---

You have now created a secure environment for the data science teams. Lets now hand things back to the Data Science Administrator and let them support the data science team.