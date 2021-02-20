# Operationalizing A Machine Learning Model

## Overview
In this project, we take a bank marketing data set and train a model to predict the likelihood of a customer to subscribe to a product. After this we deploy this model which exposes an http endpoint for authenticated users to consume and we create documentation using swagger. Finally we create and deploy a pipeline.

The data set contains 20 columns and 32,950 rows from results of a previous campaign. It contains details of customers such as age, education, marital status and so on.

## Architecture

![Architecture](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/architecture.png)

Since a private Azure subscription was used for this, a service principal "ml-auth" was created for the purpose of authentication. Authentication without human intervention allows for more seamless operation in a production environment. The best model from the AutoML run was deployed and tested by sending sample requests. A pipeline was published and swagger documentation was created for the deployed model.

## Key Steps
### Authentication
Authentication is done by way of a service principal. As earlier stated, using a service principal eliminates the need for human intervention thereby ensuring smoother operation of the application in production. Service principal named "ml-auth" was created via command line:

![Service Principal Created](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/service_principal_created.PNG)

Service principal was assigned to the workspace for the experiment:

![Workspace share](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/az_ml_workspace_share.PNG)

### Training the Model
The bank marketing data set was uploaded for use in training the model via AutoML.

![Registered data sets](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/registered_datasets.PNG)

An AutoML experiment was created to run on compute cluster with Standard_DS12_V2 machines. Setting minimum number of nodes to 1 eliminates start up time of the machine when the experiment is created although it came at a cost as I was using a private Azure subscription.

![Model training completed](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/model_training_completed.PNG)

The best model was a voting ensemble with accuracy of 0.91775.

### Deploying the Best Model

The best model was deployed and application insights enabled. This was very useful for me later on in the project when I had to consume the endpoint and kept getting an http 502 response. I described this error a bit in the screen cast.

![Application insights](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/app_insights_enabled.PNG)

### Enable logging

Logging is a powerful tool for a DevOps engineer. It helps with troubleshooting issues and with understanding the context of an application activity. I enabled logging as shown below:

![enable logging](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/logs_screen.PNG)

### Consume Model Endpoint

A sample request was built using the endpoint.py file. I updated the scoring URI and application key which was obtained from the endpoints section in ML Studio.

![endpoint response](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/endpoint_json_output.PNG)

I struggled a bit here as I was getting an http response 502. I had to drill down using application insights to find out I had been sending input data with only 13 values as against the expected 20. I was using the endpoints.py data in the exercise starter files which wasn't the same as the endpoints.py data in starter files folder. I made this correction and got the expected json response.

### Create Swagger Documentation

Using swagger.sh and the swagger.json file from the deployed model I was able to set up swagger documentation for the endpoint to our deployed model. 

![swagger on localhost](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/swagger_localhost.PNG)

![swagger response body](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/swagger_response.PNG)

### Publish Pipeline

Using the notebook provided, we created a pipeline that can be used to train the model. The cells were updated accordingly and pipeline run initiated

![pipeline created](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/pipeline_created.PNG)

Run details widget shows pipeline run in progress:

![run details widget](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/run_details.PNG)

Once completed, the pipeline was published. Image shows pipeline endpoint and active status.

![published pipeline](https://github.com/obinnaonyema/nd00333_AZMLND_C2_Operationalize_Model_in_Azure/blob/master/starter_files/Images/published_pipeline_overview.PNG)

## Screencast

View my video recording on Youtube [here](https://youtu.be/Azw8JpAlM-g)

## Future Improvements
The experiment could be allowed to run for longer than 1 hour although this comes at additional costs. Longer runs may yield better results.
Future campaigns can try to balance out under-represented populations in the data such as unmarried people




