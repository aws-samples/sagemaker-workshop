+++
title = "Secure Networking"
chapter = false
weight = 10
+++

Amazon SageMaker allows you to create resources attached to your [AWS Virtual Private Cloud](https://aws.amazon.com/vpc/) (VPC).  This allows you to govern access to SageMaker resources and your data sets using familiar tools such as security groups, routing tables, and VPC endpoints.  Using these network-layer tools you can create a secure network environment that allows you to explicitly control the data ingress and egress of your data science environment.  Please take a few moments and read about these tools in more detail.

---

## Private Network Environment

Begin with your VPC which will be used to host Amazon SageMaker and other components of your data science environment.  Your VPC provides a familiar set of network-level controls to allow you to govern ingress and egress of data.  You will begin this lab by creating a VPC with no Internet Gateway (IGW), therefore all subnets will be private, without Internet connectivity.  Network connectivity with AWS services or your own shared services will be provided using VPC endpoints powered by PrivateLink.  Security Groups will be used to control traffic between different resources, allowing you to group like resources together and manage their ingress and egress traffic.

## Virtual Private Cloud

A Virtual Private Cloud (VPC) gives you a self-contained network environment that you control.  When initially created the VPC does not allow network traffic into or out of the VPC.  It's only by adding VPC endpoints, Internet Gateways (IGW), or Virtual Private Gateways (VPGW) that you begin to configure your private network environment to communicate with the wider world.  For the rest of these labs you can assume that no access to the internet is required and that all communication with AWS services will be done explicitly through private connectivity to the AWS APIs through VPC endpoints.  It is also recommend to create multiple subnets in your VPC in multiple availability zones to support highly available deployments and resilient architectures.

To find out more about VPCs and VPC concepts such as routing tables, subnets, security groups, and network access control lists please visit the [AWS documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html).

## VPC Endpoints

A VPC endpoint allows you to establish a private, secure connection between your VPC and an AWS service without requiring you to configure an Internet Gateway, NAT device, or VPN connection.  Using VPC endpoints your VPC resources can communicate with AWS services without ever leaving the AWS network.  A VPC endpoint is a highly available virtual device that is managed on your behalf.  As a VPC resource an endpoint is given IP addresses within your VPC and security groups assigned to the endpoint can control who can communicate with the endpoint.  

![VPC Endpoint](/images/vpc_endpoint.png)

In addition to security groups you can also apply VPC endpoint policies to an AWS service endpoint.  An endpoint policy is an IAM resource policy that gets applied to a VPC endpoint and governs which APIs can be called on an AWS service through the endpoint.  For example, if the following endpoint policy were applied to an Amazon S3 VPC endpoint it would only allow read access to objects in the specified S3 bucket.  Actions against other buckets would be denied.

```json
{
  "Statement": [
    {
      "Sid": "Access-to-specific-bucket-only",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::my_secure_bucket",
        "arn:aws:s3:::my_secure_bucket/*"
      ]
    }
  ]
}
```

VPC endpoint policies, along with security groups, provide the ability to implement Defense-in-Depth and bring a multi-layered approach to security and who can access what resources within your VPC.  

In this lab you will create a VPC with endpoints for the following services:

 - Amazon S3, for reading and writing data 
 - Amazon SageMaker, for creating training jobs and hosted models
 - Amazon STS, for obtaining temporary credentials
 - Amazon CloudWatch Logs, for writing out log data from VPC-based resources

---

Use the previously created AWS Service Catalog products to create an AWS Virtual Private Cloud, security groups, and VPC endpoints to configure a precise, secure network environment to support your data science project teams.  The VPC will have no IGW or NAT gateway attached and it will have multiple subnets across 3 availability zones.  VPC endpoints will be created and attached to the VPC and security groups applied to control what VPC resources can communicate with each other.
