--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Backing Up Your Volumes<a name="backing-up-volumes"></a>

By using Storage Gateway, you can help protect your on\-premises business applications that use Storage Gateway volumes for cloud\-backed storage\. You can back up your on\-premises Storage Gateway volumes using the native snapshot scheduler in Storage Gateway or AWS Backup\. In both cases, Storage Gateway volume backups are stored as Amazon EBS snapshots in Amazon Web Services\. 

**Topics**
+ [Using Storage Gateway to Back Up Your Volumes](#backup-with-sgw)
+ [Using AWS Backup to Back Up Your Volumes](#aws-backup-volumes)

## Using Storage Gateway to Back Up Your Volumes<a name="backup-with-sgw"></a>

You can use the Storage Gateway Management Console to back up your volumes by taking Amazon EBS snapshots and storing the snapshots in Amazon Web Services\. You can either take an ad hoc \(one\-time\) snapshot or set up a snapshot schedule that is managed by Storage Gateway\. You can later restore the snapshot to a new volume by using the Storage Gateway console\. For information about how to back up and manage your backup from the Storage Gateway, see the following topics:
+ [Testing Your Gateway ](GettingStarted-use-volumes.md#GettingStartedTestGatewayMain) 
+ [Creating a One\-Time Snapshot](managing-volumes.md#CreatingSnapshot) 
+ [Cloning a Volume](managing-volumes.md#clone-volume)

## Using AWS Backup to Back Up Your Volumes<a name="aws-backup-volumes"></a>

AWS Backup is a centralized backup service that makes it easy and cost\-effective for you to back up your application data across AWS services in both the Amazon Web Services Cloud and on\-premises\. Doing this helps you meet your business and regulatory backup compliance requirements\. AWS Backup makes protecting your AWS storage volumes, databases, and file systems simple by providing a central place where you can do the following: 
+ Configure and audit the AWS resources that you want to back up\.
+ Automate backup scheduling\.
+ Set retention policies\.
+ Monitor all recent backup and restore activity\.

Because Storage Gateway integrates with AWS Backup, it enables customers to use AWS Backup to back up on\-premises business applications that use Storage Gateway volumes for cloud\-backed storage\. AWS Backup supports backup and restore of both cached and stored volumes\. For information about AWS Backup, see the AWS Backup documentation\. For information about AWS Backup, see [What is AWS Backup?](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) in the *AWS Backup User Guide*\. 

You can manage Storage Gateway volumes' backup and recovery operations with AWS Backup and avoid the need to create custom scripts or manually manage point\-in\-time backups\. With AWS Backup, you can also monitor your on\-premises volume backups alongside your in\-cloud AWS resources from a single AWS Backup dashboard\. You can use AWS Backup to either create a one\-time on\-demand backup or define a backup plan that is managed in AWS Backup\.

Storage Gateway volume backups taken from AWS Backup are stored in Amazon S3 as Amazon EBS snapshots\. You can see the Storage Gateway volume backups from the AWS Backup console or the Amazon EBS console\. 

You can easily restore Storage Gateway volumes that are managed through AWS Backup to any on\-premises gateway or in\-cloud gateway\. You can also restore such a volume to an Amazon EBS volume that you can use with Amazon EC2 instances\.

**Benefits of Using AWS Backup to Back Up Storage Gateway Volumes**

The benefits of using AWS Backup to back up Storage Gateway volumes are that you can meet compliance requirements, avoid operational burden, and centralize backup management\. AWS Backup enables you to do the following:
+ Set customizable scheduled backup policies that meet your backup requirements\.
+ Set backup retention and expiration rules so you no longer need to develop custom scripts or manually manage the point\-in\-time backups of your volumes\. 
+ Manage and monitor backups across multiple gateways, and other AWS resources from a central view\.

**To use AWS Backup to create backups of your volumes**
**Note**  
AWS Backup requires that you choose an AWS Identity and Access Management \(IAM\) role that AWS Backup consumes\. You need to create this role because AWS Backup doesn't create it for you\. You also need to create a trust relationship between AWS Backup and this IAM role\. For information about how to do this, see the *AWS Backup User Guide*\. For information about how to do this, see [Creating a Backup Plan](https://docs.aws.amazon.com/aws-backup/latest/devguide/creating-a-backup-plan.html) in the *AWS Backup User Guide*\.

1. Open the Storage Gateway console and choose **Volumes** from the navigation pane at left\.

1. For **Actions**, choose **Create on\-demand backup with AWS Backup ** or **Create AWS backup plan**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/cryo-backup-menu.png)

   If you want to create an on\-demand backup of the Storage Gateway volume, choose **Create on\-demand backup with AWS Backup**\. You are directed the AWS Backup console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/on-demand.png)

   If you want to create a new AWS Backup plan, choose **Create AWS backup plan**\. You are directed to the AWS Backup console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/backup-plan.png)

   On the AWS Backup console, you can create a backup plan, assign a Storage Gateway volume to the backup plan, and create a backup\. You can also do ongoing backup management tasks\.

### Finding and Restoring Your Volumes from AWS Backup<a name="find-cryo-snapshots"></a>

You can find and restore your backup Storage Gateway volumes from the AWS Backup console\. For more information, see the *AWS Backup User Guide*\. For more information, see [Recovery Points](https://docs.aws.amazon.com/aws-backup/latest/devguide/recovery-points.html) in the *AWS Backup User Guide*\.

**To find and restore your volumes**

1. Open the AWS Backup console and find the Storage Gateway volume backup that you want to restore\. You can restore the Storage Gateway volume backup to an Amazon EBS volume or to a Storage Gateway volume\. Choose the appropriate option for your restore requirements\.

1. For **Restore type**, choose to restore a stored or cached Storage Gateway volume and provide the required information:
   + For a stored volume, provide the information for **Gateway name**, **Disk ID**, and **iSCSI target name**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/restore-stored-volume.png)
   + For a cached volume, provide the information for **Gateway name**, **Capacity**, and **iSCSI target name**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/restore-cached-volume.png)

1.  Choose **Restore resource** to restore your volume\.

**Note**  
You can't use the Amazon EBS console to delete a snapshot that is created by AWS Backup\.