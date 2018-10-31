# SQS
#### SQS gurantees delivery but there can be duplicates
  * SQS requires you to implement your own application-level tracking, especially if your application uses multiple queues.
# SWF
#### Maximum number of SWF domains allowed in an AWS account is 100
  * You can have a maximum of 10,000 workflow and activity types (in total) that are either registered or depreciated in each domain. 
  * You can have a maximum of 100 Amazon SWF domains(including registered and depreciated domains) in your AWS account. 
  * gurantees delivery order of message/tasks.
#### How long can workflow executions run
  * Each workflow execution can run for a maximum of 1 year. Each workflow execution history can grow up to 25,000 events.
#### Developing Deciders in Amazon SWF
  * A decider is an implementation of the **coordination logic** of your workflow type that runs during the execution of your workflow. You can run multiple deciders for a single workflow type.
# SNS
  * The various SNS endpoints for northern virginia:
    * US-East-1 (Virginia): http://sns.us-east-1.amazonaws.com 
#### What is the format of an Amazon SNS topic?
  * Topic names are limited to 256 characters. Alphanumeric characters plus hyphens (-) and underscores (_) are allowed. Topic names must be unique within an AWS account. After you delete a topic, you can reuse the topic name.
  * When a topic is created, Amazon SNS will assign a unique ARN (Amazon Resource Name) to the topic
  * The following is the ARN for a topic named “mytopic” created by a user with the AWS account ID “123456789012” and hosted in the US East region:

arn:aws:sns:us-east-1:1234567890123456:mytopic 
#### How long will subscription requests remain pending, while waiting to be confirmed?
  * Token included in the confirmation message sent to end-points on a subscription request are valid for 3 days.
#### Can a message be deleted after being published?
  * No, once a message has been successfully published to a topic, it cannot be recalled.
#### Does Amazon SNS provide at-least-once message delivery to Amazon SQS queue?
  * Yes, Amazon SNS gurantees that each message is delivered to Amazon SQS at least once. 

# Dynamo DB
#### Primary key
  * An example of a good primary key is CustomerID or User_ID if the application has many customers requests made to various customers records tend to be more or less uniform. 
  * An example of a heavily skewed primary key is "Product Category Name" where certain product categories are more popular than the rest. 
#### Atomic Counter
  * allows all write requests to be applied in the order they are recieved by incrementing or decrementing the attribute value
#### Tables limit in AWS accout
  * 256 tables per region by default. You can request an increase on this limit.
#### Query and Scan
  * both  supports eventual consistent reads.
  * returns the item matching the primary key search; return all attributes on an item 
  * much more efficient because it searches indexes only.
#### BatchGetItem
  * By default, BatchGetItem performs **eventually consistent reads** on every table in the request. If you want **strongly consistent reads** instead, you can set **ConsistentRead to true** for any or all tables.
  * A single operation can retrieve up to 16 MB of data, which can contain as many as 100 items. If you request more than 100 items BatchGetItem will return a ValidationException with the message "Too many items requested for the BatchGetItem call".
  * If DynamoDB returns any unprocessed items, you should retry the batch operation on those items. However, we strongly recommend that you use an exponential backoff algorithm.
#### GetItem
  * The GetItem operation returns a set of attributes for the item with the given primary key. If there is no matching item, GetItem does not return any data and there will be no Item element in the response.
  * GetItem provides an eventually consistent read by default. If your application requires a strongly consistent read, set ConsistentRead to true
#### Query
  * The Query operation finds items based on primary key values. You can query any table or secondary index that has a composite primary key (a partition key and a sort key).
  * The Query operation will return all of the items from the table or index with that partition key value. You can optionally narrow the scope of the Query operation by specifying a sort key value and a comparison operator in KeyConditionExpression.
  * Query results are always sorted by the sort key value. If the data type of the sort key is Number, the results are returned in numeric order; otherwise, the results are returned in order of UTF-8 bytes. By default, the sort order is ascending. To reverse the order, set the ScanIndexForward parameter to false.
  *  To further refine the Query results, you can optionally provide a FilterExpression. A FilterExpression determines which items within the results should be returned to you
  * FilterExpression is applied after a Query finishes, but before the results are returned. A FilterExpression cannot contain partition key or sort key attributes. You need to specify those attributes in the KeyConditionExpression.
### Errors 
#### ProvisionedThroughputExceededException
  * Your request rate is too high. The AWS SDKs for DynamoDB automatically retry requests that receive this exception. Your request is eventually successful, unless your retry queue is too large to finish. Reduce the frequency of requests and **use exponential backoff**. HTTP Status Code: 400
#### ResourceNotFoundException
  * The operation tried to access a nonexistent table or index. The resource might not be specified correctly, or its status might not be ACTIVE. HTTP Status Code: 400
#### InternalServerError
  * An error occurred on the server side. HTTP Status Code: 500
### Request Parameters
  * Accepts data in JSON format
#### Key
  * A map of attribute names to AttributeValue objects, representing the primary key of the item to retrieve.

For the primary key, you must provide all of the attributes. For example, with a simple primary key, you only need to provide a value for the partition key. For a composite primary key, you must provide values for both the partition key and the sort key.

Type: String to AttributeValue object map

Key Length Constraints: Maximum length of 65535.

Required: Yes
#### TableName
  * The name of the table containing the requested item.

Type: String

Length Constraints: Minimum length of 3. Maximum length of 255.

Pattern: [a-zA-Z0-9_.-]+

Required: Yes
## Secondary Index
  * A secondary index is a data structure that contains a subset of attributes from a table, along with an alternate key to support Query operations. You can retrieve data from the index using a Query, in much the same way as you use Query with a table. A table can have multiple secondary indexes, which gives your applications access to many different query patterns.
#### Local Secondary Index
  * must be created during the table creation
  * must have the same partition key as primary key but can have different sort key
  * uses the throughput of the table
  * max 5 per table
#### Global Secondary Index
  * can be created during or after table creation
  * can have different partition key and/or different sort key.
  * requires additional throughput for the index specifically
  * max 5 per table
#### How many secondary indexs are allowed per table
  * 10 ( 5 local and 5 global) per table.
#### How can you increase your DynamoDB secondary index limit in a region?
  * DynamoDB does not allow secondary index limit increase
#### Conditional Writes
  * Conditional writes are helpful in cases where multiple users attempt to modify the same item. 
#### Data types that can be indexed in DynamoDB table?
  * Number, String, Binary and Boolean can be used for sort key element of the local secondary index.
  * Set, list and map types cannot be indexed
#### What is maximum limit of size of an item collection in DynamoDB?
  * 10 GB
#### What is smallest amount of reserved capacity that can be purchased for DynamoDB?
  * 100
#### KeyConditionExpression parameter
  * expressions can be used as part of the Query API call in DynamoDB to filter results based on values of primary keys on a table using KeyConditionExpression parameter.
#### DynamoDB  stores from 1 byte upto 400KB; S3 stores upto 5TB
#### There is no limit on number of attributes an item can have in DynamoDB
#### DynamoDB API
  * CreateTable
  * DescribeTable
  * ListTables
  * UpdateTable
  * DeleteTable
  
