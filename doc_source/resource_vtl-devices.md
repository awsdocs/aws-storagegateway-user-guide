# Working with VTL Devices<a name="resource_vtl-devices"></a>

Your tape gateway setup provides the following SCSI devices, which you select when activating your gateway\.

**Topics**
+ [Selecting a Medium Changer After Gateway Activation](#change-mediumchanger-vtl)
+ [Updating the Device Driver for Your Medium Changer](#update-vtl-device-driver)

For medium changers, AWS Storage Gateway works with the following: 
+ AWS\-Gateway\-VTL—This device is provided with the gateway\.
+ STK\-L700—This device emulation is provided with the gateway\.

  When activating your tape gateway, you select your backup application from the list and storage gateway uses the appropriate medium changer\. If your backup application is not listed, you choose **Other** and then choose the medium changer that works with backup application\.

  The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with tape gateways\. This table includes the medium changer type recommended for each backup application\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/resource_vtl-devices.html)
**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](#change-mediumchanger-vtl)\.

For tape drives, AWS Storage Gateway works with the following:
+ IBM\-ULT3580\-TD5—This device emulation is provided with the gateway\.

## Selecting a Medium Changer After Gateway Activation<a name="change-mediumchanger-vtl"></a>

 After your gateway is activated, you can choose to select a different medium changer type\. 

**Important**  
If your tape gateway uses the Symantec Backup Exec 2014 or NetBackup 7\.x backup software, you must select the AWS\-Gateway\-VTL device type\. For more information on how to change the medium changer after gateway activation for these applications, see [Best Practices for using Symantec Backup products \(NetBackup, Backup Exec\) with the Amazon Web Services \(AWS\) Storage Tape Gateway](http://www.symantec.com/docs/TECH227133) in *Symantec Support*\.

**To select a different medium changer type after gateway activation**

1. Stop any related jobs that are running in your backup software\.

1. On the Windows server, open the iSCSI initiator properties window\.

1. Choose the **Targets** tab to display the discovered targets\.

1. On the Discovered targets pane, choose the medium changer you want to change, choose **Disconnect**, and then choose **OK**\. 

1. On the AWS Storage Gateway console, choose **Gateways** from the navigation pane, and then choose the gateway whose medium changer you want to change\.

1. Choose the **VTL Devices** tab, select the medium changer you want to change, and then choose the **Change Media Changer** button,  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Change_MediumChanger.png)

1. In the Change Media Changer Type dialog box that appears, select the media changer you want from the drop\-down list box and then choose **Save**\.

## Updating the Device Driver for Your Medium Changer<a name="update-vtl-device-driver"></a>

Depending on the backup software you use on your Windows server, you might need to update the driver for your medium changer\. 

1. Open Device Manager on your Windows server, and expand the **Medium Changer devices** tree\.

1. Open the context \(right\-click\) menu for **Unknown Medium Changer**, and choose **Update Driver Software** to open the **Update Driver Software\-unknown Medium Changer** window\.

1.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/device-manager.png)

   In the **How do you want to search for driver software?** section, choose **Browse my computer for driver software**\.

1. Choose **Let me pick from a list of device drivers on my computer**\. 
**Note**  
We recommend using the Sony TSL\-A500C Autoloader driver with the Veeam Backup & Replication V7, Veeam Backup & Replication V8, and Microsoft System Center 2012 R2 Data Protection Manager backup software\. This Sony driver has been tested with these types of backup software\.

1. In the **Select the device driver you want to install for this hardware** section, clear the **Show compatible hardware** check box, choose **Sony** in the **Manufacturer** list, choose **Sony \- TSL\-A500C Autoloader** in the **Model** list, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/select-driver.png)

1. In the warning box that appears, choose **Yes**\. If the driver is successfully installed, close the **Update drive software** window\.