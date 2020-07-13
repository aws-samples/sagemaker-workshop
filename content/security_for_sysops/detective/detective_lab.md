+++
title = "Lab 4: Detective and Corrective Controls"
chapter = false
weight = 41
+++

In this lab you will test a remediating detective control that was deployed by the cloud platform engineering team in Lab 1.  The control is designed to detect the creation of Amazon SageMaker training jobs outside of the secure data science VPC and terminate them.  To do this, you will go through the Jupyter notebook kernel *00_SageMaker-SysOps-Workflow* and execute the cells up to and including the creation of a training job.  You'll notice, as the training job is provisioned and begins to execute, that it is terminated by the corrective aspect of the control in the environment.

---

## Start a training job

As the data scientist, read through and execute the cells in the Jupyter Notebook kernel named `00_SageMaker-SysOps-Workflow.ipynb`.  These various cells will:

 - set variables to reference the *data* and *model* Amazon S3 buckets that the administrators created for your team
 - copy training data to your local Jupyter notebook
 - preprocess the data using SageMaker Processing 
 - push processed training data to your *data* S3 bucket
 - configure a training job to be executed by you
 - Use SageMaker Experiments to track metadata of your training jobs

When you reach the cell titled **Train without a VPC configured**, execute the cell and take note of the output.  After a few minutes you should notice that the training job was terminated.  The output should resemble the below which indicates that the training job did not complete its bootstrap.

{{% expand "Step-by-step instructions" %}}
1. Begin at the JupyterLabs interface of your notebook instance and execute the first cells of the notebook kernel.
1. Continue executing the cells through Sections A and B, until you reach Section C, Part 5.
1. When you reach the cells titled **Train without a VPC configured**, pause - the following cells should fail after a period when you execute them.
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

To succesfully run your training job you will need to configure the training job to run within your VPC.  To do this you will pass a collection of subnet IDs and security groups that we imported earlier. 

The following sample code shows how these can be specified:

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
Execute the cell below the failed training job deployment titled **Traing with a VPC**, the training job should complete successfully, producing output similar to the following:

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

---

In this lab, you experienced a remediating detective control deployed by the cloud platform engineering team and reconfigured the SageMaker training job to run connected to your VPC.  But waiting minutes to find out that your training job is going to error out is a slow and painful way to iterate during development.  

In the next lab you will look into what preventive controls can be put in place to enhance your defense in depth and provide a better developer experience for the project team members.
