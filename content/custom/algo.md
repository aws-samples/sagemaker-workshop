+++
title = "Use your own custom algorithms"
chapter = false
weight = 3
+++

In this section, you'll create your own training script using TensorFlow and the building blocks provided in tf.layers, which will predict the ages of abalones based on their physical measurements. It's possible to estimate the age of an [abalone](https://en.wikipedia.org/wiki/Abalone) (sea snail) by the number of rings on its shell. In this section, you'll be using the [UCI Abalone](https://archive.ics.uci.edu/ml/datasets/Abalone) dataset.

### Writing Custom TensorFlow Model Training and Inference Code

To train a model on Amazon SageMaker using custom TensorFlow code and deploy it on Amazon SageMaker, you need to implement training and inference code interfaces in your code.

Your TensorFlow training script must be a Python 2.7 source file. The current default TensorFlow version is 1.6. This training/inference script must contain the following functions:

  + `model_fn`: Defines the model that will be trained.
  + `train_input_fn`: Preprocess and load training data.
  + `eval_input_fn`: Preprocess and load evaluation data.
  + `serving_input_fn`: Defines the features to be passed to the model during prediction.

For more information, see [TensorFlow Model Training Code](https://docs.aws.amazon.com/sagemaker/latest/dg/tf-training-inference-code-template.html) in the Amazon SageMaker documentation.

#### Defining the model

The ``model_fn`` is a function that contains all the logic to support training, evaluation, and prediction. The basic skeleton for a ``model_fn`` looks like this:

```
def model_fn(features, labels, mode, hyperparameters):
  # Logic to do the following:
  # 1. Configure the model via TensorFlow operations
  # 2. Define the loss function for training/evaluation
  # 3. Define the training operation/optimizer
  # 4. Generate predictions
  # 5. Return predictions/loss/train_op/eval_metric_ops in EstimatorSpec object
  return EstimatorSpec(mode, predictions, loss, train_op, eval_metric_ops)
```

The ``model_fn`` function must accept four positional arguments:

- **features**: A dict containing the features passed to the model via ``train_input_fn`` in **training** mode, via ``eval_input_fn`` in **evaluation** mode, and via ``serving_input_fn`` in **predict** mode.
- **labels**: A `Tensor` containing the labels passed to the model via ``train_input_fn`` in **training** mode and ``eval_input_fn`` in **evaluation** mode. It will be empty for **predict** mode.
- **mode**: One of the following ``tf.estimator.ModeKeys`` string values indicating the context in which the ``model_fn`` was invoked:
  - **TRAIN**: the ``model_fn`` was invoked in **training** mode.
  - **EVAL**: the ``model_fn`` was invoked in **evaluation** mode.
  - **PREDICT**: the ``model_fn`` was invoked in **predict** mode.
- **hyperparameters**: The hyperparameters passed to SageMaker TrainingJob that runs your TensorFlow training script. You can use this to pass hyperparameters to your training script.

The ``model_fn`` function must return a ``tf.estimator.EstimatorSpec``.

More details on how to create a ``model_fn`` can be find in [Constructing the model_fn](https://github.com/tensorflow/tensorflow/blob/r1.4/tensorflow/docs_src/extend/estimators.md#constructing-the-model_fn-constructing-modelfn).

#### Training and Evaluation

The ``train_input_fn`` function is used to pass *features* and *labels* to the ``model_fn`` in **training** mode. The ``eval_input_fn`` function is used to *features* and *labels* to the ``model_fn`` in **evaluation** mode.

The basic skeleton for the ``train_input_fn`` looks like this:

```
def train_input_fn(training_dir, hyperparameters):
  # Logic to the following:
  # 1. Reads the **training** dataset files located in training_dir
  # 2. Preprocess the dataset
  # 3. Return 1)  a dict of feature names to Tensors with
  # the corresponding feature data, and 2) a Tensor containing labels
  return features, labels
```

An ``eval_input_fn`` follows the same format:

```
def eval_input_fn(training_dir, hyperparameters):
  # Logic to the following:
  # 1. Reads the **evaluation** dataset files located in training_dir
  # 2. Preprocess the dataset
  # 3. Return 1)  a dict of feature names to Tensors with
  # the corresponding feature data, and 2) a Tensor containing labels
  return features, labels
```

> **Note:** For TensorFlow 1.4 and 1.5, ``train_input_fn`` and ``eval_input_fn`` may also return a no-argument function which returns the tuple ``features, labels``. This is no longer supported for TensorFlow 1.6 and up.

More details on how to create input functions can be find in [Building Input Functions with tf.estimator](https://github.com/tensorflow/tensorflow/blob/r1.4/tensorflow/docs_src/get_started/input_fn.md#building-input-functions-with-tfestimator).

#### Serving the Model

The ``serving_input_fn`` function is used to define the shapes and types of the inputs the model accepts when the model is exported for Tensorflow Serving. It is optional, but required for deploying the trained model to a SageMaker endpoint.

The ``serving_input_fn`` function is called at the end of model training and is **not** called during inference.

The basic skeleton for the ``serving_input_fn`` looks like this:

```
def serving_input_fn(hyperparameters):
  # Logic to the following:
  # 1. Defines placeholders that TensorFlow serving will feed with inference requests
  # 2. Preprocess input data
  # 3. Returns a tf.estimator.export.ServingInputReceiver or tf.estimator.export.TensorServingInputReceiver,
  # which packages the placeholders and the resulting feature Tensors together.
```

> **Note:** For TensorFlow 1.4 and 1.5, ``serving_input_fn`` may also return a no-argument function which returns a ``tf.estimator.export.ServingInputReceiver`` or``tf.estimator.export.TensorServingInputReceiver``. This is no longer supported for TensorFlow 1.6 and up.

More details on how to create a `serving_input_fn` can be find in [Preparing serving inputs](https://github.com/tensorflow/tensorflow/blob/r1.4/tensorflow/docs_src/programmers_guide/saved_model.md#preparing-serving-inputs).

### Running the notebook

1. Download the [tensorflow_abalone_age_predictor.zip](../notebooks/tensorflow_abalone_age_predictor.zip) file. This archive contains the Python script for your model, the Jupyter notebook to step through the process, and the UCI Abalone dataset.

1. Access the SageMaker notebook instance you created earlier. Click the **New** button on the right and select **Folder**.  

2. Click the checkbox next to your new folder, click the **Rename** button above in the menu bar, and give the folder a name such as '*tensorflow-abalone-byom*'.

3. Click the folder to enter it.

4. Click the **Upload** button on the right. Then in the file selection popup, select the file '*tensorflow_abalone_age_predictor.zip*' from the folder on your computer where you downloadeded it earlier. Click the blue **Upload** button that appears to the right of the notebook's file name.

5. Click the **New** button on the right and select **Terminal**. In the terminal window, type the following commands to unzip the archive:

    ```
    cd SageMaker
    unzip tensorflow_abalone_age_predictor.zip
    ```

5. Close the Terminal window. You are now ready to begin the notebook. Click the notebook's file name to open it.

7. Follow the directions in the notebook. The notebook will walk you through the data preparation, training, hosting, and validating the model with Amazon SageMaker. Once completed, return from the notebook to these instructions to move to the next Module.
