+++
title = "Self-Service with Guard Rails"
chapter = false
weight = 20
+++

Allowing your developers to self-service and provision cloud resources on-demand is a powerful enabler for project teams but can be a concern from an operational risk perspective.  However, if you can empower your developers to self-service, while enforcing guard rails and best practice, then the operational and security teams will also benefit.  With enforced guard rails and best practice you can be confident that, while developers are creating resources they need, they are doing so in a manner that is inline with your policies and requirements.  

---

## Identity & Accesss Management

AWS services are interacted with via a RESTful API.  Every call into this API is authorized by the [AWS Identity & Access Managment](https://aws.amazon.com/iam/) (IAM) service.  You control AWS IAM and grant users in your environment explit permissions to use various AWS services.  You grant explicit permissions through IAM policy documents which specify the principal (who), the actions (API calls), and the resources (such as S3 objects) that are allowed, as well as under what conditions such access is granted.  

```json
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "s3:PutObject",
            "s3:GetObject",
            "s3:GetObjectVersion",
            "s3:DeleteObject",
            "s3:DeleteObjectVersion"
         ],
         "Resource":"arn:aws:s3:::my_corporate_bucket/home/${aws:userid}/*"
      }
   ]
}
```

The example policy document above grants a user permissions to read or write to an S3 bucket, but only to a sub-folder that matches the user's user ID.  We can see that the `Effect` explicitly allows the `Action`s on a single `Resource`.  

Amazon SageMaker has a [comprehensive set of conditions and actions](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam.html) which you can grant to users in your environment.  You can grant users the ability to start, stop, or access notebook servers, create training jobs, or host models in production for example.  You can also use an array of conditions such as:

 - `sagemaker:VolumeKmsKey`, to require that an encryption key is specified
 - `sagemaker:VpcSecurityGroupIds`, to require that a VPC configuration is provided
 - `sagemaker:DirectInternetAccess`, to ensure that notebooks do not have Internet access
 - `sagemaker:NetworkIsolation`, to ensure that models or training jobs cannot communicate with the network

 In this lab you will create an IAM role and a set of permissions to grant the data science team least-privilege access to the data science environment.

## AWS Service Catalog

[AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) allows you to create and manage collections of IT products and services that you have configured and parameterized.  These IT products and services can include everything from virtual machine images, servers, software, and databases to complete multi-tier application architectures. AWS Service Catalog allows you to centrally manage commonly deployed IT services, and helps you achieve consistent governance and meet your compliance requirements, while enabling users to quickly deploy only the approved IT services they need.

AWS Service Catalog provides a single location where you can centrally manage catalogs of IT products and services. With AWS Service Catalog you can control which IT services and versions are available, the configuration of the available services, and permission access by individual, group, department, or cost center.

To get started you begin by creating a [portfolio](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/catalogs_portfolios.html) which represents a collection of products and configuration information.  It allows you to specify who can access the products you define and how they can use them.

Once you've created a portfolio you then begin to define [products](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/catalogs_products.html) which describe a product or service that your users can deploy onto AWS.  A product will consist of one or more AWS resources like an Amazon EC2 instance, databases, or Jupyter notebooks.  These products are described mostly through a CloudFormation template and can be altered by the user at deployment time based upon the values of parameters you define for the user to tailor.

---

In this lab you will create a shared PyPI mirror for approved Python packages along with a Service Catalog product to enable project administrators to quickly deploy data science environments using approved controls.  Project teams can then use the environments to self-service, creating Jupyter Notebook instances for themselves on-demand and in compliance with your policies.