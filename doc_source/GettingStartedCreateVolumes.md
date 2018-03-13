# Creating a Volume<a name="GettingStartedCreateVolumes"></a>

Previously, you allocated local disks that you added to the VM cache storage and upload buffer\. Now you create a storage volume to which your applications read and write data\. The gateway maintains the volume's recently accessed data locally in cache storage, and asynchronously transferred data to Amazon S3\. For stored volumes, you allocated local disks that you added to the VM upload buffer and your application's data\.

**To create a volume**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the AWS Storage Gateway console, choose **Create volume**\.

1. In the **Create volume** dialog box, choose a gateway for **Gateway**\. 

1. For the cached volumes, type the capacity in **Capacity**\.

   For stored volumes, choose a **Disk ID** value from the list\.

1. For **Volume content**, your choices depend on the type of gateway you are creating the volume for\.

   For cached volumes, you have the following options: 

   + **Create a new empty volume**\.

   + **Create a volume based on an Amazon EBS snapshot**\. If you choose this option, provide a value for **EBS snapshot ID**\. 

   + **Clone from last volume recovery point**\. If you choose this option, choose a volume ID for **Source volume**\. If there are no volumes in the region, this option doesn't appear\.

   For stored volumes, you have the following options: 

   + **Create a new empty volume**\. 

   + **Create a volume based on a snapshot**\. If you choose this option, provide a value for **EBS snapshot ID**\.

   + **Preserve existing data on the disk**

1. Type a name for **iSCSI target name**\.

   The target name can contain lowercase letters, numbers, periods \(\.\), and hyphens \(\-\)\. This target name appears as the **iSCSI target node** name in the **Targets** tab of the **iSCSI Microsoft initiator** UI after discovery\. For example, the name `target1` appears as `iqn.1007-05.com.amazon:target1`\. Make sure that the target name is globally unique within your storage area network \(SAN\)\. 

1. Verify that the **Network interface** setting has IP address selected, or choose an IP address for **Network interface**\. For **Network interface**, one IP address appears for each adapter that is configured for the gateway VM\. If the gateway VM is configured for only one network adapter, no **Network interface** list appears because there is only one IP address\.

   Your iSCSI target will be available on the network adapter you choose\.

   If you have defined your gateway to use multiple network adapters, choose the IP address that your storage applications should use to access your volume\. For information about configuring multiple network adapters, see [Configuring Your Gateway for Multiple NICs](manage-on-premises-common.md#MaintenanceMultiNIC-common)\.
**Note**  
After you choose a network adapter, you can't change this setting\. 

1. Choose **Create volume**\. 

   If you have previously created volumes in this region, you can see them listed on the Storage Gateway console\. 

   The **Configure CHAP Authentication** dialog box appears\. You can configure Challenge\-Handshake Authentication Protocol \(CHAP\) for your volume at this point, or you can choose **Cancel** and configure CHAP later\. For more information on CHAP setup, see [Configure CHAP Authentication for Your Volumes](#GettingStartedConfigureChap-stored), following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/create-volumes.png)

If you don't want to set up CHAP, get started using your volume\. For more information, see [Using Your Volume](GettingStarted-use-volumes.md)\.

## Configure CHAP Authentication for Your Volumes<a name="GettingStartedConfigureChap-stored"></a>

CHAP provides protection against playback attacks by requiring authentication to access your storage volume targets\. In the **Configure CHAP Authentication** dialog box, you provide information to configure CHAP for your volumes\.

**To configure CHAP**

1. Choose the volume for which you want to configure CHAP\.

1. For **Actions**, choose **Configure CHAP authentication**\.

1. For **Initiator Name**, type the name of your initiator\.

1. For **Initiator secret**, type the secret phrase you used to authenticate your iSCSI initiator\.

1. For **Target secret**, type the secret phrase used to authenticate your target for mutual CHAP\.

1. Choose **Save** to save your entries\. 

   For more information about setting up CHAP authentication, see [Configuring CHAP Authentication for Your iSCSI Targets](initiator-connection-common.md#ConfiguringiSCSIClientInitiatorCHAP)\.

**Next Step**

[Using Your Volume](GettingStarted-use-volumes.md) 