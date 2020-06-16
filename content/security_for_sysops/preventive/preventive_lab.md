+++
title = "Lab 5: Preventive Controls"
chapter = false
weight = 61
+++

In Lab 4, you tested out a remediating detective control that is triggered when a SageMaker training job is launched outside of the VPC. But waiting minutes to find out that your training job is going to error out is not a great experience for the data scientists.  In this lab, you will implement a preventive control that will prevent a training job from starting if it's not launched within a VPC.  In the interest of defence in depth we will now implement the preventive control to complement the detective control exercised in the previous lab.

To deploy a preventive control, assume the role of the [Data Science Administrator](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientistAdmin&displayName=DataScienceAdmin) and create a new version of the [Data Science Project SageMaker Notebook](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products), updating it with one of the CloudFormation templates referenced below in Amazon S3.

{{% expand "Step-by-step instructions" %}}
1. The preventive control will be a modified IAM policy associated with the exeuction role of the SageMaker notebook instance.  To modify the role you first need to assume the role of the Data Science Administrator. 
1. Next click [`Products`](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products) under `Administration` on the left-hand navigation bar.
1. Click the link for the SageMaker notebook product you want to update.
1. On the product detail page click `Create new version`.
1. Click `Use a CloudFormation Template`
1. Paste the appropriate URL for your region below into the `CloudFormation template` field.
1. Type `Version 2` for the `Version title`.
1. Click `Create product version`.
1. Navigate to [`Provisioned products`](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true&#/stacks)
1. Resume the role of a data science project team member and navigate to Service Catalog's [Provisioned Products console](https://console.aws.amazon.com/servicecatalog/home?&isSceuc=true#/stacks)
1. Click the context menu next to your notebook and select `Update provisioned product`
1. Click the radio button next to `Version 2` and click `Next`.
1. Click `Next` and then `Update` and wait for the notebook's permissions to be updated.
{{% /expand %}}

 - Ireland (eu-west-1)

    `
    https://s3.eu-west-1.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_notebook_v2.yaml
    `

 - London (eu-west-2)

    `
    https://s3.eu-west-2.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_notebook_v2.yaml
    `

 - Sydney (ap-southeast-2)

    `
    https://s3.ap-southeast-2.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_notebook_v2.yaml
    `

 - Oregon (us-west-2)

    `
    https://s3.us-west-2.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_notebook_v2.yaml
    `

 - N. Virginia (us-east-1)

    `
    https://s3.us-east-1.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_notebook_v2.yaml
    `

 - Ohio (us-east-2)

    `
    https://s3.us-east-2.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_notebook_v2.yaml
    `

After the product has been successfully updated resume the role of [Data Scientist](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientist&displayName=DataScientist) and revist the Jupyter notebook kernel and execute the cell titled **Train Without VPC Configured**.  You should now quickly receive an Access Denied exception similar to the below:

```bash
ClientError: An error occurred (AccessDeniedException) when calling the CreateTrainingJob operation: User: arn:aws:sts::012348485732:assumed-role/SageMakerExecRole-ml-product-team/SageMaker is not authorized to perform: sagemaker:CreateTrainingJob on resource: arn:aws:sagemaker:eu-west-1:012348485732:training-job/sagemaker-tensorflow-2019-10-16-22-14-30-880 with an explicit deny
```
---

In this lab you modified the permissions granted to the instances of the data science team's Jupyter notebooks.  This altered their permissions so that they could only perform actions like creating a training job if that action met the security requirement of specifying a VPC configuration.  Visit the defined products using the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products) to review the CloudFormation template and the changes it made to the permissions.  Or use the [IAM console](https://console.aws.amazon.com/iam/home?#/roles) to review the role you created and the policy attached to it.  What conditions are on the IAM policy controlling access to the SageMaker API?
