+++
title = "Preventive Controls"
chapter = false
weight = 60
+++

Amazon SageMaker, like other services, is goverened by Identity and Access Management (IAM).  You configure IAM using policy documents which explicitly grant permissions to your environment.

AWS IAM is based upon the concepts of Principals, Actions, Resources, and Conditions (PARC).  This allows you to specify, using IAM policies who (principals) can do what (actions) to which resources and under what circumstances (conditions).  Conditions are a powerful part of IAM policies and gives you the ability to control aspects of the actions being taken by principals in your environment.  

Using the language of principals, actions, resources, and conditions you have a rich language to configure preventive controls that govern your environment.  Preventive controls can stop an action from ever succeeding.  However, as part of Defense-in-Depth, you should also create corrective and detective controls.

Amazon SageMaker provides [numerous conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html) that allow you to control the aspects of the many SageMaker actions your users will need.  In this lab you will use some of these controls to stop data scientists from creating non-compliant training jobs.  We won't go into the full list of conditions which Amazon SageMaker makes available, but let's take a look at some of the conditions available in the context of the machine learning lifecycle.

---

## Accessing a SageMaker notebook

To access an Amazon SageMaker Jupyter notebook instance you need permissions to call `CreatePresignedNotebookInstanceUrl`.  This action creates a pre-signed URL which grants access to the notebook instance.  Both the SageMaker API call and the resulting URL are governed by the IAM conditions associated with the action.  This means that you can leverage an IAM policy such as the following to restrict from which IP address someone in your environment can access the notebook instance.

```json
{
  "Effect": "Allow",
  "Action": [
    "sagemaker:StartNotebookInstance",
    "sagemaker:StopNotebookInstance",
    "sagemaker:DescribeNotebookInstance",
    "sagemaker:CreatePresignedNotebookInstanceUrl"
  ],
  "Resource": "*",
  "Condition": {
    "IpAddress": {
      "aws:SourceIp": [
        "192.0.2.0/24",
        "203.0.113.0/24"
      ]
    },
    "StringLike": {
      "sagemaker:ResourceTag/owner": "${aws:userid}"
    }
  }
}
```

The snippet above will allow someone to start, stop, and access a SageMaker Jupyter notebook as long as they do so from a specific IP CIDR range and if the notebook has been tagged with their user as the `owner`.

## Requiring VPC attachment

Actions for creating SageMaker resources such as notebooks, training jobs, and machine learning models can also have IAM conditions associated with them.  This gives you the ability to require that principals create these resources with a VPC configuration.  

```json
"Condition": {
  "ForAllValues:StringEqualsIfExists": {
    "sagemaker:VpcSubnets": [
      "subnet-0de4766fbf8d2ea38",
      "subnet-0c974f191edc71a33"
    ],
    "sagemaker:VpcSecurityGroupIds": [
      "sg-02dc127e01d6b1407"
    ]
  }
}
```

The condition statement above specifies that IF an action has subnets and security groups configured the values provided must be within an approved set of values.  In particular someone creating a Jupyter notebook instance, for example, would have to specify at least one of the subnets above and the security group specified.  If any provided values were outside of the specified set the condition would evaluate to False and the permission be denied.  

Another condition is to ensure that if a VPC configuration CAN be provided that it IS provided.  You can do this using the Null condition.

```json
"Condition": {
  "Null": {
    "sagemaker:VpcSubnets": "true"
  }
}
```

The condition above evaluates to True if there is no subnet configuration specified for an API call.  This will enforce that any principal invoking the SageMaker action provides a VPC configuration and the previous condition will ensure that the configuration is within an acceptable set of values.

A complete list of conditions and the SageMaker actions they are associated with can be found in the [Amazon SageMaker documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html).

## VPC Endpoint policies

In addition to associating IAM policies with users, groups, or roles you can also use the same policy language to create VPC endpoint policies.  Endpoint policies control what actions can be taken using a VPC endpoint.  Amazon SageMaker is accessible via VPC endpoints and so you can further enforce policies at the endpoint level to govern how users in your environments can access the SageMaker APIs.  This adds to your ability to create defense in depth and ensure best practice in your environment.

---

Now that you understand IAM policies and the conditions you can use to control user permissions, work through this lab to use IAM policy language to create a preventive control that will lead to an improved developer experience.  Recall that as a data scientist you were waiting for the training job to start and eventually be stopped by the remediating detective control.  On repeat this could be a slow and unpleasant way to discover you made an error.  Use a preventive control to provide clear and immediate feedback to a developer to let them know when a mistake has been made.