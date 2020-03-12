+++
title = "Submit custom code"
chapter = false
weight = 1
+++

In this section, you will train a neural network locally on the location from where this notebook is run (typically the SageMaker Notebook instance) using MXNet. You will then see how to create an endpoint from the trained MXNet model and deploy it on SageMaker. You will then inference from the newly created SageMaker endpoint. For this section, you'll be using the [MNIST](http://yann.lecun.com/exdb/mnist/) dataset.

### Running the notebook

1. Download the [mxnet_mnist_byom.zip](../notebooks/mxnet_mnist_byom.zip) file. This archive contains the Python script to train your model and the Jupyter notebook to step through the process. Unzip this on your local environment.

1. Access the SageMaker notebook instance you created earlier. Click the **New** button on the right and select **Folder**.  

2. Click the checkbox next to your new folder, click the **Rename** button above in the menu bar, and give the folder a name such as '*mxnet-mnist-byom*'.

3. Click the folder to enter it.

4. To upload the notebook, click the **Upload** button on the right. Then in the file selection popup, select the file '*mxnet_mnist_byom.ipynb*' from the folder on your computer where you downloadeded it earlier. Click the blue **Upload** button that appears to the right of the notebook's file name. Repeat this step for the '*mnist.py*' and the '*input.html*' files.

5. You are now ready to begin the notebook. Click the notebook's file name to open it.

7. Follow the directions in the notebook. The notebook will walk you through the data preparation, training, hosting, and validating the model with Amazon SageMaker. Once completed, return from the notebook to these instructions to move to the next Module.
