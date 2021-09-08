# Verify VMware High Availability \(VMware HA clusters only\)<a name="verify-ha"></a>

If your gateway is not deployed on a VMware host that is enabled for VMware High Availability \(HA\), you can skip this section\.

If your gateway is deployed on a VMware host that is enabled for VMware High Availability \(HA\) cluster, you can either test the configuration when activating the gateway or after your gateway is activated\. The following instructions show you how to test the configuration during activation\.

------
#### [ New console ]

**To test for VMware HA**

1. For **Verify VMware High Availability configuration**, choose **Next**\. Verification can take up to two minutes to complete\.

   If the test is successful, a message that indicates a successful test is displayed in the banner\. If the test fails, a failed message is displayed\. You can make changes in your vSphere configuration and repeat the test\.

1. To repeat the test, on the **Gateways** dashboard, choose your gateway, and then for **Actions**, choose **Verify VMware High Availability**\.

------
#### [ Original console ]

**To test for VMware HA**

1. On the **Verify VMware High Availability Configuration** page, choose **Verify VMware HA**\. This can take up to two minutes to complete\.

1. If the test is successful, a message that indicates a successful test is displayed in the banner\. If the test fails, a failed message is displayed\. You can make changes in your vSphere configuration and repeat the test\.

1. To repeat the test, for **Actions** choose **Verify VMware HA**\.

------

For information about how to configure your gateway for VMware HA, see [Using VMware vSphere High Availability with Storage Gateway](Performance.md#vmware-ha)\.

**Next step**

[Create a file share](GettingStartedCreateFileShare.md)