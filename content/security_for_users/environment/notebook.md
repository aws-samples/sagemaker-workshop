+++
title = "Launch a notebook"
chapter = false
weight = 2
+++

Now that a data science environment has been provisioned for your project team you can navigate to the Service Catalog and request that a Jupyter notebook be created for you.

---

1. From the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) open the details for the data science project environment you provisoined as a Project Administrator.  
1. Locate the link to assume the role of a Data Science User and assume the role.
1. Now, as a Data Science User, return to the [Service Catalog console](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/products) and launch a SageMaker Notebook product.

    {{% expand "Step-by-Step instructins" %}}
1. Click the context menu button in the upper right corner of the SageMaker Notebook product and click `Launch Product`.
1. Give the product a name such as `my-sagemaker-nb` and click `Next`.
1. Provide a **NotebookOwnerEmail** to be associated with any git commits.
1. Enter the ProjectName that was used in the previous lab, such as `project-abc`.
1. Enter a NotebookOwnerUsername to be associated with any git commits and click `Next`.
1. Enter no Tag Options and click `Next`.
1. Do not configure Notifications and click `Next`.
1. On the Review page click `Launch`.
    {{% /expand %}}

{{% notice info %}}
After approximately 5 minutes the Jupyter notebook will become available and the provisioned product outputs will include a link to open the Jupyter notebook. 
{{% /notice %}}

---

You have now created a Jupyter notebook server within the secure data science environment created for your project.  Continue to the next set of labs using the Jupyter notebook.