# Data Encryption Using AWS KMS<a name="encryption"></a>

AWS Storage Gateway uses SSL/TLS \(Secure Socket Layers/Transport Layer Security\) to encrypt data that is transferred between your gateway appliance and AWS storage\. By default, Storage Gateway uses Amazon S3\-Managed Encryption Keys \(SSE\-S3\) to server\-side encrypt all data it stores in Amazon S3\. You have an option to use the Storage Gateway API to configure the different gateway types to encrypt data stored in the cloud using the AWS Key Management Service \(KMS\)\.

**Encrypting a File Share**  
For a file share, you can configure your gateway to encrypt your objects with AWS KMS–managed keys by using SSE\-KMS\. For information on using the Storage Gateway API to encrypt data written to a file share, see [CreateNFSFileShare](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateNFSFileShare.html) in the *AWS Storage Gateway API Reference*\.

**Encrypting a Volume**  
For cached and stored volumes, you can configure your gateway to encrypt volume data stored in the cloud with AWS KMS–managed keys by using the Storage Gateway API\. You can specify one of the managed customer master keys \(CMKs\) as the KMS key\. The CMK that you use to encrypt your volume can't be changed after the volume is created\. For information on using the Storage Gateway API to encrypt data written to a cached or stored volume, see [CreateCachediSCSIVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateCachediSCSIVolume.html) or [CreateStorediSCSIVolume](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateStorediSCSIVolume.html) in the *AWS Storage Gateway API Reference*\.

**Encrypting a Tape**  
For a virtual tape, you can configure your gateway to encrypt tape data stored in the cloud with AWS KMS–managed keys by using the Storage Gateway API\. You can specify one of the managed customer master keys \(CMKs\) as the KMS key\. The CMK that you use to encrypt your tape data can't be changed after the tape is created\. For information on using the Storage Gateway API to encrypt data written to a virtual tape, see [CreateTapes](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_CreateTapes.html) in the *AWS Storage Gateway API Reference*\.

When using AWS KMS to encrypt your data*,* keep the following in mind:
+ Your data is encrypted at rest in the cloud\. That is, the data is encrypted in Amazon S3\.
+ IAM users must have the required permissions to call the AWS KMS API operations\. For more information, see [Using IAM Policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) in the *AWS Key Management Service Developer Guide*\.
+ If you delete or disable your CMK or revoke the grant token, you can't access the data on the volume or tape\. For more information, see [Deleting Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html) in the *AWS Key Management Service Developer Guide*\.
+ If you create a snapshot from a volume that is KMS\-encrypted, the snapshot is encrypted\. The snapshot inherits the volume's KMS key\.
+ If you create a new volume from a snapshot that is KMS\-encrypted, the volume is encrypted\. You can specify a different KMS key for the new volume\. 
**Note**  
AWS Storage Gateway doesn't support creating an unencrypted volume from a recovery point of a KMS\-encrypted volume or a KMS\-encrypted snapshot\.

For more information about AWS KMS, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)