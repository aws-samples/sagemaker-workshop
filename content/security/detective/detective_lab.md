+++
title = "Lab 4: Detective and Corrective Controls"
chapter = false
weight = 41
+++

In this lab you will test a remediating detective control that was deployed by the cloud platform engineering team in Lab 1.  The control is designed to detect the creation of Amazon SageMaker training jobs outside of the secure data science VPC and terminate them.  To do this, you will go through the Jupyter notebook kernel *stockmarket_predictor_v5* and execute the cells up to and including the creation of a training job.  You'll notice, as the training job is provisioned and begins to execute, that it is terminated by the corrective aspect of the control in the environment.

---

## Start a training job

As the data scientist, read through and execute the cells in the Jupyter Notebook kernel named `stockmarket_predictor_v5.ipynb`.  These various cells will:

 - set variables to reference the *data* and *model* Amazon S3 buckets that the data science administrator created for your team
 - copy training data to your local Jupyter notebook
 - push processed training data to your *data* S3 bucket
 - create a TensorFlow script to train a model
 - configure a training job to be executed by you

{{% notice note %}}
Be sure and place the names of your *data* and *model* buckets into the `TEAM_DATA_BUCKET` and `TEAM_MODEL_BUCKET` variables in the cell titled **Pre-requisites**.  If you don't know the name of your Amazon S3 buckets, created earlier by the Data Science Administrator, they should follow a pattern of `sagemakerworkshop-data-<YOUR-TEAM-NAME>` and `sagemakerworkshop-model-<YOUR-TEAM-NAME>`.  To confirm your bucket names visit the [S3 console](https://console.aws.amazon.com/s3/home) and find your *data* and *model* buckets.
{{% /notice %}}

When you reach the cell titled **Train without a VPC configured**, execute the cell and take note of the output.  After a few minutes you should notice that the training job was terminated.  The output should resemble the below which indicates that the training job did not complete its bootstrap.

{{% expand "Step-by-step instructions" %}}
1. Begin at the JupyterLabs interface of your notebook instance and execute the first 2 cells of the notebook kernel.
1. When you reach the cell titled **Pre-requisites** you will see two variables defined in the python code: `TEAM_DATA_BUCKET` and `TEAM_MODEL_BUCKET`.  These variables refer to the Amazon S3 buckets created earlier when the data scientist administrator provisioned team resources.  You can reference back to the CloudFormation template to find the name of the resource buckets if you don't remember them.  They should follow a format such as `sagemakerworkshop-data-<YOUR-TEAM-NAME>` and `sagemakerworkshop-model-<YOUR-TEAM-NAME>`.
1. After you have set the values for these two variables execute the cells up to and including the cell titled **Train without a VPC configured**.
1. Watch the output of the training as it executes, and notice the job does not complete its bootstrap.
{{% /expand %}}

```Access log
2019-10-11 13:50:37 Starting - Starting the training job...
2019-10-11 13:51:01 Starting - Launching requested ML instances...
2019-10-11 13:51:35 Starting - Preparing the instances for training......
2019-10-11 13:52:29 Downloading - Downloading input data
2019-10-11 13:52:29 Stopping - Stopping the training job
2019-10-11 13:52:29 Stopped - Training job stopped
..Training seconds: 1
Billable seconds: 1
training completed.
```

### Detective control explained

The training job was terminated by an AWS Lambda function that was executed in response to a CloudWatch Event that was triggered when the training job was created.  The Lambda function inspected the training job, saw that it was NOT attached to a VPC and stopped the training job from executing.  

Assume the role of the Data Science Administrator and review the code of the [AWS Lambda function SagemakerTrainingJobVPCEnforcer](https://console.aws.amazon.com/lambda/home?#/functions/SagemakerTrainingJobVPCEnforcer?tab=configuration). Also review the [CloudWatch Event rule SagemakerTrainingJobVPCEnforcementRule](https://console.aws.amazon.com/cloudwatch/home?#rules:name=SagemakerTrainingJobVPCEnforcementRule) and take note of the event which triggers execution of the Lambda function.

---

## Start a compliant training job

To succesfully run your training job you will need to configure the training job to run within your VPC.  To do this you will pass a collection of subnet IDs and security groups to the training job using the SageMaker SDK from your notebook. 

Visit the [Parameter Store](https://console.aws.amazon.com/systems-manager/parameters) and copy the values stored for `PrivateSubnetAId`, `PrivateSubnetBId`, and `SageMakerSecurityGroupId`.  Then, using the [SageMaker SDK documentation](https://sagemaker.readthedocs.io/en/stable/estimators.html), modify the Python code in the cell titled **Training with VPC attachment** to configure the estimator with the subnet and security group values captured from the Parameter Store.  

```python
TensorFlow(entry_point='predictor.py',
            ...,
            train_instance_count=1,
            train_instance_type=instance_type,
            subnets = ['subnet-0fc1ed6b334bd4cfd','subnet-0f398485e991f8333'],
            security_group_ids = ['sg-0da87d40633b8f922'],
            ...
            )
```
When the code has been modified execute the cell, the training job should complete successfully, producing output similar to the following:

```logs
2019-10-16 19:57:54 Starting - Starting the training job...
2019-10-16 19:57:56 Starting - Launching requested ML instances......
2019-10-16 19:58:59 Starting - Preparing the instances for training...
2019-10-16 19:59:46 Downloading - Downloading input data...
2019-10-16 20:00:25 Training - Training image download completed. Training in progress..
2019-10-16 20:00:25,711 INFO - root - running container entrypoint
2019-10-16 20:00:25,711 INFO - root - starting train task
2019-10-16 20:00:25,727 INFO - container_support.training - Training starting
```

{{% expand "Step-by-step instructions" %}}
1. Copy the values for approved VPC subnets stored into Parameter Store as [PrivateSubnetAId](https://console.aws.amazon.com/systems-manager/parameters/PrivateSubnetAId/description?) and [PrivateSubnetBId](https://console.aws.amazon.com/systems-manager/parameters/PrivateSubnetBId/description).  
1. Copy the approved security group ID stored as [SageMakerSecurityGroupId](https://console.aws.amazon.com/systems-manager/parameters/SageMakerSecurityGroupId/description?).
1. Once you have the values you need for security groups and subnets return to the Jupyter notebook kernel.
1. Modify the cell titled **Training with VPC Attachment** which re-defines the TensorFlow estimator.  
1. Add parameters to the estimator definition similar to the above, passing in the `subnets` and `security_group_ids` values to configure the estimator for [VPC attachment](https://sagemaker.readthedocs.io/en/stable/estimators.html).
1. Execute the cell to create a new training job. 
{{% /expand %}}

---

In this lab, you experienced a remediating detective control deployed by the cloud platform engineering team and reconfigured the SageMaker training job to run connected to your VPC.  But waiting minutes to find out that your training job is going to error out is a slow and painful way to iterate during development.  

In the next lab we will implement a preventive control to make such an error immediately evident.