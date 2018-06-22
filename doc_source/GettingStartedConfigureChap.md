# Configuring CHAP Authentication for Your Volumes<a name="GettingStartedConfigureChap"></a>

In AWS Storage Gateway, your iSCSI initiators connect to your volumes as iSCSI targets\. Storage Gateway uses Challenge\-Handshake Authentication Protocol \(CHAP\) to authenticate iSCSI and initiator connections\. CHAP provides protection against playback attacks by requiring authentication to access storage volume targets\. For each volume target, you can define one or more CHAP credentials\. You can view and edit these credentials for the different initiators in the Configure CHAP credentials dialog box\.

**To configure CHAP credentials**

1. In the AWS Storage Gateway Console, choose **Volumes** and select the volume for which you want to configure CHAP credentials\.

1. On the **Actions** menu, choose **Configure CHAP authentication**\.

1. For **Initiator name**, type the name of your initiator\. The name must be at least 1 character and at most 255 characters long\.

1. For **Initiator secret**, provide the secret phrase you want to used to authenticate your iSCSI initiator\. The initiator secret phrase must be at least 12 characters and at most 16 characters long\.

1. For **Target secret**, provide the secret phrase you want used to authenticate your target for mutual CHAP\. The target secret phrase must be at least 12 characters and at most 16 characters long\.

1. Choose **Save** to save your entries\. 

To view or update CHAP credentials, you must have the necessary IAM role permissions to that allows you to perform that operation\.

## Viewing and Editing CHAP Credentials<a name="edit-chap-volume"></a>

You can add, remove or update CHAP credentials for each user\. To view or edit CHAP credentials, you must have the necessary IAM role permissions that allows you to perform that operation and the gateway the initiator target is attached to must be a functioning gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-chap.png)

**To add CHAP credentials**

1. In the AWS Storage Gateway Console, choose **Volumes** and select the volume for which you want to add CHAP credentials\.

1. On the **Actions** menu, choose **Configure CHAP authentication**\.

1. In the Configure CHAPS page, provide the **Initiator name**, **Initiator secret**, and **Target secret** in the respective boxes and choose **Save**\.

**To remove CHAP credentials**

1. In the AWS Storage Gateway Console, choose **Volumes** and select the volume for which you want to remove CHAP credentials\.

1. On the **Actions** menu, choose **Configure CHAP authentication**\.

1. Click the **X** next to the credentials you want to remove and choose **Save**\.

**To update CHAP credentials**

1. In the AWS Storage Gateway Console, choose **Volumes** and select the volume for which you want to update CHAP\.

1. On the **Actions** menu, choose **Configure CHAP authentication**\.

1. In Configure CHAP credentials page, change the entries for the credentials you to update\.

1. Choose **Save**\.