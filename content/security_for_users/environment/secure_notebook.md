+++
title = "Secure Notebooks"
chapter = false
weight = 10
+++

Amazon SageMaker is designed to empower data scientists and developers, enabling them to build more quickly and remain focused on their machine learning project.  One of the ways that SageMaker does this is by providing hosted Jupyter Notebook servers.  With a single API call users can create a Jupyter Notebook server with the latest patches and kernels available.  No need to install software, patch or maintain systems - it is all done for you.  What the API creates for you is an EC2 instance running Jupyter Notebook server software with Python, R, Docker, and the most popular ML frameworks pre-installed.  But it is not just about convenience and enablement, SageMaker is also focused on hosting these notebooks for you in a secure manner.  

---

## Secure by default

An Amazon SageMaker notebook is an EC2 instance with the open source Jupyter server installed.  The SageMaker service manages the EC2 instance in order to help you maintain a level of security with little or no overhead.  To do this Amazon SageMaker takes responsibility for:

 - Patching and updating the system
 - Encrypting data on the EBS volumes
 - Limiting user access to the system
 - Enabling you to further tailor and harden the system

### Patching and updating

A SageMaker Jupyter notebook server will have 2 EBS volumes attached to it.  The first is the system drive and is ephemeral.  This hosts the operating system, Anaconda, and Jupyter server software.  The second hosts your data and anything you put into `/home/ec2-user/SageMaker`.  Over time your EC2 instance can become out of date, going unpatched.  To patch your Jupyter Notebook to the latest versions simply stop the notebook and start it again.  This will refresh the system drive without any maintenance required on your part.  However please note that if you make any changes to files on the system drive you will need to make those changes again as they will be destroyed in the stopping and starting of the notebook server.

To keep a SageMaker notebook up to date and to save costs it is recommended to stop a Jupyter notebook server when it is not needed and to restart it when you need to use it.

### Encryption at rest

As mentioned the EC2 instance has two EBS volumes mapped to it.  Both of these volumes are encrypted using a SageMaker service-managed KMS key, although you can specify a CMK to be used to encrypt the storage volume.  By doing this you can easily encrypt all of the data stored on the Jupyter Notebook server by default.  

### Limiting user access

The very nature of software development means that users can obtain some OS-level access to the Jupyter notebook server.  By default the Jupyter Web UI will allow you to open a shell terminal.  When a user access the shell, they will be logged into the EC2 instance as `ec2-user`.  If you're familiar with Amazon Linux this is the default username that you use to gain access to an AWS EC2 instance.  This user also typically has permissions to `sudo` to the root user.  This can be limited in the Jupyter Notebook configuration which will remove the user's permission to assume the root user.  They will still have permissions to install things like Python modules into their user environment, but they will not be able to modify the wider system of the notebook server.

### Further tailoring

In addition to the measures described above you also have the ability to specify Lifecycle Configurations.  These are shell scripts which execute on system bootstrap, before the notebook is made available to users.  These configuration scripts can execute on system creation, system startup, or both.  Using these scripts you can ensure that monitoring agents are installed or other types of hardening are performed to ensure that the system is in a specific state before allowing users to access the system. Here we will use the lifecycle scripts to download some open source libraries from the pip mirror we created, create an sagemaker_environment.py file to keep track of variables such as the network configuration, KMS keys that can be imported directly, without giving the datascientist access to them.

## Managed and Governed

Amazon SageMaker provides managed EC2-based services such as Jupyter notebooks, training jobs, and hosted ML models.  SageMaker runs the infrastructure for these components using EC2 resources dedicated to yourself.  These EC2 resources can be mapped to your VPC environment allowing you to apply your network level controls, such as security groups, to the notebooks, training jobs, and hosted ML models.

![SageMaker Notebook Network Diagram](/images/notebook_network.png)

Amazon SageMaker does this by creating an ENI in your specified VPC and attaching it to the EC2 infrastructure in the service account.  Using this pattern the service gives you control over the network-level access of the services you run on Amazon SageMaker.  

For Jupyter Notebooks this will mean that all Internet connectivity, Amazon S3 connectivity, or access to other systems in your VPC is governed by you and by your network configuration. 

For training jobs and hosted models these are again governed by you.  When retrieving training data from Amazon S3 the SageMaker EC2 instances will communicate with S3 through your VPC without traversing the public internet.  Equally when the SageMaker EC2 instances retrieve your trained model for hosting, they will communicate with S3 through your VPC, without traversing the public internet.  You maintain governance and control of the network communication of your SageMaker resources.

## Access Control

Access to a SageMaker Jupyter notebook instance is goverend by AWS IAM.  In order to open a Jupyter Notebook instance users will need access to the `CreatePresignedNotebookInstanceUrl` API call.  This API call creates a presigned URL which can be followed to obtain Web UI access to the Jupyter notebook server.  To secure this interface you use IAM policy statements to wrap conditions around calling the API, for example who can invoke the API and from what IP address.  

```json
{
   "Id":"notebook-example-1",
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"Enable Notebook Access",
         "Effect":"Allow",
         "Action":[
            "sagemaker:CreatePresignedNotebookInstanceUrl",
            "sagemaker:DescribeNotebookInstance"
         ],
         "Resource":"*",
         "Condition":{
            "ForAnyValue:StringEquals":{
               "aws:sourceVpce":[
                  "vpce-111bbccc",
                  "vpce-111bbddd"
               ]
            }
         }
      }
   ]
}
```

The IAM policy above states that someone can only communicate with a Notebook if they do so from within a VPC and through specific VPC endpoints.  Using mechanisms like the above you can explicitly control who can interact with a Notebook server. Other example IAM policies can be found here: https://docs.aws.amazon.com/sagemaker/latest/dg/security_iam_id-based-policy-examples.html

As a best practice, we want to limit a single user to a notebook instance. As every notebook instance has an associated IAM role, by using fine grained user level IAM roles, we can limit a single user to a single notebook instance. Similarly, for cost attribution purposes we can use tagging https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html to link usage costs to individual users or to teams. 

## Version Control

Finally, to support collaboration, SageMaker notebooks can be integrated with Git-based repositories.  Git is a distributed version control system which enables project teams to manage source code, notebook files, and other artifacts related to a project.  In the next lab the notebook you create will be configured to use a CodeCommit repository as a way of managing the sample project provided.  In this lab you will push and tag the code to the master branch of the code repository using the CLI or Jupyter UI. 

---

In this lab, as a data scientist, you will use the newly launched notebook to kickoff a new ML project.
