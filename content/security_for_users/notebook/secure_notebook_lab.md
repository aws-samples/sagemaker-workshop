+++
title = "Lab 3: Deploy a Jupyter Notebook"
chapter = false
weight = 31
+++

At this point, the cloud platform engineering team has built a secure base environment to host data analytics environments.  The data science administrator has provisioned resources within that environment for your team to work, and they have provided you with an AWS Service Catalog to enable you to provision SageMaker notebooks.  Now, use these resources to provision a Jupyter notebook and start developing your ML solution.

As a data scientist working in a regulated industry such as financial services, your Cloud Platform engineering team has created a secure environment for you. That said, data scientists need to ensure that their notebooks follow best practices in 8 key areas:

1. **Compute and Network Isolation**

The platform team has already done this for us.

2. **Authentication and Access Management**

The platform team has already provisioned a role for you, we will look at it in more detail in the notebooks. 

3. **Artifact Management**

It is up to the data scientist to ensure that the notebooks, data is being stored in the proper buckets that have been provisioned for us. Bucket access policies and security has been set by the platform team so data scientists can focus on the science.

4. **Data Encryption**

You will learn how to ensure that all training/processing volumes are passed appropriate encryption keys to ensure data encryption at rest. Also we will see how to use Network configurations to ensure that data is encrypted in transit and doesn't traverse the internet.

5. **Traceability and Auditability**

You will learn how to use SageMaker Experiments to easily maintain the lineage of your trained models and training jobs. SageMaker Experiments will automatically manage the metadata associated with your training jobs and surface it in a data frame. Within SageMaker Studio, this is natively integrated into the Studio UI.

6. **Explainability and Interpretability**

Once you train your ML model, we will use SHAP values to derive feature importances. 

7. **Real Time Model Monitoring**

You will learn how to monitor your production endpoints in real time for data drift

8. **Reproducibility**

For audit reasons, you may need to reproduce your work at a later time. To ensure reproducibility, you will see how to use SageMaker experiments in combination with CodeCommit to version your code and track the entire lineage of your models.

Now let's get started.

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
After the notebook has launched successfully you can open it by visiting the [SageMaker console](https://console.aws.amazon.com/sagemaker/home).  With your Jupyter notebook open, familiarize yourself with the web interface and open the Notebook kernel named `01_SageMaker-DataScientist-Workflow.ipynb`.  Don't forget to reference back to the [Jupyter cheat sheet](https://www.edureka.co/blog/cheatsheets/jupyter-notebook-cheat-sheet) for a quick reference.

{{% expand "Step-by-step instructions" %}}
1. When your Notebook has been created access the list of running Notebook instances on the [SageMaker console](https://console.aws.amazon.com/sagemaker/home?#/notebook-instances).  
1. Open your notebook by clicking `Open JupyterLab`.
1. With the notebook open, launch the Notebook kernel listed on the left hand side named `01_SageMaker-DataScientist-Workflow.ipynb`.
{{% /expand %}}

{{% notice info %}}
When its open the Notebook kernel should use the `conda_python3` kernel.  If Jupyter asks you to set the kernel select `conda_python3` and if Jupyter displays an error, reload the Jupyter page by clicking your browser Refresh button.
{{% /notice %}}

![Jupyter Notebook Interface](/media/jupyter_notebooks.png)

This lab comprises of two noteboks: the first: `01_SageMaker-DataScientist-Workflow.ipynb` will cover a typical Data Scientist workflow and show you how to explore data, pre-process data, train an XGBoost model using a custom container and explore feature importances for that model in a secure manner, maintaining network traffic via our private VPC and enforing encryption at rest and in transit. Furthermore, you will learn how to using SageMaker Processing for running processing jobs at scale, and leverage Spot instance pricing to save on training costs.

In the second notebook, `02_SageMaker-DevOps-Workflow.ipynb`, we will deploy the trained model from the SageMaker notebook to production and monitor the endpoint for data drift using ModelMonitor. Finally we will use SageMaker Experiments to track any model metadata, code commits etc from our repo to trace the lineage of our models and endpoints.

Execute the steps in each of the cells of both notebooks carefully reading the Markdown as you work through these notebooks. 

Once you complete the `01_SageMaker-DataScientist-Workflow.ipynb` notebook, move on to `02_SageMaker-DevOps-Workflow.ipynb`, and execute the cells there until you reach **Part 8:Reproducibility**

## Part 8: Reproducibility (Optional)

In the last part of Notebook 2, we will output a dataframe that contains the lineage of our trained model, tagging the Git Commits. To do so, we first need to commit something to CodeCommit. Here we will mimic the data scientist commiting their training code to Git and the DevOps Engineer commiting their deployment code to Git. 

Navigate to your Jupyter environment which contains these notebooks and the code. In the drop down **New**, click on **Terminal**.

In the Terminal window, change directory to the directory for your project source code.  This will be a directory similar to `/home/ec2-user/SageMaker/ds-source-MyProject-MyEnv`.  From this directory execute the following commands: 

```bash
$ cd ~/SageMaker/< your project source directory >
$ git add 01_SageMaker-DataScientist-Workflow.ipynb
$ git commit -m "Added Trained model" 
$ git push -u origin master
$ git add 02_SageMaker-DevOps-Workflow.ipynb
$ git commit -m  "Added Model Deployment Notebook"
$ git push -u origin master
$ git log --pretty=oneline # you should see two logs for both commits. 
```
You should see a log containing your commitIDs. We will now load this Commit metadata to SageMaker Experiments for tracking.

Now go back to the `02_SageMaker-DevOps-Workflow.ipynb` and enter your commitIDs in the `sclineage` and `endpointlineage`, and execute the next cell.

You should see a dataframe containing the lineage history of your Preprocessing job, Experiments, as well as the Commit information to Git. 

This can now be fed into a database for tracking metadata associated with any model training jobs to maintain model governance and provenance by combining SageMaker Experiments training jobs tracking with Git / CodeCommit source and version control APIs.

## Takeaways

Having completed the steps in both notebooks, you have learned how to build, train, deploy, and monitor models using Amazon SageMaker with security best practices. 

