+++
title = "Lab 3: Deploy a Jupyter Notebook"
chapter = false
weight = 31
+++

At this point, the cloud platform engineering team has built a self-service mechanism to provision secure environments to host data science projects.  The project administrators have provisioned resources using the self-service mechanism for your team to work, and they have provided you with a self-service mechanism to enable you to provision SageMaker notebooks.  Now, use these resources to provision a Jupyter notebook and start developing your ML solution.

---

## Launch the notebook product

Navigate to the AWS Service Catalog console and on go to the detail page for the recently provisioned data science environment.  Use the hyperlink labeled `AssumeProjectUserRole` under *Outputs* to assume the role of a data science user. Assume the role and visit the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.  Using the notebook product defined for you by the project administrators, launch a Notebook product using the same project name that was used to create the environment.

{{% expand "Step-by-step instructions" %}}
1. Open the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?isSceuc=true#/stacks) provisioned products list.
1. Click on the provisioned data science project and locate the hyperlinks in the Output section of the provisioned product detail.
1. Click on the AssumeProjectUserRole hyperlink.
1. Click `Assume Role` on the next screen.
1. Return to the [Service Catalog](https://console.aws.amazon.com/servicecatalog/home?#/products) product listing.
1. Open the menu for the SageMaker notebook product and click `Launch Product`
1. Give the notebook product a name such as `YOUR-NAME-ds-notebook` and click `Next`
1. Use the same project name as earlier in the labs
1. Enter an email address for `NotebookOwnerEmail` and a username for `NotebookOwnerUsername`
1. Click `Next` and on the next 2 screens click `Next`
1. On the `Review` screen click `Launch`

You will land at a Provisioned Product page while the Service Catalog creates your Jupyter Notebook.  Periodically click Refresh until the Status reads `Succeeded`.  This should take about 5 minutes to launch your notebook.

![Provision Product Status](/images/launch_product_status.png)

{{% /expand %}}

![Provisioned Notebook Product](/images/provisioned_product.png)

## Access the notebook
After the notebook has launched successfully you can open it by clicking the `NotebookUrl` hyperlink in the Outputs section of the provisoined notebook details page. With your Jupyter notebook open, familiarize yourself with the web interface and open the Notebook kernel named `00_SageMaker-SysOps-Workflow`.  Don't forget to reference back to the [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) for a quick reference if you need one.

{{% notice info %}}
When its open the Notebook kernel should use the `conda_python3` kernel.  If Jupyter asks you to set the kernel select `conda_python3` and if Jupyter displays an error, reload the Jupyter page by clicking your browser Refresh button.
{{% /notice %}}

---

You will now learn about methods for implementing detective and corrective controls on AWS.  In the next lab you will return to the Jupyter notebook to test the detective controls.
