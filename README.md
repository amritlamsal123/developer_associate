# SQS
#### SQS gurantees delivery but there can be duplicates
  * SQS requires you to implement your own application-level tracking, especially if your application uses multiple queues.
#### Default visibility timeout for SQS queue in seconds?
  * 30 seconds (The minimum is 0 seconds. The maximum is 12 hours)
  * When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message.Thus, the consumer must delete the message from the queue after receiving and processing it.
#### How do I configure the maximum message size for Amazon SQS?
  * limit to a value between 1,024 bytes (1 KB), and 262,144 bytes (256 KB).
  * To send messages larger than 256 KB, use the Amazon SQS Extended Client Library for Java. 
  * For size over 256 KB, store the information in S3 and attach retrieval information to the message for the application to process.
#### How do I configure Amazon SQS to support longer message retention?
  * You can use the MessageRetentionPeriod attribute to set the message retention period from 60 seconds (1 minute) to 1,209,600 seconds (14 days)
# SWF
#### Programming language SDK's available for SWF
  * Java, Ruby, .NET and PHP
####  Can I use AWS Identity and Access Management (IAM) to manage access to Amazon SWF? 
  * Yes. You can grant IAM users permission to access Amazon SWF. IAM users can only access the SWF domains and APIs that you specify.
#### Maximum number of SWF domains allowed in an AWS account is 100
  * You can have a maximum of 10,000 workflow and activity types (in total) that are either registered or depreciated in each domain. 
  * You can have a maximum of 100 Amazon SWF domains(including registered and depreciated domains) in your AWS account. 
  * gurantees delivery order of message/tasks.
#### How long can workflow executions run
  * Each workflow execution can run for a maximum of 1 year. Each workflow execution history can grow up to 25,000 events.
#### In SWF what are the containers called for segregating application resources.
  * Domains
    * In SWF, you define logical containers called domains for your application resources. Domains can only be created at the level of your AWS account and may not be nested
#### Developing Deciders in Amazon SWF
  * A decider is an implementation of the **coordination logic** of your workflow type that runs during the execution of your workflow. You can run multiple deciders for a single workflow type.
#### True about SWF
  * Human can perform an activity task, but not decision task. 
#### Some core benefits of SWF
  * One of the major use case of SWF is video encoding
  * Centralize the coordination of steps in the application
  * Automate the workflow that include human tasks
  * Manage the flow of work between application components
  * Integrate a range of programs and components 
  * help developers use asynchronous programming in the development of their applications
# SNS
  * The various SNS endpoints for northern virginia:
    * US-East-1 (Virginia): http://sns.us-east-1.amazonaws.com 
#### What are appropriate ways for you to provide timely, device-specific instructions to end users when annoncing this downtime?
  * Send a single message, but customize the text in the SNS message field so that each device gets only the information that is appropriate for them.
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
#### NOTE: An item is a collection of attributes. Each attributes has a name and value. An attribute value can be a scalar, a set, or a document type.
#### Primary key
  * An example of a good primary key is CustomerID or User_ID if the application has many customers requests made to various customers records tend to be more or less uniform. 
  * An example of a heavily skewed primary key is "Product Category Name" where certain product categories are more popular than the rest. 
#### Which DynamoDB API call doesn't consume capacity units?
  * UpdateTable
#### Throughput capacity for Reads and Writes
  * When you create a table or index in Amazon DynamoDB, you must specify your capacity requirements for read and write activity
  * One read capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. If you need to read an item that is larger than 4 KB, DynamoDB will need to consume additional read capacity units. The total number of read capacity units required depends on the item size, and whether you want an eventually consistent or strongly consistent read.
  * One write capacity unit represents one write per second for an item up to 1 KB in size. If you need to write an item that is larger than 1 KB, DynamoDB will need to consume additional write capacity units. The total number of write capacity units required depends on the item size.
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
#### NOTE: You cannot create more than one table with a secondary index at a time.
#### GSI (Global Secondary Index):
  * A GSI is an index with a hash and range key that can be different from those on the table. 
#### Data types that can be indexed in DynamoDB table?
  * Number, String, Binary and Boolean can be used for sort key element of the local secondary index.
  * Set, list and map types cannot be indexed
#### What is maximum limit of size of an item collection in DynamoDB?
  * 10 GB
#### What is smallest amount of reserved capacity that can be purchased for DynamoDB?
  * 100
#### KeyConditionExpression parameter
  * expressions can be used as part of the Query API call in DynamoDB to filter results based on values of primary keys on a table using KeyConditionExpression parameter.
#### You're using a CloudFormation template to build out staging environments. What section of the CloudFormation would you edit in order to allow the user to specify the PEM key-name at start time?
  * Parameter Section
  * Use the optional Parameters section to customize your templates. Parameters enable you to input custom values to your template each time you create or update a stack
#### What would you set in CloudFormation template to fire up different instance sizes based off environment type? i.e. ( If this is for prod, use m1.large instead of t1.micro)
  * Conditions
#### DynamoDB  stores from 1 byte upto 400KB; S3 stores upto 5TB
#### Read Consistency
  * DynamoDB uses eventually consistent reads, unless you specify otherwise. Read operations (such as GetItem, Query, and Scan) provide a ConsistentRead parameter. If you set this parameter to true, DynamoDB uses strongly consistent reads during the operation.
  * All reads of DynamoDB table are not subject to eventual consistency.
  * Read requests are eventually consistent unless otherwise specified. 
#### Conditional Writes
  * By default, the DynamoDB write operations (PutItem, UpdateItem, DeleteItem) are unconditional: each of these operations will overwrite an existing item that has the specified primary key.
  * Conditional writes are helpful in cases where multiple users attempt to modify the same item. 
#### There is no limit on number of attributes an item can have in DynamoDB
#### CloudFormation stack deloys to "Rollback" if it is not going to work at time of creation:
  * A subnet specified in the template does not exist
  * An AMI specified in the template exist in a different region than one in which the stack is deployed
  * The template specifies an instance-store backed AMI and an incompatible EC2 instance type. 
    * NOTE: If we are encountering a JSON syntax error, it will return a template validation error prior to the creation and deployment of resource itself. se
#### Optimistic Locking With Version Number
  * Optimistic locking is a strategy to ensure that the client-side item that you are updating (or deleting) is the same as the item in DynamoDB. If you use this strategy, then your database writes are protected from being overwritten by the writes of others — and vice-versa.
  * Optimistic locking prevents you from accidentally overwriting changes that were made by others; it also prevents others from accidentally overwriting your changes.
  * To support optimistic locking, the AWS SDK for Java provides the @DynamoDBVersionAttribute annotation. In the mapping class for your table, you designate one property to store the version number, and mark it using this annotation
#### DynamoDB API
  * CreateTable
  * DescribeTable
  * ListTables
  * UpdateTable
  * DeleteTable
# CloudFormation 
#### Fn::GetAtt
  * The Fn::GetAtt intrinsic function returns the value of an attribute from a resource in the template.
#### Query an item by it's primary key
  * GetItem
#### AWS::SNS::Subscription
  * The AWS::SNS::Subscription resource subscribes an endpoint to an Amazon Simple Notification Service (Amazon SNS) topic. The owner of the endpoint must confirm the subscription before Amazon SNS creates the subscription.

#### Explanation of following cloudformation template
    * "SNSTopic": {
        "Type":"AWS::SNS::Topic",
        "Properties":{
          "Subscription":[{
            "Protocol":"sqs";
            "Endpoint":{"Fn::GetAtt":["SQSQueue","Arn"]}
            }]
      }
      --Creates an SNS topic and adds a subscription ARN endpoint for SQS resource created under the logical name SQSQueue
 #### Fn::Join
   * { "Fn::Join" : [ "delimiter", [ comma-delimited list of values ] ] 
   * for ex: The following example returns: "a:b:c".
     * "Fn::Join" : [ ":", [ "a", "b", "c" ] ]
#### Ref
  * When you pass the logical ID of an AWS::EC2::Instance object to the intrinsic Ref function, the object's InstanceId is returned. For example: i-1234567890abcdef0.
#### Note: CloudFormation assume default template version if one is not explicitly mentioned in a CloudFormation template.
#### Note: There is no limit to the number of templates. Each AWS CloudFormation account is limited to a maximum of 200 stacks. Contact AWS for higher limit, request will be responded within 2 business days.
#### Note: You can use AWS CloudFormation Template to Create a Topic that Sends Messages to Amazon SQS Queues
#### Note: Resource is only Mandatory field of the 6 available sections on a CloudFormation template ( Template Decription Declaration, Template Format Version Declaration, Parameters, Resources, Mappings, Outputs)
#### Note: CloudFormation can be used with both Chef and Puppet
#### Format Version
  * The AWSTemplateFormatVersion section (optional) identifies the capabilities of the template. The latest template format version is 2010-09-09 and is currently the only valid value.
  * If you don't specify a value, AWS CloudFormation assumes the latest template format version
#### AWS API REFERENCE
  * https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html
#### CloudFormation Helper Scripts Reference
  * AWS CloudFormation provides the following Python helper scripts that you can use to install software and start services on an Amazon EC2 instance that you create as part of your stack. 
#### Parameters
  * Use the optional Parameters section to customize your templates. Parameters enable you to input custom values to your template each time you create or update a stack.
#### Supported AWS-Specific Parameter Types
  * ex: AWS::EC2::KeyPair::KeyName
    * An Amazon EC2 key pair name
#### Referencing a Parameter within a Template
  * You use the Ref intrinsic function to reference a parameter, and AWS CloudFormation uses the parameter's value to provision the stack. You can reference parameters from the Resources and Outputs sections of the same template.
#### Intrinsic Function Reference 
  * AWS CloudFormation provides several built-in functions that help you manage your stacks. Use intrinsic functions in your templates to assign values to properties that are not available until runtime.
  * You can use intrinsic functions only in specific parts of a template. Currently, you can use intrinsic functions in resource properties, outputs, metadata attributes, and update policy attributes. You can also use intrinsic functions to conditionally create stack resources. Ex: Fn:GetAtt, Fn:Join, Ref, etc. 
#### Creating an Amazon S3 Bucket with Defaults
  * This example uses a AWS::S3::Bucket to create a bucket with default settings.
    * "myS3Bucket" : {
      "Type" : "AWS::S3::Bucket"
      }
#### Access Control List (ACL) Overview
  * When you create a bucket or an object, Amazon S3 creates a default ACL that grants the resource owner full control over the resource.
  * Amazon S3 access control lists (ACLs) enable you to manage access to buckets and objects. Each bucket and object has an ACL attached to it as a subresource. It defines which AWS accounts or groups are granted access and the type of access. When a request is received against a resource, Amazon S3 checks the corresponding ACL to verify that the requester has the necessary access permissions.
#### Describing and Listing Your Stacks
  * You can use two AWS CLI commands to get information about your AWS CloudFormation stacks: aws cloudformation list-stacks and aws cloudformation describe-stacks.
#### Default behavior of a CloudFormation stack if creation fails
  * Rollback
#### Delete Your Stacks But Keep Your Data
  * "myS3Bucket" : {
  "Type" : "AWS::S3::Bucket",
  "DeletionPolicy" : "Retain"
}
# S3
#### Policy in S3 bucket
  * When defining policy in s3 bucket as cloudformation template, note policy consists of 4 sections: Resource, action, effect and principal
#### S3 handles error codes with HTTP responses.
#### Note: Multi-part upload API allows you to stop and resume uploads.
#### Introducing randomness to key name
  * Add a hash string as prefix to key name. Ex: examplebucket/232a-2013-26-05-15-00-00/myfolder234234/photo1.jpg
#### Max size of S3 object
  * 5 TB
  * The largest object that can be uploaded in a single PUT is 5GB
  * For objects larger than 100MB, use Multipart upload
#### Default limit in S3 bucket
  * Account(NOT Region) can have a maximum of 100 buckets. BUt you can increase the limit by contacting AWS.
#### if you receive s3 API: 403 forbidden
  * AccessDenied
#### HTTP 404
  * NoSuchBucket
  * NoSuchKey
  * NoSuchLifecycleConfiguration
  * NoSuchUpload
  * NoSuchVersion
#### Note: S3 bucket ownership is not transferrable. The resourse owner can optionally grant access permissions to others by writing an access policy.
#### Note: S3 coupled with Route53 Alias record supports buckets like mydomain.com, example.mydomain.com, and www.mydomain.com.
# EC2
#### If software-based load tester's traffic is hitting only the instance in one AZ among 2 AZ.
  * Force the software-based load tester to re-resolve DNS before every request
  * Use a third party load-testing service to send requests from globally distributed clients
#### DescribeImages
  * find out AMIs that are available for you to use in given region.
#### Which API call would you use to attach an EBS volume to an EC2 instance?
  * AttachVolume
# IAM
#### Identity Providers and Federation
  * If you already manage user identities outside of AWS, you can use IAM identity providers instead of creating IAM users in your AWS account
  * To use an IdP, you create an IAM identity provider entity to establish a trust relationship between your AWS account and the IdP. IAM supports IdPs that are compatible with OpenID Connect (OIDC) or SAML 2.0 (Security Assertion Markup Language 2.0).

  
  

