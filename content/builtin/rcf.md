+++
title = "Anomaly Detection with Random Cut Forest"
chapter = false
weight = 3
+++

In this section, you'll work your way through a Jupyter notebook that demonstrates how to use a built-in algorithm in SageMaker. More specifically, you'll use SageMaker's [Random Cut Forest (RCF)](https://docs.aws.amazon.com/sagemaker/latest/dg/randomcutforest.html) algorithm, an algorithm designed to detect anomalous data points within a dataset. Examples of when anomalies are important to detect include when website activity uncharacteristically spikes, when temperature data diverges from a periodic behaviour, or when changes to public transit ridership reflect the occurrence of a special event.

In this notebook, we will use the SageMaker RCF algorithm to train a model on the [Numenta Anomaly Benchmark (NAB) NYC Taxi dataset](https://github.com/numenta/NAB/blob/master/data/realKnownCause/nyc_taxi.csv) which records the amount New York City taxi ridership over the course of six months. We will then use this model to predict anomalous events by emitting an "anomaly score" for each data point. 

### Running the notebook

1. Access the SageMaker notebook instance you created earlier. Open the [**SageMaker Examples**](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-nbexamples.html) tab.

2. In the *Introduction to Amazon Algorithms* section locate the *random_cut_forest.ipynb* notebook and create a copy by clicking on **Use**.

3. You are now ready to begin the notebook.

4. In the ```bucket = '<<bucket_name>>'``` code line, paste the name of the S3 bucket you created in Module 1 to replace ```<<bucket_name>>```.  The code line should now read similar to ```bucket = 'smworkshop-john-smith'```.  Do NOT paste the entire path (s3://.......), just the bucket name.  

5. Follow the directions in the notebook. The notebook will walk you through the data preparation, training, hosting, and validating the model with Amazon SageMaker. Once completed, return from the notebook to these instructions to move to the next Module.
