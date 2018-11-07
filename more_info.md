# Server Side Encryption
  * Server-side encryption encrypts only the object data, not object metadata
    * can be done with:
       AWS SDK for Java, .NET, PHP
       REST API
       AWS Management Console
     * S3 server side encryption uses AES-256
#### Protecting Data Using Server-Side Encryption with Amazon S3-Managed Encryption Keys (SSE-S3)
  * Server-side encryption protects data at rest. Server-side encryption with Amazon S3-managed encryption keys (SSE-S3) uses strong multi-factor encryption. Amazon S3 encrypts each object with a unique key. As an additional safeguard, it encrypts the key itself with a master key that it rotates regularly. Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256), to encrypt your data.
  * You can't enforce SSE-S3 encryption on objects that are uploaded using presigned URLs. You can specify server-side encryption only with the AWS Management Console or an HTTP request header. 
  * If you need server-side encryption for all of the objects that are stored in a bucket, use a bucket policy. For example, the following bucket policy denies permissions to upload an object unless the request includes the x-amz-server-side-encryption header to request server-side encryption:
  
#### Protecting Data Using Server-Side Encryption with AWS KMS–Managed Keys (SSE-KMS)
  * You use AWS KMS via the Encryption Keys section in the IAM console or via AWS KMS APIs to centrally create encryption keys, define the policies that control how keys can be used, and audit key usage to prove they are being used correctly. You can use these keys to protect your data in Amazon S3 buckets.
  * The first time you add an SSE-KMS–encrypted object to a bucket in a region, a default CMK is created for you automatically.
  * Creating your own CMK gives you more flexibility, including the ability to create, rotate, disable, and define access controls, and to audit the encryption keys used to protect your data.
  * If you are uploading or accessing objects encrypted by SSE-KMS, you need to use AWS Signature Version 4 for added security
  * When uploading large objects using the multipart upload API, you can specify SSE-KMS by adding the x-amz-server-side-encryption header to the Initiate Multipart Upload request with the value of aws:kms
  * All GET and PUT requests for an object protected by AWS KMS will fail if they are not made via SSL or by using SigV4.
  * Encryption request headers should not be sent for GET requests and HEAD requests if your object uses SSE-KMS or you’ll get an HTTP 400 BadRequest error.
#### Protecting Data Using Server-Side Encryption with Customer-Provided Encryption Keys (SSE-C)
  * Using server-side encryption with customer-provided encryption keys (SSE-C) allows you to set your own encryption keys.
  * With the encryption key you provide as part of your request, Amazon S3 manages both the encryption, as it writes to disks, and decryption, when you access your objects. Therefore, you don't need to maintain any code to perform data encryption and decryption. The only thing you do is manage the encryption keys you provide.
  * Amazon S3 will reject any requests made over http when using SSE-C
  * If you lose the encryption key any GET request for an object without its encryption key will fail, and you lose the object.
## Using Amazon SNS for System-to-System Messaging with an HTTP/S Endpoint as a Subscriber
  * You can use Amazon SNS to send notification messages to one or more HTTP or HTTPS endpoints. When you subscribe an endpoint to a topic, you can publish a notification to the topic and Amazon SNS sends an HTTP POST request delivering the contents of the notification to the subscribed endpoint.
  * Make sure that
    * Your Endpoint is Ready to Process Amazon SNS Messages
    * Subscribe the HTTP/HTTPS endpoint to the Amazon SNS topic
    * Confirm the subscription
#### What kind of message does SNS send to endpoint?
  * A JSON document with parameters like Message, Signature, Subject, Type
#### 409 Conflict
  * BucketNotEmpty --the bucket you tried to delete is not empty
#### Visiblity Timeout = 0
  * Makes the message immediately available
#### Benefits of multi-part upload
  * Improved throuput
  * Quick recovery from any network issues
  * Pause and resume objects uploads
  * Begin an upload before you know the final object size 
  * You can upload a file as it is being created
  * Note: parts of multi-part upload will not be completed until the "complete" request has been called which puts all the parts of the file together. 
#### Instance Metadata 
  * Instance metadata is data about your instance that you can use to configure or manage the running instance.
  * Anyone who can access the instance can view its metadata. Therefore, you should take suitable precautions to protect sensitive data (such as long-lived encryption keys). You should not store sensitive data, such as passwords, as user data.
  * You can also use instance metadata to access user data that you specified when launching your instance.
  * A request for a general metadata resource (the URI ends with a /) returns a list of available resources, or a 404 - Not Found HTTP error code if there is no such resource
  * If you're throttled while accessing the instance metadata service, retry your query with an exponential backoff strategy.
#### User Data
  * User data is treated as opaque data: what you give is what you get back.
  * User data is limited to 16 KB. 
  * User data must be decoded when you retrieve it. The data is decoded when you retrieve it using instance metadata and the console.
  * If you stop an instance, modify its user data, and start the instance, the updated user data is not executed when you start the instance
#### Specify Instance User Data at Launch 
  * You can specify user data when you launch the instance.
#### Modify Instance User Data 
  * You can modify user data for an instance in the stopped state if the root volume is an EBS volume. 
#### Retrieve Instance User Data
  * To retrieve user data from within a running instance, use the following URI:
  http://169.254.169.254/latest/user-data
#### You have software on EC2 instance that needs to access both the private and public IP addresses of that instance. The best way for the software to get that information is
  * Look it up in instance metadata.
#### Note: "Resource" section in CloudFormation template is mandatory
#### What is the function of a conditional write?
  * A change to a DynamoDB attribute will only be written if that attribute's value has not changed since it was read.
#### AWS provide SDK for
  * Jave, .NET, Node.js, PHP, Python, Ruby, Go, C++, and AWS Mobile SDK
#### What HTTP response code indicates that an AWS API was successful.
  * 200
#### While debugging problem in Amazon DynamoDB:
  * Use the AWS CLI to query the DynamoDB table and data
  * Use the AWS Console to view the DynamoDB table and data
#### Which of the following are actors in Amazon SWF workflow:
  * Decider
  * Activity Worker
  * Workflow starter( is the first task of the workflow)
#### Suitable for 2-tier, higly available web application, durable storage for static content while utilizing lower Overall CPU resources for the web tier
  * S3
#### AWS services offered with no cost.
  * AutoScaling
  * Amazon VPC
#### Note: Primary key = hash, Sort key = range in DynamoDB
#### Recommendation to help ensure requests are evenly distributed across the webservers
  * Re-configure the load-testing software to re-solve DNS for each web request
  * Use a 3rd party load-testing service which offers globally-distributed test clients.
#### DynamoDB limits can be raised by contacting AWS for:
  * The number of tables per account
  * The number of provisioned throughput units per accout.
#### Leveraging SNS Mobile Push for push notification
  * Call the CreatePlatformEndPoint API funtion to register multiple device tokens. 
#### What type of block cipher does Amazon S3 offer for server side encryption?
  * Advanced Encryption Standart (AES-256)
#### Which of the following are correct statements with policy evaluation logic in AWS IAM?
  * An explicit allow overrides default deny AS WELL AS an explicit deny overwrites all allows.
  * By default, all requests are denied.
#### Which statements about DynamoDB are true?
  * DynamoDB uses optimistic concurrency control
    * Optimistic locking is a strategy to ensure that the client-side item that you are updating(or deleting) is the same as the item in DynamoDB. If you use this strategy, then your database writes are protected from being overwritten by writes of others-and vice-versa.
  * DynamoDB uses conditional writes for consistency
    * Conditional Writes for PutItem, DeleteItem, and UpdateItem Operation succeeds only if the item attributes meet one or more expected conditions; otherwise it will return an error.
#### ProvisionedThroughputExceededException
  * You exceeded your maximum allowed provisioned throughput for a table or for one or more global secondary indexes. GSI consists of hashkey-mandatory(partitionkey) and rangekey-optional(sort-key).
#### What happens, by default, when one of the resources in a CloudFormation stack cannot be created?
  * Previously-created resources are deleted and the stack creation terminates.
#### Which of the following are valid arguments for an SNS Public request?
  * Type
  * MessageId
  * TopicArn
  * Subject
  * Message
  * Timestamp
  * SignatureVersion
  * Signature
  * SigningCertURL
  * UnsubscribeURL
#### Which feature can be used to restrict access to data in S3
  * Set an S3 Bucket policy
  * Set an S3 ACL on the bucket or the object
#### Most secure approach for signing requests to the DynamoDB API 
  * Request temporary security credentials  using web identity federation to sign the requests.
#### Secure data at rest on EBS volume
  * Use an Encrypted file system on top of EBS volume.
#### To prevent web fonts from being blocked by the browser
  * Configure the cdfonts bucket to allow cross-origin requests by creating a CORS configuration
#### Strong consistency is more expensive and uses more throuphput than eventually consistent reads.
#### SNS Subscription request are possible by:
  * HTTP/ HTTPS
  * Email / Email-JSON
  * SQS
  * SMS
#### Hourly log files in S3 format
  * HH-DD-MM-YYYY-log_instanceID
#### When using a large Scan Operation in DynamoDB, What technique can be used to minimize the impact of a scan on a table's provisioned throughput?
  * Set a smaller page size for the scan
    * a larger number of smaller scan or query operations would allow your other critical requests to succeed without throttling.
#### Key-value stores
  * S3
  * DynamoDB
  * ElastiCache
#### In DynamoDB, What type of HTTP response codes indicate that a problem was found with the client request sent to the service?
  * HTTP Status Code 400
#### DynamoDB(ConditionExpression)
  * Boolean Functions: ATTRIBUTE_EXIST, CONTAINS, & BEGINS_WITH
  * Comparison Operators: =,<>,<,>,<=,>=,BETWEEN, & IN
  * Logical Operators: NOT, AND, & OR
#### API for DynamoDB table
  * CreateTable
  * UpdateTable
  * DeleteTable
  * DescribeTable
  * ListTables
#### What configuration does AWS provide to handle unsuccessful-processed messages in SQS?
  * Dead letter queues
    * You can use API or the console to configure dead letter queues, which are queues that receive message from other source queues.
#### Note: SQS long-polling RecieveMessage calls are billed exactly the same as short-polling RecieveMessage calls.
#### The protocol versions supported by SQS are:
  * TLS 1.0
  * TLS 1.1
  * TLS 1.2
#### Max length of topic name in SNS
  * 256 characters
#### Owners permission possible in SNS
  * CreateTopic
  * DeleteTopic
  * ListTopics
  * ListSubscriptionsByTopic
  * SetTopicAttributes
  * GetTopicAttributes
  * AddPermission
  * RemovePermission
#### SQS consists of :
  * MessageId
  * Timestamp
  * TopicArn
  * Type
  * UnsubscribeURL
  * Message
  * Subject
  * Signature
  * SignatureVersion
#### SNS offers 10 million subscribers per topic, and 100,000 topics per account. To request a higher limit, contact AWS.
#### AMIs can only be shared within a region. To make them available across region, you need to copy them across regions.
  
  
