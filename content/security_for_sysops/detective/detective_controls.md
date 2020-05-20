+++
title = "Detective Controls"
chapter = false
weight = 40
+++

AWS recommends a [defense-in-depth](https://wa.aws.amazon.com/wat.pillar.security.en.html) approach to security, applying security at every level of your application and environment.  In this section we will focus on the concept of detective controls and incident response in the form of a corrective control.  A detective control is responsible for identifying potential security threats or incidents while a corrective control is responsible for limiting the potential damage of the threat or incident after detection.

One example of a form of detective control is the use of internal auditing to ensure that an environment and user practice is inline with your policies and requirements.  These controls can help your organization identify and understand the scope of anomalous activity.

Many AWS services are available to help you implement detective controls.  AWS CloudTrail records AWS API calls, AWS Config provides a detailed inventory of your AWS resources and configuration. Amazon GuardDuty is a managed threat detection service that continuously monitors for malicious or unauthorized behavior. Amazon CloudWatch is a monitoring service for AWS resources which can trigger CloudWatch Events to automate security responses.

Even with detective controls, you should still put processes in place to respond to and mitigate the potential impact of security incidents. 

Amazon CloudWatch Events allows you to create rules that trigger automated responses such as the execution of AWS Lambda.

## Amazon CloudWatch Events

Amazon CloudWatch recieves metrics, logs, and [events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) and can, in near-real time, respond to this information on your behalf.  You can use log entries from applications or AWS services to automatically notify a member of your team when an error has occurred.  You can use metrics to scale up or scale out your infrastructure when a CPU is exceeding a utilization threshold.  Or you can inspect a launching EC2 instance that is the result of an API call into AWS.  

Logs, metrics, and events can trigger a number of responses such as emailing alerts or executing a Lambda function in response.  

A list of AWS services and the events that they send to CloudWatch can be found in the [documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html).  Within Amazon CloudWatch you can configure an [events rule](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) which can detect anomalous activity and trigger a response. 

The response that is triggered can be any of a number of activities to include executing an AWS Lambda function, an Amazon ECS task, or an SSM RunCommand.  This gives you flexibility to automate responses to incidents in your environment.

---

The detective and corrective controls made possible by CloudWatch Events and AWS Lambda allow you to inspect SageMaker training jobs, EC2 instance security groups, and much more.  By detecting resources that are not in line with your standards you can quickly respond to and correct the resource using automated incident response.
