## The folder has the following
1. Notebook file which pulls in the data file , creates lags in the data, runs it against Linear learner and creates an endpoint
2. Code for Lambda to make the required transformations and run it againt the model endpoint.

Note : Please follow the below steps to upload the script to lambda because if the file is larger than 15 mb , lambda will not display the 
content and we need to upload the file with code for libraries being used in the code.

https://docs.aws.amazon.com/lambda/latest/dg/python-package.html


