--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

--------

# Managing Your Gateway<a name="managing-gateway-common"></a>

Managing your gateway includes tasks such as configuring cache storage and upload buffer space, working with volumes or virtual tapes, and doing general maintenance\. If you haven't created a gateway, see [Getting Started](GettingStarted.md)\.

Gateway software releases will periodically include OS updates and security patches that have been validated\. These updates are applied as part of the regular gateway update process during a scheduled maintenance window, and are typically released every six months\. Note: Users should treat the Storage Gateway appliance as a managed embedded device, and should not attempt to access or modify the Storage Gateway appliance instance\. Attempting to install or update any software packages using other methods \(ex: SSM or Hypervisor tools\) than the normal gateway update mechanism may result in disruption to the proper functioning of the Gateway\.

**Topics**
+ [Managing your file gateway](managing-gateway-file.md)
+ [Managing Your Volume Gateway](managing-volumes.md)
+ [Managing Your Tape Gateway](managing-gateway-vtl.md)
+ [Moving your data to a new gateway](migrate-data.md)