# Server Side Encryption
  * Server-side encryption encrypts only the object data, not object metadata
    * can be done with:
       AWS SDK for Java, .NET, PHP
       REST API
       AWS Management Console
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
