+++
title = "Lab 02: Deploy a Notebook"
chapter = false
weight = 31
+++

Your project team has been presented with IAM roles and a Service Catalog Portfolio to allow your team to self service and obtain resources to support your efforts.  Following the steps below use this portfolio to create a Jupyter notebook instance for yourself.

---

## Launch the notebook product

Click into the details for the [data science environment product](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) you provisioned in the last step and find the link to assume the role of a data science user.  After assuming this role return to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) and launch an Amazon SageMaker Jupyter notebook.

{{% expand "Step-by-Step instructions" %}}
1. From the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) open the details for the data science project environment you provisoined as a Project Administrator.  
1. Locate the link to assume the role of a Data Science User and assume the role.
1. Now, as a Data Science User, return to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/products) and launch a SageMaker Notebook product.
1. Click the context menu button in the upper right corner of the SageMaker Notebook product and click `Launch Product`.
1. Give the product a name such as `my-sagemaker-nb` and click `Next`.
1. Provide a **NotebookOwnerEmail** to be associated with any git commits.
1. Enter the ProjectName that was used in the previous lab, such as `project-abc`.
1. Enter a NotebookOwnerUsername to be associated with any git commits and click `Next`.
1. Enter no Tag Options and click `Next`.
1. Do not configure Notifications and click `Next`.
1. On the Review page click `Launch`.

You will land at a Provisioned Product page while the Service Catalog creates your Jupyter Notebook.  Periodically click Refresh until the Status reads `Succeeded`.  This should take about 5 minutes to launch your notebook.

![Provision Product Status](/images/launch_product_status.png)

{{% /expand %}}

![Provisioned Notebook Product](/images/provisioned_product.png)

## IAM Permissions

Every SageMaker notebook has permissions granted to it to be able to access, create, and delete AWS resources and APIs.  These permissions are granted through an IAM role associated with the Jupyter notebook's EC2 instance.  An example of the permissions associated with the notebook are highlighted in the next set of labs. 

The IAM role assigned to your notebook has been created just for your notebook and represents an association of the Jupyter notebook to both yourself and your project.  This will be represented in audit logs, identifying that actions were taken by your Jupyter notebook and allow for easy tracking of which notebook, for which project, belonging to which project team member performed an action on AWS resources.  

## Access the notebook

After the notebook product has finished provisioning you can open it by clicking the `NotebookUrl` link provided as an *Output* in the provisioned product detail.  With your Jupyter notebook open, familiarize yourself with the web interface and open the Notebook kernel named `01_SageMaker-DataScientist-Workflow.ipynb`.  Don't forget to reference back to the [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) for a quick reference if you need one.

{{% expand "Step-by-step instructions" %}}
1. From the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) open the link for the notebook product you provisioned.
1. Under *Outputs*, click the link for `NotebookUrl` to launch the Jupyter notebook interface.
1. With the notebook open, launch the Notebook kernel listed on the left hand side named `01_SageMaker-DataScientist-Workflow.ipynb`.
{{% /expand %}}

{{% notice info %}}
When its open the Notebook kernel should use the `conda_python3` kernel.  If Jupyter asks you to set the kernel select `conda_python3` and if Jupyter displays an error, reload the Jupyter page by clicking your browser Refresh button.
{{% /notice %}}

![Jupyter Notebook Interface](/images/jupyter_notebook.png)

---

You have now created a Jupyter notebook dedicated to yourself as a member of the project team.  With the notebook open you are ready to take on the next set of labs where you will use the notebook and the data science environment to engineer a feature set, train a model, deploy the model, and then monitor the model for performance or drift.
