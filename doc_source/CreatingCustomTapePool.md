--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Creating a Custom Tape Pool<a name="CreatingCustomTapePool"></a>

In this section, you can find information about how to create a new custom tape pool in AWS Storage Gateway\. 

**Topics**
+ [Choosing a Tape Pool Type](#ChoosingTapePoolType)
+ [Using Tape Retention Lock](#TapeRetentionLock)
+ [Creating a Custom Tape Pool](#CreatingCustomTapePools)

## Choosing a Tape Pool Type<a name="ChoosingTapePoolType"></a>

AWS Storage Gateway uses tape pools to determine the storage class that you want tapes to be archived in when they are ejected\. Storage Gateway provides two standard tape pools:
+ **Glacier Pool** – Archives the tape in the S3 Glacier Flexible Retrieval storage class\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives, where you can retrieve the tapes typically within 3\-5 hours\. For more information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\. 
+ **Deep Archive Pool** – Archives the tape in the S3 Glacier Deep Archive storage class\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation, where data is accessed once or twice a year\. You can retrieve tapes archived in S3 Glacier Deep Archive typically within 12 hours\. For detailed information, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

If you archive a tape in S3 Glacier Flexible Retrieval, you can move it to S3 Glacier Deep Archive later\. For more information, see [Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class](managing-gateway-vtl.md#moving-tapes-vtl)\.

Storage Gateway also supports creation of custom tape pools, which allow you to enable tape retention lock to prevent archived tapes from being deleted or moved to another pool for a fixed amount of time, up to 100 years\. This includes locking permission controls on who can delete tapes or modify retention settings\.

## Using Tape Retention Lock<a name="TapeRetentionLock"></a>

With tape retention lock, you can lock archived tapes\. Tape retention lock is an option for tapes in a custom tape pool\. Tapes that have tape retention lock enabled can't be deleted or moved to another pool for a fixed amount of time, up to 100 years\.

You can configure tape retention lock in one of two modes:
+ **Governance mode** – When configured in governance mode, only AWS Identity and Access Management \(IAM\) users with the permissions to perform `storagegateway:BypassGovernanceRetention` can remove tapes from the pool\. If you're using the AWS Storage Gateway API to remove the tape, you must also set `BypassGovernanceRetention` to `true`\.
+ **Compliance mode** – When configured in compliance mode, the protection cannot be removed by any user, including the root AWS account\. 

  When a tape is locked in compliance mode, its retention lock type can't be changed, and its retention period can't be shortened\. The compliance mode lock type helps ensure that a tape can't be overwritten or deleted for the duration of the retention period\.

**Important**  
A custom pool’s configuration cannot be changed after it is created\.

You can enable tape retention lock when you create a custom tape pool\. Any new tapes that are attached to a custom pool inherit the retention lock type, period, and storage class for that pool\.

You can also enable tape retention lock on tapes that were archived before the release of this feature by moving tapes between the default pool and a custom pool that you create\. If the tape is archived, the tape retention lock is effective immediately\.

**Note**  
If you're moving archived tapes between the S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive storage classes, you are charged a fee for moving a tape\. There is no additional charge to move a tape from a default pool to a custom pool if the storage class remains the same\.

## Creating a Custom Tape Pool<a name="CreatingCustomTapePools"></a>

Use the following steps to create a custom tape pool using the AWS Storage Gateway console\. 

**To create a custom tape pool**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the left navigation pane, choose the **Tape Library** tab, and then choose the **Pools** tab\.

1. Choose **Create pool** to open the **Create pool** pane\.

1. For **Name**, enter a unique name to identify your custom tape pool\. The pool name must be between 2 and 100 characters long\.

1. For **Storage class**, choose **Glacier** or **Glacier Deep Archive**\.

1. For **Retention lock type**, choose **None**, **Compliance**, or **Governance**\.
**Note**  
If you choose **Compliance**, tape retention lock cannot be removed by any user, including the root AWS account\.

1. If you choose a tape retention lock type, enter the **Retention period** in days\. The maximum retention period is 36,500 days \(100 years\)\.

1. \(Optional\) For **Tags**, choose **Add new tag** to add a tag to your custom tape pool\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your custom tape pools\. 

   Enter a **Key**, and optionally, a **Value** for your tag\. You can add up to 50 tags to the tape pool\. 

1. Choose **Create pool** to create your new custom tape pool\.