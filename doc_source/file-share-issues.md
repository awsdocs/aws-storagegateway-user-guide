# Troubleshooting File Share Issues<a name="file-share-issues"></a>

You can find information following about actions to take if you experience unexpected issues with your file share\.


+ [Your File Share Is Stuck in CREATING Status](#creating-state)
+ [You Can't Create a File Share](#create-file-trobleshoot)
+ [Multiple File Shares Can't Write to the Mapped Amazon S3 Bucket](#multiwrite)
+ [You Can't Upload Files into Your S3 Bucket](#access-s3bucket)

## Your File Share Is Stuck in CREATING Status<a name="creating-state"></a>

When your file share is being created, the status is CREATING\. The status transitions to AVAILABLE status after the file share is created\. If your file share gets stuck in the CREATING status, do the following:

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Make sure the Amazon S3 bucket that you mapped your file share to exists\. If the bucket doesn’t exist, create it\. After you create the bucket, the file share status transitions to AVAILABLE\. For information about how to create an Amazon S3 bucket, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Make sure your bucket name complies with the rules for bucket naming in Amazon S3\. For more information, see [Rules for Bucket Naming](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.

## You Can't Create a File Share<a name="create-file-trobleshoot"></a>

1. If you can't create a file share because your file share is stuck in CREATING status, verify that the Amazon S3 bucket you mapped your file share to exists\. For information on how to do so, see [Your File Share Is Stuck in CREATING Status](#creating-state), preceding\.

1. If the Amazon S3 bucket exists, then verify that AWS Security Token Service is enabled in the region where you are creating the file share\. If a security token is not enabled, you should enable it\. For information about how to enable a token using AWS Security Token Service, see [Activating and Deactivating AWS STS in an AWS Region](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

## Multiple File Shares Can't Write to the Mapped Amazon S3 Bucket<a name="multiwrite"></a>

We don't recommend configuring your Amazon S3 bucket to allow multiple file shares to write to one S3 bucket\. This approach can cause unpredictable results\. 

Instead, we recommend that you allow only one file share to write to each S3 bucket\. You create a bucket policy to allow only the role associated with your file share to write to the bucket\. For more information, see [File Share Best Practices](managing-gateway-file.md#fileshare-best-practices)\.

## You Can't Upload Files into Your S3 Bucket<a name="access-s3bucket"></a>

If you can't upload files into your Amazon S3 bucket, do the following:

1. Make sure you have granted the required access for the file gateway to upload files into your S3 bucket\. For more information, see [Granting Access to an Amazon S3 Bucket](managing-gateway-file.md#grant-access-s3)\.

1. Make sure the role that created the bucket has permission to write to the S3 bucket\. For more information, see [File Share Best Practices](managing-gateway-file.md#fileshare-best-practices)\.