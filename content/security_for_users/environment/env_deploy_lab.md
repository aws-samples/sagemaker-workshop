+++
title = "Lab 01: Deploy the environment"
chapter = false
weight = 20
+++

In the following steps you will use AWS CloudFormation and AWS Service Catalog to create a self-service mechanism to create secure data science environments.  You will first deploy a CloudFormation template which provisions a shared service environment which hosts a PyPI mirror along with a detective control to enforce Amazon SageMaker resources being attached to a VPC.  The template will also create a product portfolio in AWS Service Catalog which enables users with appropriate permissions to create a data science environment dedicated to a single project.  Once this environment is created you will move on to the next lab which will use AWS Service Catalog to provision a SageMaker notebook.

---

Using one of the buttons below deploy the Secure Data Science quickstart into your AWS account.  This will provision a shared services VPC which hosts a PyPI mirror.  The CloudFormation stack will also create an AWS Service Catalog to allow project administrators to create data science environments using the self-service mechanisms of the AWS Service Catalog.

After the CloudFormation stack has been deployed visit the `Outputs` tab of the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home).  You will notice a hyperlink which will allow you to the assume the role of a **Project Administrator**.  Click this link and then navigate to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/products) and launch a **Data Science Project Environment**.

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |


{{% expand "Step-by-Step instructions" %}}

1. Visit the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home) and select the `Outputs` tab for the stack you just deployed.
1. Use the provided link to assume the role of **Project Administrator**.
1. Navigate to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/products) and launch a **Data Science Project Environment**.
1. Click the context menu button in the upper-right of the **Data Science Project Environment** tile and select `Launch product`.
1. Give the provisioned product a name, such as `project-abc-environment` and click `Next`.
1. Use a **ProjectName** such as `project-abc` and click `Next`. 
**Note** S3 bucket names need to be globally unique, so don't use project-abc verbatim but replace "abc" with something unique such as "yourname-12345" etc. 
1. Click `Next` without entering any Tag Options.
1. Click `Next` without checking any Notifications.
1. On the Review page click `Launch`.

{{% /expand %}}

{{% notice info %}}
The product will take approximately 10 minutes to launch.
{{% /notice %}}

---

At this point, the cloud platform engineering team has created the underlying infrastructure to host secure data science environments.  The project administrators have provisioned a data science environment for your project team and they have provided you with an AWS Service Catalog to enable you to provision SageMaker notebooks.  In the next lab you will use these resources to provision a Jupyter notebook and start developing your ML solution.
