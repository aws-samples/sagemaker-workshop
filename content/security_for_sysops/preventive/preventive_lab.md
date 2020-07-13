+++
title = "Lab 5: Preventive Controls"
chapter = false
weight = 61
+++

In this lab, you will implement a preventive control that will stop a training job from starting if it's not launched within a VPC.  In the interest of defence in depth you will implement the preventive control to complement the detective control exercised in the previous lab.

---

The preventive control you will deploy here is a modification to the IAM policy which grants a user's notebook the permission to launch a training job.  Every user's notebook has an IAM role created for it and an IAM policy attached to this role.  Updating these policies individually is not feasible at scale so use the ability of AWS Service Catalog to do the heavy lifting for you.

Create an updated version of the Jupyter notebook product to deliver the updated IAM policy to user's notebooks.  Resume the role of the Project Administration team and visit the AWS Service Catalog console.  Drill into the [SageMaker notebook product](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products) and provide an updated teamplate for this product using the appropriate URL from the list below.  When you have a new version of the SageMaker notebook product created return to the role of the project team member and update your notebook to the latest revision.

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

After the product has been successfully updated revist the Jupyter notebook kernel and execute the cell titled **Train Without VPC Configured**.  You should now quickly receive an Access Denied exception similar to the below:

```bash
ClientError: An error occurred (AccessDeniedException) when calling the CreateTrainingJob operation: User: arn:aws:sts::012348485732:assumed-role/SageMakerExecRole-ml-product-team/SageMaker is not authorized to perform: sagemaker:CreateTrainingJob on resource: arn:aws:sagemaker:eu-west-1:012348485732:training-job/sagemaker-tensorflow-2019-10-16-22-14-30-880 with an explicit deny
```

---

In this lab you modified the permissions granted to the instances of the data science team's Jupyter notebooks.  This altered their permissions so that they could only perform actions like creating a training job if that action met the security requirement of specifying a VPC configuration.  Visit the defined products using the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products) to review the CloudFormation template and the changes it made to the permissions.  Or use the [IAM console](https://console.aws.amazon.com/iam/home?#/roles) to review the role you created and the policy attached to it.  What conditions are on the IAM policy controlling access to the SageMaker API?
