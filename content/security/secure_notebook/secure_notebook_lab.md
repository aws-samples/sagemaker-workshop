+++
title = "Lab 3: Deploy a Jupyter Notebook"
chapter = false
weight = 31
+++

At this point, the cloud platform engineering team has built a secure base environment to host data analytics environments.  The data science administrator has provisioned resources within that environment for your team to work, and they have provided you with an AWS Service Catalog to enable you to provision SageMaker notebooks.  Now, use these resources to provision a Jupyter notebook and start developing your ML solution.

---

## Launch the notebook product

Navigate to CloudFormation and go to the stack with the name **SC-###** (it should be the base stack, not a nested stack). Go to Outputs and click the AssumeProjectUserRole link. Assume the role and visit the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.  Launch a SageMakerNotebook product using the **Team Name** defined by the Data Science Administrator.

{{% expand "Step-by-step instructions" %}}
1. Assume the role of the ds-user by following the steps above to go to CloudFormation and clicking on the stack starting with **sc-###**.(Make sure its not a nested stack) In the Outputs section click the link next to AssumeProjectUserRole.
For the `Account` field enter your 12-digit AWS account ID.  You can find it on the [My Account](https://console.aws.amazon.com/billing/home?#/account) page.
1. Open the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.
1. Open the menu for the `SageMakerNotebook` product and click `Launch Product`
1. Give the notebook product a name such as `YOUR-NAME-ds-notebook` and click `Next`
1. Use the same team name as earlier in the labs, `team-<PRODUCT NAME>` or similar
1. For NotebookName specify `YOUR-NAME-ml-notebook` or similar
1. On the next 3 screens click `Next` and then click `Launch`.

You will land at a Provisioned Product page while the Service Catalog creates your Jupyter Notebook.  Periodically click Refresh until the Status reads `Succeeded`.  This should table about 5 minutes to launch your notebook.

![Provision Product Status](/images/launch_product_status.png)

{{% /expand %}}

![Provisioned Notebook Product](/images/provisioned_product.png)

## Access IAM
Every SageMaker notebook needs permissions granted to it to be able to access/create/delete AWS resources and APIs. Let's go ahead and take a look at the permissions that are attached to this SageMaker notebook. 

Here's the step by step:

1. Navigate to IAM in the AWS Console. 

2. In *Roles*, look for the IAM role starting with **SageMakerExecRoleArn-${TeamName}**. This role is attached to our notebook instance and has been automatically provisioned by the workshop.

3. Take a look at the policies attached to this role. 

For example, we have attached Sagemaker Full Access policy. This allows this role to call any SageMaker APIs. This role below is more permissive than what we would have in typical production environments. Here are some of the ways we may want to restrict access further. 

As a best practice, if organizations have well defined data scientist/engineer/devops roles, it is a good idea to prevent data scientists from creating endpoints or creating/deleting elastic network interfaces (ENIs). This can be done by creating a managed policy which doesn't include access to SageMaker APIs for creating endpoints. Data scientists however often need to test their models on a batch of test data for offline inferencing, and for that; batch transform can be allowed. 

Similarly, data scientists should not get S3 full access as we have done here. While we can use bucket policies to restrict access to objects in buckets, we can also prevent our SageMaker notebooks from accessing certain S3 buckets: for examples, buckets that contain production data may not be accessible to development environments.


## Access the notebook
After the notebook has launched successfully you can open it by visiting the [SageMaker console](https://console.aws.amazon.com/sagemaker/home).  With your Jupyter notebook open, familiarize yourself with the web interface and open the Notebook kernel named `00_SageMaker-SysOps-Workflow`.  Don't forget to reference back to the [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) for a quick reference.

{{% expand "Step-by-step instructions" %}}
1. When your Notebook has been created access the list of running Notebook instances on the [SageMaker console](https://console.aws.amazon.com/sagemaker/home?#/notebook-instances).  
1. Open your notebook by clicking `Open JupyterLab`.
1. With the notebook open, launch the Notebook kernel listed on the left hand side named `00_SageMaker-SysOps-Workflow.ipynb`.
{{% /expand %}}

{{% notice info %}}
When its open the Notebook kernel should use the `conda_python3` kernel.  If Jupyter asks you to set the kernel select `conda_python3` and if Jupyter displays an error, reload the Jupyter page by clicking your browser Refresh button.
{{% /notice %}}

![Jupyter Notebook Interface](/images/jupyter_notebook.png)

Execute the steps in each of the cells of both notebooks carefully reading the Markdown as you work through these notebooks until you get to ## Lab 4: Train Without VPC Configured. 

Navigate to Lab 4 to understand the ReadMe prior to executing the lab.

Once that is complete, finish the rest of this notebook. 

## Takeaways

Having completed the steps in both notebooks, you have learned how to build, train, deploy, and monitor models using Amazon SageMaker with security best practices. 

