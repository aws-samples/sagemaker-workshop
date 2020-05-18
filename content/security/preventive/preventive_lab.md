+++
title = "Lab 5: Preventive Controls"
chapter = false
weight = 61
+++

In Lab 4, you tested out a remediating detective control that is triggered when a SageMaker training job is launched outside of the VPC. But waiting minutes to find out that your training job is going to error out is not a great experience for the data scientists.  In this lab, you will implement a preventive control that will prevent a training job from starting if it's not launched within a VPC.  In the interest of defence in depth we will now implement the preventive control to complement the detective control exercised in the previous lab.

To deploy a preventive control, assume the role of the [Data Science Administrator](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientistAdmin&displayName=DataScienceAdmin) and create a new version of the [Data Science Project Environment](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products), updating it with one of the CloudFormation templates referenced below in Amazon S3.

{{% expand "Step-by-step instructions" %}}
1. The preventive control will be a modified IAM policy associated with the exeuction role of the SageMaker notebook instance.  To modify the role we will first need to assume the role of the Data Science Administrator. 
1. Next visit the Products tab on the [Service Catalog Product Administration console](https://console.aws.amazon.com/servicecatalog).
1. Open the `Data Science Project Environment` and click `Create new version`.

    ![Product Version](/images/product_version.png)

1. Specify the S3 location of the updated CloudFormation template.  Based on the region you've been building in use an S3 URL from below.
1. Specify a version title of *Version 2*, provide a description, and click `Save`.
{{% /expand %}}

 - Ireland (eu-west-1)

    ```
    https://sagemaker-workshop-cloudformation-eu-west-1.s3.amazonaws.com/ds_notebook.yaml
    ```

 - London (eu-west-2)

    ```
    https://sagemaker-workshop-cloudformation-eu-west-2.s3.amazonaws.com/ds_notebook.yaml
    ```

 - Sydney (ap-southeast-2)

    ```
    https://sagemaker-workshop-cloudformation-ap-southeast-2.s3.amazonaws.com/ds_notebook.yaml
    ```

 - Oregon (us-west-2)

    ```
    https://sagemaker-workshop-cloudformation-us-west-2.s3.amazonaws.com/ds_notebook.yaml
    ```

 - N. Virginia (us-east-1)

    ```
    https://sagemaker-workshop-cloudformation-us-east-1.s3.amazonaws.com/ds_notebook.yaml
    ```

 - Ohio (us-east-2)

    ```
    https://sagemaker-workshop-cloudformation-us-east-2.s3.amazonaws.com/ds_notebook.yaml
    ```

Now with a new version of the product defined turn to the [Provisioned Products console](https://console.aws.amazon.com/servicecatalog/home?#/stacks) and update the execution role created earlier to the latest version you just created.

{{% expand "Step-by-step instructions" %}}
1. Next switch to the [Provisioned Products console](https://console.aws.amazon.com/servicecatalog/home?#/stacks).
1. Click the *3-dot* menu icon next to the execution role provisioned earlier and click `Update provisioned product`.

    ![Update Product Version](/images/update_product_version.png)

1. Select the radio button for the latest revision of the product and click `Next`, `Next`, `Update`.  Wait until the product has updated itself and its Status is set to Succeeded.

    ![Wait for Update to Complete](/images/update_provisioned_product.png)
{{% /expand %}}

After the product has been successfully updated resume the role of [Data Scientist](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientist&displayName=DataScientist) and revist the Jupyter notebook kernel and execute the cell titled **Train Without VPC Configured**.  You should now quickly receive an Access Denied exception similar to the below:

```bash
ClientError: An error occurred (AccessDeniedException) when calling the CreateTrainingJob operation: User: arn:aws:sts::012348485732:assumed-role/SageMakerExecRole-ml-product-team/SageMaker is not authorized to perform: sagemaker:CreateTrainingJob on resource: arn:aws:sagemaker:eu-west-1:012348485732:training-job/sagemaker-tensorflow-2019-10-16-22-14-30-880 with an explicit deny
```
---

In this lab you modified the permissions granted to the instances of the data science team's Jupyter notebooks.  This altered their permissions so that they could only perform actions like creating a training job if that action met the security requirement of specifying a VPC configuration.  Visit the defined products using the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=false&#admin-products) to review the CloudFormation template and the changes it made to the permissions.  Or use the [IAM console](https://console.aws.amazon.com/iam/home?#/roles) to review the role you created and the policy attached to it.  What conditions are on the IAM policy controlling access to the SageMaker API?
