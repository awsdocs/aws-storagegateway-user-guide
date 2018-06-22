# Encrypting Your Data Using AWS Key Management System<a name="encryption"></a>

AWS Storage Gateway uses AWS Key Management Service \(AWS KMS\) to support encryption\. Storage Gateway is integrated with AWS KMS so you can use the Customer Master Keys \(CMKs\) in your account to protect the data that Storage Gateway receives, stores, or manages\. Currently, you can do this by using the AWS Storage Gateway API\. 
+ To use the Storage Gateway API to encrypt data written to a file share, see [CreateNFSFileShare](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.
+ To use the Storage Gateway API to encrypt data written to a cached volume, see [CreateCachediSCSIVolume](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateCachediSCSIVolume.html) in the *AWS Storage Gateway API Reference*\.
+ To use the Storage Gateway API to encrypt data written to a virtual tape, see [CreateTapes](http://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html) in the *AWS Storage Gateway API Reference*\.

When using AWS KMS to encrypt your data keep the following in mind:
+ AWS Storage Gateway currently doesn't support creating a snapshot of an encrypted cached volume\.
+ AWS Storage Gateway currently doesn’t support creating an unencrypted volume from a recovery point of a KMS\-encrypted volume\.
+ AWS Storage Gateway currently doesn’t support creating a volume from a KMS\-encrypted EBS snapshot\.
+ Your data is encrypted at rest\. That is, the data is encrypted in Amazon S3\.
+ IAM users must have the required permissions to call the AWS KMS APIs\. For more information, see [Using IAM Policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) in the *AWS Key Management Service Developer Guide*
+ If you delete or disable your Customer Master Key \(CMK\) or revoke the grant token, you wouldn't be able to access the data on the volume or tape\. For more information, see [Deleting Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html) in the *AWS Key Management Service Developer Guide*\.

For more information about AWS KMS see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.