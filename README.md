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

Train a model and deloy it to an Endpoint on AWS. In this respository we are taking data from Kaggle Bike Sharing Demand Dataset
Reference: https://www.kaggle.com/c/bike-sharing-demand/data

You can find the Notebook file which is used to analyze and Deploy to an endpoint in the xgboost folder of the repository.
Given below is an image showing information on various files in that folder.

![image](https://user-images.githubusercontent.com/3585863/82162024-40cce280-9856-11ea-99fb-751f0c46ec48.png)

Once the model is deployed , A lambda function will be used to act as a trigger between API calls through API getway to the model endpoint
and response from the model endpoint back to API gateway.
Required Transformations such as converting data into format which the specific model can consume is done by the lambda functions.

### The code for creating the lambda function is in Integration examples for sagemaker folder.

API gateway need to configured to get PUT requests and show be configured to push/ recieve information from the Lambda function created above.




