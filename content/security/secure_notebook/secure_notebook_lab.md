+++
title = "Lab 3: Deploy a Jupyter Notebook"
chapter = false
weight = 31
+++

At this point, the cloud platform engineering team has built a secure base environment to host data analytics environments.  The data science administrator has provisioned resources within that environment for your team to work, and they have provided you with an AWS Service Catalog to enable you to provision SageMaker notebooks.  Now, use these resources to provision a Jupyter notebook and start developing your ML solution.

---

## Launch the notebook product

Assume the role of the [Data Scientist](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientist&displayName=DataScientist) and visit the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.  Launch a SageMakerNotebook product using the **Team Name** defined by the Data Science Administrator.

{{% expand "Step-by-step instructions" %}}
1. Assume the role of the [Data Scientist](https://signin.aws.amazon.com/switchrole?account=000000000000&roleName=DataScientist&displayName=DataScientist) 
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

## Access the notebook
After the notebook has launched successfully you can open it by visiting the [SageMaker console](https://console.aws.amazon.com/sagemaker/home).  With your Jupyter notebook open, familiarize yourself with the web interface and open the Notebook kernel named `stockmarket_predictor_v5.ipynb`.  Don't forget to reference back to the [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) for a quick reference.

{{% expand "Step-by-step instructions" %}}
1. When your Notebook has been created access the list of running Notebook instances on the [SageMaker console](https://console.aws.amazon.com/sagemaker/home?#/notebook-instances).  
1. Open your notebook by clicking `Open JupyterLab`.
1. With the notebook open, launch the Notebook kernel listed on the left hand side named `stockmarket_predictor_v5.ipynb`.
{{% /expand %}}

{{% notice info %}}
When its open the Notebook kernel should use the `conda_tensorflow_p27` kernel.  If Jupyter asks you to set the kernel select `conda_tensorflow_p27` and if Jupyter displays an error, reload the Jupyter page by clicking your browser Refresh button.
{{% /notice %}}

![Jupyter Notebook Interface](/images/jupyter_notebook.png)

Read through the notebook and in the next lab we will execute the notebook cells to exercise the security controls of our environment.
