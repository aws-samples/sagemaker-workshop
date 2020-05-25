+++
title = "Deploy the environment"
chapter = false
weight = 1
+++

In the following steps you will use AWS CloudFormation and Amazon Service Catalog to create a self-service mechanism to create secure data science environments.  You will first deploy a CloudFormation template which provisions a shared service environment which hosts a PyPI mirror along with a detective control to enforce Amazon SageMaker resources being attached to a VPC.  The template will also create a product portfolio in Amazon Service Catalog which enables users with appropriate permissions to create a data science environment dedicated to a single project.  Once this environment is created you will move on to the next lab which will use Amazon Service Catalog to provision a SageMaker notebook.

---

1. Deploy a CloudFormation template which creates a product portfolio, a detective control, and a PyPI mirror:

    | Region | Launch Template |
    |:---:|:---|
    | Oregon (us-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Oregon {{% /button %}} |
    | Ohio (us-east-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ohio {{% /button %}} |
    | N. Virginia (us-east-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-us-east-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS N. Virginia {{% /button %}} |
    | Ireland (eu-west-1) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-1/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Ireland {{% /button %}} |
    | London (eu-west-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-eu-west-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS London {{% /button %}} |
    | Sydney (ap-southeast-2) | {{% button href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?stackName=secure-ds-core&templateURL=https://s3.amazonaws.com/sagemaker-workshop-cloudformation-ap-southeast-2/quickstart/ds_administration.yaml" icon="fas fa-play" %}} Deploy to AWS Sydney {{% /button %}} |

1. Visit the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home) and select the `Outputs` tab for the stack you just deployed.
1. Use the provided link to assume the role of **Project Administrator**.
1. Navigate to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/products) and launch a **Data Science Project Environment**.

    {{% expand "Step-by-Step instructions" %}}

1. Click the context menu button in the upper-right of the **Data Science Project Environment** tile and select `Launch product`.
1. Give the provisioned product a name, such as `project-abc-environment` and click `Next`.
1. Use a **ProjectName** such as `project-abc` and click `Next`.
1. Click `Next` without entering any Tag Options.
1. Click `Next` without checking any Notifications.
1. On the Review page click `Launch`.

    {{% /expand %}}

{{% notice info %}}
The product will take approximately 10 minutes to launch.
{{% /notice %}}

---

You have now created the underlying infrastructure for a secure data science environment which enables project teams to self service.  In the next lab you will use this environment to create a Jupyter notebook for yourself.