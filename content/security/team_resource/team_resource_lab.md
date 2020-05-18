+++
title = "Lab 2: Team Resources"
chapter = false
weight = 25
+++

The cloud platform enineering team have now minted a new secure environment for us to begin supporting data science teams.  To get started, as the Data Science Administrator, use the AWS Service Catalog to create an IAM role for a new data science team.  This IAM role will be used as the execution role for all SageMaker Notebooks created by the data scientists on the team.  The execution role controls what AWS resources, such as S3 buckets, the SageMaker notebook will be able to access.  We will also use AWS CloudFormation to create the rest of the team resources including an AWS Service Catalog Portfolio.  The portfolio will contain a SageMaker Notebook product that will allow data scientists to self-service and deploy their own secure SageMaker notebooks.  

---

## Enable the data science team

Assume the role of the [Data Science Administrator](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientistAdmin&displayName=DataScienceAdmin) and using the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) launch the **Data Science Project Environment** product.  After the product has launched create a Service Catalog portfolio specifically for the data science team.

{{% expand "Step-by-step instructions" %}}
1. Assume the role of the [Data Science Administrator](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientistAdmin&displayName=DataScienceAdmin) 
For the `Account` field enter your 12-digit AWS account ID.  You can find it on the [My Account](https://console.aws.amazon.com/billing/home?#/account) page.
1. Open the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.
1. Open the menu for the Data Science Project Environment product and click `Launch Product`

    ![Provision Notebook Execution Role](/images/launch_sagemaker_role.png)

1. Give the product a name such as `<your name>-sagemaker-demo` (LOWER CASE ONLY), click `Next` and then enter a unique `TeamName` such as `team-<PRODUCT NAME>` or a similar value of your choosing.
1. Click `Next` on the next 3 screens and then click `Launch`.  
1. You will land on a Provisioned Product page and can periodically click the Refresh button to see the status of the product deployment.

    ![Provision Product Status](/images/launch_product_status.png)

1. Once the Status for the product shows `Succeeded` you can move on to the next step.

    ![Provision Product Success](/images/provisioned_product.png)

{{% /expand %}}

---

With the role created it's now time to create resources for a data science product team.  The team will need Amazon S3 buckets, KMS encryption keys, and a Service Catalog Portfolio to self-service and create Jupyter notebooks.  To create these resources use the **Deploy to AWS** button for your region below to launch a CloudFormation template.  Please ensure that the same Team Name that was specified in Service Catalog above is used now with CloudFormation.

As in the previous step all of the parameters should have reasonable defaults but you can change them to your preference provided those changes are in line with the stack deployment in Lab 1.

{{% expand "Step-by-step instructions" %}}
1. Click **Deploy to AWS** for one of the AWS regions below, ensure it is the same region that you used in Lab 1.
1. Enter the **Team Name** you chose in the steps above under **Team Configuration**
1. Click **Create stack** 

You will be redirected to the CloudFormation console where you can see it provisioning resources on your behalf.  When it shows **CREATE_COMPLETE** for the status you can proceed to the next step.
{{% /expand %}}

{{% notice info %}}
Be sure and launch into the same region you used during the previous step and use the same team name as defined above.
{{% /notice %}}

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-west-2&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-east-2&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-us-east-1&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-eu-west-1&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-eu-west-2&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}}
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=sagemaker-security-team-resources&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/team_resources.json&param_ProductS3Bucket=sagemaker-workshop-cloudformation-ap-southeast-2&param_TeamName=team-<PRODUCT NAME>" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

---
## Review team resources

You have now created multiple AWS resources to support the data science team.  Please take a moment and review these resources and their configuration.

- **Amazon S3 buckets for training data and trained models**

    [Visit the S3 console](https://console.aws.amazon.com/s3/home) and see the Amazon S3 buckets that have been created for the team.  Take note of the bucket policy that has been applied to the data bucket.

- **AWS KMS key for encrypting data at rest**

    A KMS key to encrypt data at rest in the data science environment. [Visit the console](https://console.aws.amazon.com/kms/home?#/kms/home), who is allowed to take what actions on the keys created?

- **Parameters added to Parameter Store**

    A [parameter](https://console.aws.amazon.com/systems-manager/parameters) has been added to the collection in Parameter Store.  Can you see what parameter has been added?  How would you use this value?

- **Service Catalog Jupyter Notebook product**

    A [Service Catalog Portfolio](https://console.aws.amazon.com/servicecatalog/console?#portfolios) containing a best practice Jupyter notebook product has been configured to give the data science team members the ability to create resources on demand.

---

With the teams resources created let's move on to Lab 3 where we will, as a data scientist, self-service and create a Jupyter notebook server.
