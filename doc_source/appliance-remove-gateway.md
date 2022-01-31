--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Removing a gateway from the hardware appliance<a name="appliance-remove-gateway"></a>

To remove gateway software from your hardware appliance, use the following procedure\. After you do so, the gateway software is uninstalled from your hardware appliance\.

**To remove a gateway from a hardware appliance**

1. From the **Hardware** page of the Storage Gateway console, choose the check box for the hardware appliance\.

1. From the **Actions** menu, choose **Remove Gateway**\.

1. In the **Remove gateway from hardware appliance** dialog box, type *remove* in the input field, then choose **Remove**\.
**Note**  
When you delete a gateway, you can't undo the action\. For certain gateway types, you can lose data on deletion, particularly cached data\. For more information on deleting a gateway, see [Deleting Your Gateway by Using the AWS Storage Gateway Console and Removing Associated Resources](deleting-gateway-common.md)\.

Deleting a gateway doesn't delete the hardware appliance from the console\. The hardware appliance remains for future gateway deployments\.