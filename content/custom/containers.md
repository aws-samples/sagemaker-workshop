+++
title = "Overview of containers for Amazon SageMaker"
chapter = false
weight = 5
+++

SageMaker makes extensive use of Docker containers to allow users to train and deploy algorithms. Containers allow developers and data scientists to package software into standardized units that run consistently on any platform that supports Docker. Containerization packages code, runtime, system tools, system libraries and settings all in the same place, isolating it from its surroundings, and insuring a consistent runtime regardless of where it is being run.

When you develop a model in Amazon SageMaker, you can provide separate Docker images for the training code and the inference code, or you can combine them into a single Docker image.

### Anatomy of an Amazon SageMaker container

![Containers](/images/sm-containers.gif)

Using this powerful container environment, developers can deploy any kind of code in the Amazon SageMaker ecosystem. You can also create a logical division of labor by creating a deployment team, such as a DevOps, security, and infrastructure teams (who maintains the container) and a data scientists team (who focuses on producing models that are later added to the container).

### Example container folder
The [Bring Your Own scikit Algorithm](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/scikit_bring_your_own) example provides a detailed walkthrough on how to package a scikit-learn algorithm for training and production-ready hosting using containers. Let's take a look at the *container* folder structure to explain how Amazon SageMaker runs Docker for training and hosting your own algorithm.

```
container/
  Dockerfile
  build_and_push.sh
  decision_trees/
      nginx.conf
      predictor.py
      serve
      train
      wsgi.py
```

Let's discuss each of these in turn:

  * `Dockerfile` describes how to build your Docker container image. More details below.
  * `build_and_push.sh` is a script that uses the `Dockerfile` to build your container images and then pushes it to Amazon ECR.
  * `decision_trees` is the directory which contains the files that will be installed in the container.

In this simple application, only five files are installed in the container. You may only need that many or, if you have many supporting routines, you may wish to install more. These five show the standard structure of our Python containers, although you are free to choose a different toolset and therefore could have a different layout. If you're writing in a different programming language, you'll certainly have a different layout depending on the frameworks and tools you choose.

The files that will be installed in the container are:

  * `nginx.conf` is the configuration file for the nginx front-end.
  * `predictor.py` is the program that actually implements the Flask web server and the decision tree predictions for this app.
  * `serve` is the program started when the container is started for hosting. It simply launches the gunicorn server which runs multiple instances of the Flask app defined in `predictor.py`.
  * `train` is the program that is invoked when the container is run for training.
  * `wsgi.py` is a small wrapper used to invoke the Flask app.


#### The Dockerfile

The Dockerfile describes the image that you want to build. You can think of it as describing the complete operating system installation of the system that you want to run. A Docker container running is quite a bit lighter than a full operating system, however, because it takes advantage of Linux on the host machine for the basic operations.

For the Python science stack, we will start from a standard Ubuntu installation and run the normal tools to install the things needed by scikit-learn. Finally, we add the code that implements our specific algorithm to the container and set up the right environment to run under.

Along the way, we clean up extra space. This makes the container smaller and faster to start.

Let's look at the Dockerfile for the example:

```
# Build an image that can do training and inference in SageMaker
# This is a Python 2 image that uses the nginx, gunicorn, flask stack
# for serving inferences in a stable way.

FROM ubuntu:16.04

MAINTAINER Amazon AI <sage-learner@amazon.com>


RUN apt-get -y update && apt-get install -y --no-install-recommends \
         wget \
         python \
         nginx \
         ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Here we get all python packages.
# There's substantial overlap between scipy and numpy that we eliminate by
# linking them together. Likewise, pip leaves the install caches populated which uses
# a significant amount of space. These optimizations save a fair amount of space in the
# image, which reduces start up time.
RUN wget https://bootstrap.pypa.io/get-pip.py && python get-pip.py && \
    pip install numpy scipy scikit-learn pandas flask gevent gunicorn && \
        (cd /usr/local/lib/python2.7/dist-packages/scipy/.libs; rm *; ln ../../numpy/.libs/* .) && \
        rm -rf /root/.cache

# Set some environment variables. PYTHONUNBUFFERED keeps Python from buffering our standard
# output stream, which means that logs can be delivered to the user quickly. PYTHONDONTWRITEBYTECODE
# keeps Python from writing the .pyc files which are unnecessary in this case. We also update
# PATH so that the train and serve programs are found when the container is invoked.

ENV PYTHONUNBUFFERED=TRUE
ENV PYTHONDONTWRITEBYTECODE=TRUE
ENV PATH="/opt/program:${PATH}"

# Set up the program in the image
COPY decision_trees /opt/program
WORKDIR /opt/program
```

### Running a container for Amazon SageMaker training

Amazon SageMaker invokes the training code by running a version of the following command:

```
docker run <image> train
```

This means that your Docker image should have an executable file in it that is called `train`. You will modify this program to implement your training algorithm. This can be in any language that is capable of running inside of the Docker environment, but the most common language options for data scientists include Python, R, Scala, and Java. For our Scikit example, we use Python.

At runtime, Amazon SageMaker injects the training data from an Amazon S3 location into the container. The training program ideally should produce a model artifact. The artifact is written, inside of the container, then packaged into a compressed tar archive and pushed to an Amazon S3 location by Amazon SageMaker.

When Amazon SageMaker runs training, your `train` script is run just like any regular program. A number of files are laid out for your use, under the `/opt/ml` directory:

```
/opt/ml
├── input
│   ├── config
│   │   ├── hyperparameters.json
│   │   └── resourceConfig.json
│   └── data
│       └── <channel_name>
│           └── <input data>
├── model
│   └── <model files>
└── output
    └── failure
```

#### The input

  * `/opt/ml/input/config` contains information to control how your program runs. `hyperparameters.json` is a JSON-formatted dictionary of hyperparameter names to values. These values will always be strings, so you may need to convert them. `resourceConfig.json` is a JSON-formatted file that describes the network layout used for distributed training. Since scikit-learn doesn't support distributed training, we'll ignore it here.
  * `/opt/ml/input/data/<channel_name>/` (for File mode) contains the input data for that channel. The channels are created based on the call to CreateTrainingJob but it's generally important that channels match what the algorithm expects. The files for each channel will be copied from S3 to this directory, preserving the tree structure indicated by the S3 key structure.
  * `/opt/ml/input/data/<channel_name>_<epoch_number>` (for Pipe mode) is the pipe for a given epoch. Epochs start at zero and go up by one each time you read them. There is no limit to the number of epochs that you can run, but you must close each pipe before reading the next epoch.

#### The output

  * `/opt/ml/model/` is the directory where you write the model that your algorithm generates. Your model can be in any format that you want. It can be a single file or a whole directory tree. SageMaker will package any files in this directory into a compressed tar archive file. This file will be available at the S3 location returned in the DescribeTrainingJob result.
  * `/opt/ml/output` is a directory where the algorithm can write a file failure that describes why the job failed. The contents of this file will be returned in the FailureReason field of the DescribeTrainingJob result. For jobs that succeed, there is no reason to write this file as it will be ignored.


### Running a container for SageMaker hosting

Amazon SageMaker invokes hosting service by running a version of the following command

```
docker run <image> serve
```

This launches a RESTful API to serve HTTP requests for inference. Again, this can be done in any language or framework that works within the Docker environment.

In most Amazon SageMaker containers, `serve` is simply a wrapper that starts the inference server. Furthermore, Amazon SageMaker injects the model artifact produced in training into the container and unarchives it automatically.

Amazon SageMaker uses two URLs in the container:

  * `/ping` will receive GET requests from the infrastructure. Your program returns 200 if the container is up and accepting requests.
  * `/invocations` is the endpoint that receives client inference POST requests. The format of the request and the response is up to the algorithm. If the client supplied ContentType and Accept headers, these will be passed in as well.

### Storing SageMaker Containers

For SageMaker to run a container for training or hosting, it needs to be able to find the image hosted in the image repository, Amazon Elastic Container Registry (Amazon ECR). The three main steps to this process are building locally, tagging with the repository location, and pushing the image to the repository.

To build the local image, call the following command:

```
docker build <image name>
```

This takes instructions from the Dockerfile we discussed earlier to generate an image on your local instance. After the image is built, we need to let our local instance of Docker know where to store the image so that SageMaker can find it. We do this by tagging the image with the following command:

```
docker tag <image name> <repository name>
```

The repository name has the following structure:

```
<account number>.dkr.ecr.<region>.amazonaws.com/<image name>:<tag>
```

Without tagging the image with the repository name, Docker defaults to uploading to Docker Hub, and not Amazon ECR. Amazon SageMaker currently requires Docker images to reside in Amazon ECR. To push an image to ECR, and not the central Docker registry, you must tag it with the registry hostname.

Unlike Docker Hub, Amazon ECR images are private by default, which is a good practice with Amazon SageMaker. If you want to share your Amazon SageMaker images publicly, you can find more information in the Amazon ECR User Guide.

Finally, to upload the image to Amazon ECR, with the Region set in the repository name tag, call the following command:

```
docker push <repository name>
```

One final note on Amazon SageMaker Docker containers. We have already shown you that you have the option to build one Docker container serving both training and hosting, or you can build one for each. While building two Docker images can increase storage requirements and cost due to duplicated common libraries, you might get a benefit from building a significantly smaller inference container, allowing the hosting service to scale more quickly when reacting to traffic increases. This is especially common when you use GPUs for training, but your inference code is optimized for CPUs. You need to consider the tradeoffs when you decide if you want to build a single container or two.
