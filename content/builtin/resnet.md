+++
title = "Image Classification with ResNet"
chapter = false
weight = 2
+++

In this section, you'll work your way through a Jupyter notebook that demonstrates how to use a built-in algorithm in SageMaker. More specifically, you'll use SageMaker's [image classification](https://docs.aws.amazon.com/sagemaker/latest/dg/image-classification.html) algorithm, a supervised learning algorithm that takes an image as input and classifies it into one of multiple output categories. It uses a convolutional neural network (ResNet) that can be trained from scratch, or trained using transfer learning when a large number of training images are not available. In this section you'll train the image classification algorithm from scratch on the [Caltech-256 ](http://resolver.caltech.edu/CaltechAUTHORS:CNS-TR-2007-001) dataset.

### Running the notebook

1. Download the [image-classification-fulltraining.ipynb](../notebooks/image-classification-fulltraining.ipynb) notebook.

1. Access the SageMaker notebook instance you created earlier. Click the **New** button on the right and select **Folder**.  

2. Click the checkbox next to your new folder, click the **Rename** button above in the menu bar, and give the folder a name such as '*image-classification-resnet*'.

3. Click the folder to enter it.

4. To upload the notebook, click the **Upload** button on the right. Then in the file selection popup, select the file '*image-classification-fulltraining.ipynb*' from the folder on your computer where you downloadeded it earlier. Click the blue **Upload** button that appears to the right of the notebook's file name.

5. You are now ready to begin the notebook. Click the notebook's file name to open it.

6. In the ```bucket = '<<bucket_name>>'``` code line, paste the name of the S3 bucket you created in Module 1 to replace ```<<bucket_name>>```.  The code line should now read similar to ```bucket = 'smworkshop-john-smith'```.  Do NOT paste the entire path (s3://.......), just the bucket name.  

7. Follow the directions in the notebook. The notebook will walk you through the data preparation, training, hosting, and validating the model with Amazon SageMaker. Once completed, return from the notebook to these instructions to move to the next Module.
