# Using Storage Classes<a name="storage-classes"></a>

 AWS Storage Gateway supports the Amazon S3 Standard, Amazon S3 Standard\-Infrequent Access, Amazon S3 One Zone\-Infrequent Access and S3 Glacier storage classes\. For more information about storage classes, see [Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Using Infrequent Access Storage Class With File Gateway<a name="ia-file-gateway"></a>

When you create or update a file share, you have the option to select a storage class for your objects\. You can either choose the Amazon S3 Standard or any of the infrequent access storage classes such as S3 Standard IA or S3 One Zone IA\. Objects stored in any of these storage classes can be transitioned to GLACIER using a Lifecycle Policy\.

Although you can write object directly from a file share to the S3\-Standard\-IA or S3\-One Zone\-IA storage class, we recommend you use a Lifecycle Policy to transition your objects rather than write directly from the file share, especially if you're expecting to update or delete the object within 30 days of archiving it\. For information about Lifecycle Policy, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)\. 

## Using GLACIER Storage Class With File Gateway<a name="using-glacier-strage-class"></a>

If you transition a file to S3 Glacier through Amazon S3 Lifecycle Policies and the file is visible to your file share clients through the cache, you get IO errors when you update the file\. We recommend that you set up CloudWatch Events to receive notification when these IO errors occur and use the notification to take action\. For example, you can take action to restore the archived object to S3\. After the object is restored to S3, your file share clients can access and update them successfully through the file share\. For information about how to restore archived objects, see [Restoring Archived Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/restoring-objects.html)\.