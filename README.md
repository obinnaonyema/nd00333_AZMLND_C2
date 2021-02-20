#Operationalizing A Machine Learning Model

##Overview
In this project, we take a bank marketing data set and train a model to predict the likelihood of a customer to subscribe to a product. After this we deploy this model which exposes an http endpoint for authenticated users to consume and we create documentation using swagger. Finally we create and deploy a pipeline.

The data set contains 20 columns and 32,950 rows from results of a previous campaign. It contains details of customers such as age, education, marital status and so on.

##Architecture

[!Architecture]()

Since a private Azure subscription was used for this, a service principal "ml-auth" was created for the purpose of authentication. Authentication without human intervention allows for more seamless operation in a production environment. The best model from the AutoML run was deployed and tested by sending sample requests. A pipeline was published and swagger documentation was created for the deployed model.

##Key Steps




