# SQS
#### SQS gurantees delivery but there can be duplicates
  * SQS requires you to implement your own application-level tracking, especially if your application uses multiple queues.
# SWF
#### Maximum number of SWF domains allowed in an AWS account is 100
  * You can have a maximum of 10,000 workflow and activity types (in total) that are either registered or depreciated in each domain. 
  * You can have a maximum of 100 Amazon SWF domains(including registered and depreciated domains) in your AWS account. 
# Dynamo DB
#### Primary key
  * An example of a good primary key is CustomerID or User_ID if the application has many customers requests made to various customers records tend to be more or less uniform. 
  * An example of a heavily skewed primary key is "Product Category Name" where certain product categories are more popular than the rest. 
  
