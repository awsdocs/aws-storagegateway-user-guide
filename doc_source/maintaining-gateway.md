--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Maintaining Your Gateway<a name="maintaining-gateway"></a>

Maintaining your gateway includes tasks such as configuring cache storage and upload buffer space, and doing general maintenance your gateway's performance\. These tasks are common to all gateway types\. If you haven't created a gateway, see [Creating Your Gateway](creating-your-gateway.md)\.

**Topics**
+ [Shutting Down Your Gateway VM](MaintenanceShutDown-common.md)
+ [Managing local disks for your Storage Gateway](ManagingLocalStorage-common.md)
+ [Managing Bandwidth for Your Tape or Volume Gateway](MaintenanceUpdateBandwidth-common.md)
+ [Managing Gateway Updates Using the AWS Storage Gateway Console](MaintenanceManagingUpdate-common.md)
+ [Performing Maintenance Tasks on the Local Console](manage-on-premises.md)
+ [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)

**Note**  
 FGW will delete those parts except in situation where it can't \(like a network issue\)\. In that case, using a lifecycle policy might be the fail over solution\.