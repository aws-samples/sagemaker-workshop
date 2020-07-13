+++
title = "Lab 1: Best Practice as Code"
chapter = false
weight = 25
+++

Before you can begin creating templates for deployment by the Project Administration team you will need a shared services VPC to host a Python package mirror (PyPI) for use by data science teams.  The mirror will host a collection of approved Python packages.  The concept of a shared services VPC or PyPI mirror is not something that is detailed in this workshop, and is partially assumed as common practice among many AWS customers.  After you have created a shared services VPC and PyPI mirror you will then, as the Cloud Platform Engineering Team, create a Service Catalog Portfolio which the project administrators can use to easily deploy data science environments in support of new projects.

{{% notice info %}}
This lab assumes other recommended security practices such as enabling AWS CloudTrail and capturing VPC Flow Logs.  The contents of this lab focus soley on controls and guard rails directly related to data science resources.
{{% /notice %}}

---

## Shared Services architecture

In this section you will quickly get started by deploying a shared PyPI mirror for use by data science project teams.  In addition to deploying a shared service this template will also create an IAM role for use by the AWS Service Catalog and for use by project administrators who are responsible for creating data science project environments.

The shared PyPI mirror will be hosted in a shared services VPC and exposed to project environments using a PrivateLink-powered endpoint.  The mirror will host approved Python packages and can be used by all internal Python applications, such as machine learning code running on SageMaker.

The resulting architecture will look like this:

![Shared Services Architecture](/images/sec-ds-architecture-simplified-v2.png)

### Deploy your shared service

As a cloud platform engineering team member, deploy the CloudFormation template linked below to provision a shared service VPC and IAM roles.

| Region | Launch Template |
|:---:|:---|
| Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
| Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
| N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
| Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
| London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
| Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_administration.yaml&param_QuickstartMode=false" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

Deployment should take around 5 minutes.

{{% expand "Step-by-Step instructions" %}}
1. Click the button above which is appropriate for the AWS region in which you want to build.  This will take you to the CloudFormation console to create a CloudFormation stack using the reference CloudFormation template.  
1. Check the Stack name and the name you want to use for your shared service resources.  You can accept the default values if you wish.
1. Under `Capabilities` click the 2 check boxes indicating you understand that the CloudFormation template will create IAM resources.
1. Click `Create stack`
{{% /expand %}}

### Create Project Portfolio

With the shared services VPC online and available you now need to provide the project administration team with a configured Service Catalog to provision data science project environments.  To start, visit the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog/home) and create a Portfolio.  Grant the DataScienceAdmin role permissions to access the portfolio adn then use the appropriate CloudFormation template linked below to create a Data Science Environment product.  Ensure that the product has a constraint applied to it that uses the IAM role ServiceCatalogLaunchRole to launch the product upon request.  This will give the Service Catalog service the permissions needed to create a Data Science Environment.

**Service Catalog Product Templates by Region**:

  - **Region ap-southeast-2**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_environment.yaml` 
  - **Region eu-west-1**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_environment.yaml` 
  - **Region eu-west-2**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_environment.yaml` 
  - **Region us-east-1**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_environment.yaml` 
  - **Region us-east-2**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_environment.yaml` 
  - **Region us-west-2**, `https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_environment.yaml` 

{{% expand "Step-by-Step instructions" %}}
1. Access the [AWS Service Catalog console](https://console.aws.amazon.com/servicecatalog/home)
1. Click Portfolios on the left
1. Click `Create portfolio`
1. Enter a `Portfolio name` of `Data Science Project Portfolio`
1. Enter a `Owner` of `Cloud Operations Team`
1. Click the link for your new portfolio to view the portfolio's details
1. Click the `Groups, roles, and users` tab
1. Click `Add groups, roles, users`
1. Click `Roles` and type `DataScienceAdmin` into the search field
1. Tick the box next to your DataScienceAdministrator role and click `Add access`
1. Click `Products`
1. Click `Upload new product`
1. Enter a `Product name` of `Data Science Environment`
1. For `Owner` enter `Cloud Operations Team`
1. Click `Use a CloudFormation template`
1. For the `CloudFormation template URL` enter the appropriate URL from the list below:
1. For `Version title` enter `v1`
1. Click `Review` and `Create product`
1. Click the radio button next to the new product and from the `Actions` drop down select `Add product to portfolio`
1. Click the radio button for your product portfolio and click `Add Product to Portfolio`
1. Return to the list of Portfolios by clicking `Portfolios` on the left
1. Click the link for the data science portfolio
1. Click the `Constraints` tab in the portfolio detail page
1. Click `Create constraint`
1. From the `Product` drop down select your product
1. Select `Launch` for the `Constraint type`
1. Under `Launch Constraint` click `Select IAM role`
1. From the `IAM role` drop down select `ServiceCatalogLaunchRole`
1. Click `Create`
{{% /expand %}}

---

## Review team resources

In addition to the Service Catalog Portfolio and product you have also created the following AWS resources to support the project administration team.  Please take a moment and review these resources and their configuration.

IAM roles

- **AWS IAM roles**

    The IAM roles for the project administration team and the Service Catalog have been created.  Visit the [AWS IAM console](https://console.aws.amazon.com/iam/home?#/roles) and review the permissions granted to these two roles.  

- **AWS Lambda Detective Control**

    An AWS Lambda has been deployed and configured to execute whenever an Amazon SageMaker resource is deployed.  The Lambda function will act as a detective control, inspecting launched resources to ensure that the resource is configured correctly.  To inspect the Lambda function and its triggers visit the [AWS Lambda console](https://console.aws.amazon.com/lambda/home?#/functions).  Can you determine exactly what types of events will cause the Lambda function to execute?

- **Parameters added to Parameter Store**

    A collection of [parameters](https://console.aws.amazon.com/systems-manager/parameters) have been added to Parameter Store.  Can you see what parameters have been added?  How would you use these values?

- **Shared Services VPC**

    The template has created a VPC that will house our shared applications. [Visit the console](https://console.aws.amazon.com/vpc/home) and see what services are accessible from within the VPC?

- **PyPI Mirror Service**

    A Python package mirror has been deployed as a containerised service in the Shared Services VPC. This service is running on a cluster managed by Amazon Elastic Container Service (ECS) Fargate which means there are no Amazon EC2 servers for you to manage.  [Visit the ECS console](https://console.aws.amazon.com/ecs/home) to check whether the service is up and running. You can also see the task logs from the container through the ECS console to check its status.

---

With these resources created you can now move on to Lab 2 where you will, as a project administrator, deploy a secure data science environment for a new project team.
