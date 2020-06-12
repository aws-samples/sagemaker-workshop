+++
title = "Overview of Notebook 02"
chapter = false
weight = 4
+++

In notebook 02_SageMaker-DevOps-Workflow, you will assume the role of a DevOps Engineer. You will pick up the model trained by the data scientists and deploy it as an endpoint.  You will also set up model monitoring on the endpoint for detecting data drift. 

As a best practice, you will commit the notebook to a git repo that is already provisioned, and call SageMaker Experiments APIs to track any model metadata, container artifacts, data and code lineage from the raw code to the production endpoint.

Let's get started. 

Once you run all the cells in this notebook you will reach the Reproducibility Section. Here you will generate a dataframe that contains the lineage of our trained model, tagging the Git Commits. To do so, you first need to commit something to CodeCommit. Here you will mimic the data scientist commiting their training code to Git and the DevOps Engineer commiting their deployment code to Git. 

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

## Model Monitoring

Note that the ModelMonitor service runs a CRON job, so it looks for violations and drift from the baseline on the hour. You might have to run the MM service for a number of hours before you start to see the drift from the columns that we have modified. Here are some images of what the output of ModelMonitor looks like once its running for a while. These graphics are generated using SageMaker Studio UI. 

A dataframe view shows all the drift that the service detects.

![ModelMonitor DataFrame](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/MM_dataframe.png)

We see that there is a clearly large drift in the MARRIAGE column, but over time the service also detects some drift in the Label distribution and other columns such as the BILL_AMT.

![Marriage](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/Marriage_drift.png)


![Label](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/Label_drift.png)


![Bill](https://github.com/stefannatu/sagemaker-workshop/blob/master/static/images/Bill_drift.png)


By setting thresholds for acceptable drift, you can decide when to retrain your models. 











