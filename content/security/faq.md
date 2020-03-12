+++
title = "Frequently Asked Questions"
chapter = false
weight = 200
+++

The following is a compiled list of questions or issues you may face while going through these labs.  If you encounter any issues, hopefully you will find helpful guidance below.

---

 Q: **I am trying to execute a cell in Jupyter Notebook but nothing happens.**

 A: Check whether the kernel has finished loading.  You can see kernel status in the upper right corner of the Notebook.  For the purpose of this workshop, the kernel should be conda_tensorflow_p27.  If the kernel has not loaded, give it a couple of minutes to load.  If it still says No Kernel, then navigate to the Jupyter menu bar and select Kernel → Restart Kernel. Select conda_tensorflow_p27 if prompted for the kernel type.

 Q: **Training job completes successfully, but I see all sorts of errors and warnings in the logs such as “No response body. Response code: 404” and “If the signature check failed. This could be because of a time skew. Attempting to adjust the signer.”**
 
 A: You may ignore these informational log messages for the purpose of this workshop.

 Q: **Training job or hosting endpoint deployment terminate in error “ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling the CreateEndpoint operation: The account-level service limit 'ml.m4.xlarge for endpoint usage' is 0 Instances, with current utilization of 0 Instances and a request delta of 1 Instances. Please contact AWS support to request an increase for this limit.”**

 A: Try to launch a new training job with another instance type (see SageMaker instance types https://aws.amazon.com/sagemaker/pricing/instance-types/). If error persists, contact workshop support staff or your AWS account team.

 Q: **In Lab 2, we use AWS Service Catalog to create a team IAM role and CloudFormation to deploy the rest of the team resources including S3 buckets, KMS key, etc.  Why not do everything in Service Catalog?**

 A: One reason is educational, to get exposure to both services.  Another reason is that creating an IAM role requires privileged permissions to IAM service.  Service Catalog provides a great option for delegating IAM role creation in a safe manner without having to grant the Data Science Admin permission to the underlying IAM service. 

