# Configure Amazon CloudWatch logging<a name="configure-loging-file"></a>

To notify you about the health of your file gateway and its resources in Storage Gateway, you can configure an Amazon CloudWatch log group\. For more information, see [Getting file gateway health logs with CloudWatch log groups](monitoring-file-gateway.md#cw-log-groups)\.

------
#### [ New console ]

**To configure a CloudWatch log group for your file gateway**

1. For **Configure logging \- *optional***, choose one of the following:
   + **Disable logging** if you don't want to monitor your gateway using CloudWatch log groups\.
   + **Create a new log group** to create a new CloudWatch log group\.
   + **Use an existing log group** to use a CloudWatch log group that already exists\.

     Choose a log group from the **Existing log group list**\.

1. Choose **Save and continue** to save your configuration settings\.

------
#### [ Original console ]

**To configure a CloudWatch log group for your file gateway**

1. In the **Gateway Log Group** wizard, choose the **Create new Log Group** link to create a new log group\. You are directed to the CloudWatch console to create one\. If you already have a CloudWatch log group that you want to use to monitor your gateway, choose that group for **Gateway Log Group**\.

1. If you create a new log group, choose the refresh button to view the new log group in the list\.

1. If your gateway is deployed on a VMware host that is enabled for VMware High Availability \(HA\) cluster, you're prompted to verify and test the VMware HA configuration\. In this case, choose **Verify VMware HA**\. Otherwise, choose **Save and Continue**\.

------