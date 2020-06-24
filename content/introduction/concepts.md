+++
title = "Machine Learning with Amazon SageMaker"
chapter = false
weight = 10
+++


This section describes a typical machine learning workflow and summarizes how you accomplish those tasks with Amazon SageMaker.

In machine learning, you "teach" a computer to make predictions, or inferences. First, you use an algorithm and example data to train a model. Then you integrate your model into your application to generate inferences in real time and at scale. In a production environment, a model typically learns from millions of example data items and produces inferences in hundreds to less than 20 milliseconds.

The following diagram illustrates the typical workflow for creating a machine learning model:

![Image NOT FOUND](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ml-concepts-10.png)

 As the diagram illustrates, you typically perform the following activities:

### Generate example data

To train a model, you need example data. The type of data that you need depends on the business problem that you want the model to solve (the inferences that you want the model to generate). For example, suppose you want to create a model to predict a number given an input image of a handwritten digit. To train such a model, you need example images of handwritten numbers.

Data scientists often spend a lot of time exploring and preprocessing, or "wrangling," example data before using it for model training. To preprocess data, you typically do the following:

   1. **Fetch the data**— You might have in-house example data repositories, or you might use datasets that are publicly available. Typically, you pull the dataset(s) into a single repository.

   1. **Clean the data**—To improve model training, inspect the data and clean it up as needed. For example, if your data has a `country name` attribute with values `United States` and `US`, you might want to edit the data to be consistent.

   1. **Prepare or transform the data**—You might perform additional data transformations to improve performance. For example, you might choose to combine attributes. If your model predicts the conditions that require de-icing an aircraft, instead of using temperature and humidity attributes separately, you might combine them into a new attribute to get a better model.

   In Amazon SageMaker, you preprocess example data in a Jupyter notebook on your notebook instance. You use your notebook to fetch your dataset, explore it and prepare it for model training. You'll [create a Notebook Instance](./notebook.html) in the next section.

### Train a model

Model training includes both training and evaluating the model, as follows:

  + **Training the model**—To train a model, you need an algorithm. The algorithm you choose depends on a number of factors. In Amazon SageMaker, you have the following options for a training algorithm:

    + **Use an algorithm provided by Amazon SageMaker**—Amazon SageMaker provides training algorithms. If one of these meets your needs, it's a great out-of-the-box solution for quick model training. For a list of algorithms provided by Amazon SageMaker, see the [Amazon SageMaker documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html). You'll use some of the built-in SageMaker algorithms in the [Using Built-in Algorithms](../builtin.html) module.
  
    + **Use Apache Spark with Amazon SageMaker**—Amazon SageMaker provides a library that you can use in Apache Spark to train models with Amazon SageMaker. Using the library provided by Amazon SageMaker is similar to using Apache Spark MLLib. For more information, see [Using Apache Spark with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/apache-spark.html).
  
    + **Submit custom code to train with deep learning frameworks**—You can submit custom Python code that uses TensorFlow or Apache MXNet for model training. You'll see an example of using Apache MXNet with Amazon SageMaker in the [Using Custom Algorithms](../custom.html) module.
  
    + **Use your own custom algorithms**—Put your code together as a Docker image and specify the registry path of the image in an Amazon SageMaker `CreateTrainingJob` API call. For more information, see the [Amazon SageMaker documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html). You'll see an example of how to use your own algorithm in the [Using Custom Algorithms](../custom.html) module.

  + **Evaluating the model**—After you've trained your model, you evaluate it to determine whether the accuracy of the inferences is acceptable. You can evaluate your model using historical data (offline) or live data:

    + **Offline testing**—Use historical, not live, data to send requests to the model for inferences.
      Deploy your trained model to an alpha endpoint, and use historical data to send inference requests to it. To send the requests, use a Jupyter notebook in your Amazon SageMaker notebook instance and either the AWS SDK for Python (Boto) or the high-level Python library provided by Amazon SageMaker.

    + **Online testing with live data**—Amazon SageMaker supports multiple models (called production variants) to a single Amazon SageMaker endpoint. You configure the production variants so that a small portion of the live traffic goes to the model that you want to validate. For example, you might choose to send 10% of the traffic to a model variant for evaluation. After you are satisfied with the model's performance, you can route 100% traffic to the updated model.

  You use a Jupyter notebook in your Amazon SageMaker notebook instance to train and evaluate your model.

### Deploy the model

Traditionally, you do some re-engineering of a model to integrate it with your application, before deploying the model into production. With Amazon SageMaker hosting services, you can deploy your model independently, decoupling it from your application code. For more information, see [Deploying a Model on Amazon SageMaker Hosting Services](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-hosting.html).

Machine learning is a continuous cycle. After deploying a model, you monitor the inferences, then collect "ground truth," and evaluate the model to identify drift. You then increase the accuracy of your inferences by updating your training data to include the newly collected ground truth, by retraining the model with the new dataset. As more and more example data becomes available, you continue retraining your model to increase accuracy over time.
