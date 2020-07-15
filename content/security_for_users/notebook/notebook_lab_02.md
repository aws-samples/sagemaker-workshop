+++
title = "Lab 04: DevOps Workflow"
chapter = false
weight = 30
+++

In notebook `02_SageMaker-DevOps-Workflow.ipynb`, you will complete the machine learning lifecycle and deliver a model into production.  As a DevOps Engineer you will pick up the model trained by the data scientists and deploy it as an endpoint.  You will also set up model monitoring on the endpoint for detecting data drift.  This notebook is primarily focused on two aspects of running machine learning models in production, monitoring the model's performance and being able to demonstrate the model's lineage.

---

## Model Monitoring

When a model is operating in production issues can arise that can be difficult to detect.  For instance invalid data inputs can be sent to your model for inference, if those invalid data inputs have an invalid data type that can be easily detected, but if the value is a numeric value that is greater than any value seen previously in training, how would you detect that?  Another issue could be what's called concept drift, meaning that the patterns the model was trained to detect are no longer relevant for the world in which the model is operating.  For instance, if a model was trained to detect fraud but the patterns of fraud have shifted in response to increased detection, the model would no longer be as accurate as it once was.  Amazon SageMaker offers a monitoring capability to detect erroneous inputs and concept drift.  In this notebook you will use Model Monitor to capture the inputs and outputs of your model and to provide reports on its observations of your model.

Amazon SageMaker Model Monitor can detect the following violations:

 - Invalid input or output data types
 - Detection of uncharacteristic levels of null values
 - Data distribution of input or output data is outside of baseline data distribution
 - Missing or extra input values
 - Unknown categorical values

For more information about the types of violations and their meanings please see the [Model Monitor documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-interpreting-violations.html).

Note that the Model Monitor is scheduled to execute periodically.  As a result you may have to give the monitor up to an hour to execute itself as you work through the notebook.  Below you will find example screen captures of monitor output, similar to what your monitoring job will show after time.

{{% notice info %}}
These graphics are generated using SageMaker Studio UI. 
{{% /notice %}}

The image below shows a sample Pandas dataframe showing some of the violations which Model Monitor can detect. Note, these may not be the exact final violations you see. For example, below we have also modified the Marriage column. 

![ModelMonitor DataFrame](/images/MM_dataframe.png)

This same dataframe is shown below as a graph.  You can see that there is a large drift in the MARRIAGE column, but over time the service also detects some drift in the Label distribution. SageMaker Studio can also directly plot the actual drift as shown in the BILL_AMT figure instead of comparing the baseline to the observed values. 

![Marriage](/images/Marriage_drift.png)


![Label](/images/Label_drift.png)


![Bill](/images/Bill_drift.png)


By setting thresholds for acceptable drift, you can decide when to retrain your models. 

## Model Lineage

As a best practice, you will commit the notebook to a git repo that is already provisioned, and call SageMaker Experiments APIs to track any model metadata, container artifacts, data and code lineage from the raw code to the production endpoint.

As you near the end of the notebook you will reach **Part 8: Lineage**. Here you will generate a dataframe that contains the lineage of your trained model, tagging the experiment with relevant Git commit ids and details. 

---

Congratulations, you have now taken raw data and an algorithm to produce a trained model for your business challenge.  You have deployed this model and monitored it for drift, with the ability to demonstrate the lineage of the model in terms of when it was trained, which version of the code was used, etc.  
