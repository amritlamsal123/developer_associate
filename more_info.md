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
  * Name, type and value must not be empty or null and the message body shouldn't be empty or null
#### AMIs can only be shared within a region. To make them available across region, you need to copy them across regions.
#### What is the common pattern scenario's when it comes to the combination of SNS and SQS? 
  * fanout
    * One of the common design pattern is called "fanout". In this pattern, a message published to an SNS topic is distributed  to another number of SQS queue in parallel
#### An IT admin has enabled long polling in their SQS queue. What must be done for long polling to be enabled in SQS?
  * Set the ReceiveMessageWaitTimeSeconds property of the queue to 20 seconds. 
    * Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses (when there are no messages available for a ReceiveMessage request) and false empty responses (when messages are available but aren't included in a response)
#### Difference betn Long and Short polling
  * By default, Amazon SQS uses short polling, querying only a subset of its servers (based on a weighted random distribution), to determine whether any messages are available for a response.
    * Short polling occurs when the WaitTimeSeconds parameter of a ReceiveMessage request is set to 0  
#### What can be used to deploy workers and deciders in Amazon SWF?
  * Amazon EC2 instance
  * Amazon Lambda
  * On-premise machines
#### Use case of SWF are:
  * Media processing
  * Business process automation
  * Data analysis
  * Migration to the cloud
  * Batch processing
#### What in AWS can be used to restrict access to SWF?
  * IAM
    * You can grant IAM users permission to access Amazon SWF. IAM users can only access the SWF domains and APIs that you specify.
#### What is the maximum limit of data that can be retrieved by a scan operation in DynamoDB?
  * 1 MB
    * The result set from a Scan is limited to 1 MB per call. 
#### What can be used in DynamoDB as part of the Query API call to filter results based on the values of primary keys?
  * Expressions
    * You can specify an expression as part of the Query API call to filter results based on values of primary keys on a table using the KeyConditionExpression parameter. 
#### Which function is used in CLoudformation to return the value corresponding to keys in a two-level map that is declared in the Mappings section?
  * Fn::FindInMap
#### Which function is used in Cloudformation to append a set of values into a single value?
  * Fn::Join
#### Retrieve an object from a set of objects
  * Fn::Select
#### {"Fn::Select":["1",["1","2","3","4"]]} returns
  * 2
#### {"Fn::Split":["|","a|b|c"]} returns
  * ["a","b","c"]
#### Which API calls can be used to get information about stacks based on a specific filter.
  * ListStacks
    *includes existing stacks and stacks that have been deleted(90 days period) 
#### Resource is only mandatory field in Cloudformation
#### error "ItemCollectionSizeLimitExceededException"
  * Size of a group of items with the same partition key value has exceeded 10GB. 
#### Note: Any objects uploaded prior to versioning will have the version ID as NULL.
#### 409 Conflict
  * Bucket Not Empty
#### What is used in S3 to enable client web applications that are loaded in one domain to interact with resources in a different domain?
  * CORS Configuration
    * Cross-Origin resource sharing(CORS) defines a way for client web application that are loaded in one domain to interact with resources in a different domain. 
      * Scenario 1: Suppose that you are hosting a website in an Amazon S3 bucket named website as described in Hosting a Static Website on Amazon S3. Your users load the website endpoint http://website.s3-website-us-east-1.amazonaws.com. Now you want to use JavaScript on the webpages that are stored in this bucket to be able to make authenticated GET and PUT requests against the same bucket by using the Amazon S3 API endpoint for the bucket, website.s3.amazonaws.com. A browser would normally block JavaScript from allowing those requests, but with CORS you can configure your bucket to explicitly enable cross-origin requests from website.s3-website-us-east-1.amazonaws.com.
      * Scenario 2: Suppose that you want to host a web font from your S3 bucket. Again, browsers require a CORS check (also called a preflight check) for loading web fonts. You would configure the bucket that is hosting the web font to allow any origin to make these requests.
#### Cloudformation automatically tags Amazon EBS volumes and Amazon EC2 instances with the name of the AWS CloudFormation stack they are part of. CloudFormation natively supports:
  * Network ACL's
  * RouteTables
  * EC2
#### In Cloudformation what is the limit to the number of parameters in the template:
  * You can include up to 60 parameters and 60 outputs in a template.
#### Benefits of SNS
  * Instantaneous, push-based delivery (no polling)
  * Simple APIs and easy integration with applications
  * Flexible message delivery over multiple transport protocols
  * Inexpensive, pay-as-you-go model with no up-front costs
  * Web-based AWS Management Console offers the simplicity of a point-and-click interface
#### error "403 forbidden"
  * InvalidAccesKeyID
#### Which feature of DynamoDB provides a time-ordered sequence of item-level changes made to data?
  * DyanmoDB Streams
#### There is no limit to size of table in DynamoDB. 
#### S3 REST API operation:
  * Put Object
  * Upload Object
  * Get Bucket
    * Write Object is not a S3 API operation
#### HTTP response code "300"
  * AWS REST API call resulted in a redirection
    * This class of status code indicates that further action needs to be taken by the user agent in order to fulfill the request.
#### AWS Flow Framework
  * Helps in developing SWF based applications.
#### SWF Workers can run behind a firewall
  * You do not have to configure your firewall to allow inbound requests. 
#### In SWF, what is the maximum number of open activity tasks
  * 1000
#### Note: SWF workflows can be managed across 3 AZ
  * doen't matter if single AZ fails
#### How can you ensure maximum protection of preserved versions in S3?
  * MFA Delete
    * If you enable Versioning with MFA Delete on your S3 bucket, two forms of authentication are required to permanently delete a version of an object: your AWS account credentials and a valid six-digit code and serial number from authentication device in your physical possession
#### Default setting of connection draining in ELB
  * 300 seconds
    * To ensure that CLB stops sending requests to instances that are de-registered or unhealthy, while keeping the existing connection open, use connection draining. 
#### How many subnets can I create per VPC?
  * 200
#### Note: You cannot use all IP addresses assigned to a subnet. Amazon reserves first four IP addresses and last 1 IP address of every subnet for IP networking purpose.
#### S3 Infrequent Access storage provide the same performance as standard storage
#### 2 ways in which you can get data into S3-Standard IA?
  * Put request by changing the x-amz-storage-class header
  * Use lifecycle policies
#### S3 is not suitable for server-side scripting. EC2 is recommended for website with server-side scripting and database interaction.
#### API call for creating new item in DynamoDB
  * PutItem
  

  
