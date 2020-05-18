# Machine learning on AWS

![image](https://user-images.githubusercontent.com/3585863/82161582-dd8d8100-9852-11ea-948f-8d3ca437ae80.png)

Above is the architecture on how to build a machine learning workflow on AWS . This repository has code which can be used 
to build a machine learning workflow to enable the Model running on a endpoint to be called by external application through API gateway
or to store the model results on a S3 bucket which in turn can be visualized using Athena and Tableau .

### Housekeeping/Environment Setup 

### Anaconda Python 

Download and Install Anaconda Python in your local machine 
Recommended: Python 3.6 or later 
https://www.anaconda.com/distribution/ 
Install Instructions: Windows, macOS, Linux 
To Test the installation, from Anaconda Prompt, run the command: 
python --version 

### Boto3 SDK Install 

Install Boto3 SDK in your local machine 
From Anaconda Prompt, run the command: 
conda install -c anaconda boto3 
https://anaconda.org/anaconda/boto3 

### SageMaker SDK Install 

Install SageMaker SDK in your local machine. 
From Anaconda Prompt, run the command: 
pip install sagemaker 
https://github.com/aws/sagemaker-python-sdk 

### Clone Repository 
You can now clone the source code for this content to your local machine 
From your local directory, run this command 
git clone 

### Update Security Permissions 

SageMaker SDK now requires couple of additional permissions. 
Logon with my_admin account and open IAM Management Console 

Look for Policies 

Search for SageMakerInvokeEndpoint policy.  
If you have not already created a SageMakerInvokeEndpoint policy then follow the below steps to create one.

https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html

Edit Policy 

Select JSON and paste the below JSON policy, review and save the changes 
{ 
"Version": "2012-10-17", 
"Statement": [ 
{ 
"Sid": "VisualEditor0", 
"Effect": "Allow", 
"Action": [ 
"sagemaker:DescribeEndpointConfig", 
"sagemaker:DescribeEndpoint", 
"sagemaker:InvokeEndpoint" 
], 
"Resource": "*" 
} 
] 
}

## Invoking Endpoint through API Gateway
![image](https://user-images.githubusercontent.com/3585863/82161871-26ded000-9855-11ea-8a9e-73b7b7b27185.png)

This requires Three major services on AWS namely Sagemaker,lambda and API Gateway.

This approach is useful for scenarios of Sub second latency

Train a model and deloy it to an Endpoint on AWS. In this respository we are taking data from Kaggle Bike Sharing Demand Dataset
Reference: https://www.kaggle.com/c/bike-sharing-demand/data

You can find the Notebook file which is used to analyze and Deploy to an endpoint in the xgboost folder of the repository.
Given below is an image showing information on various files in that folder.

![image](https://user-images.githubusercontent.com/3585863/82162024-40cce280-9856-11ea-99fb-751f0c46ec48.png)

Once the model is deployed , A lambda function will be used to act as a trigger between API calls through API getway to the model endpoint
and response from the model endpoint back to API gateway.
Transformations such as data wrangling,formatting etc is done by the lambda functions.

### The code for creating the lambda function is in Integration examples for sagemaker folder.

API gateway needs to configured to get PUT requests and should be configured to push/ recieve information from the Lambda function created above.

## Running and Saving Scheduled Inferences to S3 for visualizing in Quicksight or Tableau

![image](https://user-images.githubusercontent.com/3585863/82166719-2bfc4900-986e-11ea-9339-a3d4f4c1c0a2.png)

For scenarios where subsecond latency is not required , Inferences can be run on a scheduled basis or when new data flows into S3. These inferences are triggered by a lambda function when new data flows into S3. This lambda function will make the required transformations need to the data before running it against a specific model endpoint. The results are stored in the S3 bucket and Athena/EMR can be used on top of it to query the results and visualize that in Tableau or any other data analytics software.

### Here in this example
we will look at Matson Navigation stock prices for last 20 years ( data downloaded from Yahoo finance) and predict using a Linear learner model. this will be solving a binary classification problem. We will use the concept of adding lags for 1 week of data to look for trends. This Model will save the results in a S3 bucket . A lambda function similar to bike training use case can be used to run the data against the hosted endpoint whenever new data moves into the specific S3 bucket it will trigger the lambda function ( image below)
![image](https://user-images.githubusercontent.com/3585863/82166822-8b5a5900-986e-11ea-9b57-d33768e90698.png)

The notebook for the Stock prediction can be found in the Matson Stock Prediction Folder.

## Monitoring and Alerting using Cloudwatch

All the deployed Model in Production can be monitored for the following Metrics. 
1. Input data quality
2. Algorithm specific Metrics
3. Model accuracy or loss function results during training phases
4. Distributional characteristics of predictions, classifications, and/or parameter estimate
5. CPU, memory, and GPU utilization

Refer to the following Link : https://aws.amazon.com/blogs/machine-learning/use-amazon-cloudwatch-custom-metrics-for-real-time-monitoring-of-amazon-sagemaker-model-performance/

AWS also offer a service specifically designed for Model performance called Sagemaker Model Monitor. 
![image](https://user-images.githubusercontent.com/3585863/82167062-61edfd00-986f-11ea-8735-bb62badb9b0a.png)

More details here : 
https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html#model-monitor-how-it-works

# Future Topics:
1. Deploying Custom Machine Learning Models  built on various Frameworks by building custom Docker containers.
2. Deploying Multiple Models to single Endpoint.
3. Hyper Parameter Tuning jobs – Schedule .
4. Batch Transform Millions of records and get inferences for them .
5. Amazon SageMaker Studio
6. Amazon Model Monitor
7. Amazon AutoPilot
