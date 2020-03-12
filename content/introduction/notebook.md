+++
title = "Creating a Notebook Instance"
chapter = false
weight = 40
+++


SageMaker provides hosted Jupyter notebooks that require no setup, so you can begin processing your training data sets immediately. With a few clicks in the SageMaker console, you can create a fully managed notebook instance, pre-loaded with useful libraries for machine learning. You need only add your data.

You'll start by creating an Amazon S3 bucket that will be used throughout the workshop.  You'll then create a SageMaker notebook instance, which you will use for the other workshop modules.

### Create a S3 Bucket

SageMaker typically uses S3 as storage for data and model artifacts.  In this step you'll create a S3 bucket for this purpose. To begin, sign into the AWS Management Console, https://console.aws.amazon.com/.

Keep in mind that your bucket's name must be globally unique across all regions and customers. We recommend using a name like `smworkshop-firstname-lastname`. If you get an error that your bucket name already exists, try adding additional numbers or characters until you find an unused name.

1. In the AWS Management Console, choose **Services** then select **S3** under Storage.

1. Choose **Create Bucket**

1. Provide a globally unique name for your bucket such as '*smworkshop-firstname-lastname*'.

1. Select the Region you've chosen to use for this workshop from the dropdown.

1. Choose **Create** in the lower left of the dialog without selecting a bucket to copy settings from.

### Launch the Notebook Instance

1. In the upper-right corner of the AWS Management Console, confirm you are in the desired AWS region. Select N. Virginia, Oregon, Ohio, or Ireland.

2. Click on Amazon SageMaker from the list of all services.  This will bring you to the Amazon SageMaker console homepage.

    ![Services in Console](/images/sm-console-services.png)

3. To create a new notebook instance, go to **Notebook instances**, and click the **Create notebook instance** button at the top of the browser window.

    ![Notebook Instances](/images/sm-notebook-instances.png)

4. Type *smworkshop-[First Name]-[Last Name]* into the **Notebook instance name** text box, and select ml.m4.xlarge for the **Notebook instance type**.

    ![Create Notebook Instance](/images/sm-notebook-settings.png)

5. For IAM role, choose **Create a new role**, and in the resulting pop-up modal, select **Specific S3 buckets** under **S3 Buckets you specify – optional**. In the text field, paste the name of the S3 bucket you created above, AND the following bucket name separated from the first by a comma:  `gdelt-open-data`.  The combined field entry should look similar to ```smworkshop-john-smith, gdelt-open-data```. Click **Create role**.

    ![Create IAM role](/images/sm-role-popup.png)

6. You will be taken back to the Create Notebook instance page.  Click **Create notebook instance**.

### Access the Notebook Instance

1. Wait for the server status to change to **InService**. This will take several minutes, possibly up to ten but likely less.

    ![Access Notebook](/images/sm-open-notebook.png)

2. Click **Open**. You will now see the Jupyter homepage for your notebook instance.

    ![Open Notebook](/images/sm-jupyter-homepage.png)
