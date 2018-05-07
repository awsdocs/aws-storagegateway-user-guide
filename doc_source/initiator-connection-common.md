# Connecting iSCSI Initiators<a name="initiator-connection-common"></a>

When managing your gateway, you work with file shares, volumes or virtual tape library \(VTL\) devices that are exposed as Internet Small Computer System Interface \(iSCSI\) targets\. For file gateways the iSCSI targets are file share, For volume gateways, the iSCSI targets are volumes\. For tape gateways, the targets are VTL devices\. As part of this work, you do such tasks as connecting to those targets, customizing iSCSI settings, connecting from a Red Hat Linux client, and configuring Challenge\-Handshake Authentication Protocol \(CHAP\)\. 

**Topics**
+ [Connecting to Your Volumes to a Windows Client](#ConfiguringiSCSIClient)
+ [Connecting Your VTL Devices to a Windows client](#ConfiguringiSCSIClient-vtl)
+ [Connecting Your Volumes or VTL Devices to a Linux Client](#ConfiguringiSCSIClientInitiatorRedHatClient)
+ [Customizing iSCSI Settings](#recommendediSCSISettings)
+ [Configuring CHAP Authentication for Your iSCSI Targets](#ConfiguringiSCSIClientInitiatorCHAP)

The iSCSI standard is an Internet Protocol \(IP\)–based storage networking standard for initiating and managing connections between IP\-based storage devices and clients\. The following list defines some of the terms that are used to describe the iSCSI connection and the components involved\. 

**iSCSI initiator **  
The client component of an iSCSI network\. The initiator sends requests to the iSCSI target\. Initiators can be implemented in software or hardware\. AWS Storage Gateway only supports software initiators\.

**iSCSI target**  
The server component of the iSCSI network that receives and responds to requests from initiators\. Each of your volumes is exposed as an iSCSI target\. Connect only one iSCSI initiator to each iSCSI target\.

**Microsoft iSCSI initiator**  
The software program on Microsoft Windows computers that enables you to connect a client computer \(that is, the computer running the application whose data you want to write to the gateway\) to an external iSCSI\-based array \(that is, the gateway\)\. The connection is made using the host computer's Ethernet network adapter card\. The Microsoft iSCSI initiator is already installed on Windows Server 2008 R2, Windows 7, Windows Server 2008, and Windows Vista\. On these operating systems, you don't need to install the initiator\. 

**Red Hat iSCSI initiator**  
The iscsi\-initiator\-utils Resource Package Manager \(RPM\) package provides you with an iSCSI initiator implemented in software for Red Hat Linux\. The package includes a server daemon for the iSCSI protocol\.

Each type of gateway can connect to iSCSI devices, and you can customize those connections, as described following\.

## Connecting to Your Volumes to a Windows Client<a name="ConfiguringiSCSIClient"></a>

A volume gateway exposes volumes you have created for the gateway as iSCSI targets\. For more information, see [Connecting Your Volumes to Your Client](GettingStarted-use-volumes.md#GettingStartedAccessVolumes)\.

**Note**  
To connect to your volume target, your gateway must have an upload buffer configured\. If an upload buffer is not configured for your gateway, then the status of your volumes is displayed as UPLOAD BUFFER NOT CONFIGURED\. To configure an upload buffer for a gateway in a stored volumes setup, see [To configure upload buffer or cache storage ](ManagingLocalStorage-common.md#GatewayWorkingStorageCachedTaskBuffer)\. To configure an upload buffer for a gateway in a cached volumes setup, see [To configure upload buffer or cache storage ](ManagingLocalStorage-common.md#GatewayWorkingStorageCachedTaskBuffer)\. 

The following diagram highlights the iSCSI target in the larger picture of the AWS Storage Gateway architecture\. For more information, see [How AWS Storage Gateway Works \(Architecture\)](StorageGatewayConcepts.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ArchitectureDiagram_HighlightediSCSI-diagram.png)

You can connect to your volume from either a Windows or Red Hat Linux client\. You can optionally configure CHAP for either client type\.

Your gateway exposes your volume as an iSCSI target with a name you specify, prepended by `iqn.1997-05.com.amazon:`\. For example, if you specify a target name of `myvolume`, then the iSCSI target you use to connect to the volume is `iqn.1997-05.com.amazon:myvolume`\. For more information about how to configure your applications to mount volumes over iSCSI, see [Connecting to Your Volumes to a Windows Client](#ConfiguringiSCSIClient)\. 


| To | See | 
| --- | --- | 
|  Connect to your volume from Windows\.  |  [Connecting Your Volumes to Your Client](GettingStarted-use-volumes.md#GettingStartedAccessVolumes) in the Getting Started section  | 
|  Connect to your volume from Red Hat Linux\.  |  [Connecting to a Microsoft Windows Client](GettingStarted-create-tape-gateway.md#iscsi-vtl-windows)  | 
|  Configure CHAP authentication for Windows and Red Hat Linux\.  |  [Configuring CHAP Authentication for Your iSCSI Targets](#ConfiguringiSCSIClientInitiatorCHAP)  | 

**To connect your Windows client to a storage volume**

1. On the **Start** menu of your Windows client computer, type **iscsicpl\.exe** in the **Search Programs and files** box, locate the iSCSI initiator program, and then run it\.
**Note**  
You must have administrator rights on the client computer to run the iSCSI initiator\.

1. If prompted, choose **Yes** to start the Microsoft iSCSI initiator service\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_09.png)

1. In the **iSCSI Initiator Properties** dialog box, choose the **Discovery** tab, and then choose the **Discovery Portal** button\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_10.png)

1. In the **Discover Target Portal** dialog box, type the IP address of your iSCSI target for **IP address or DNS name**, and then choose **OK**\. To get the IP address of your gateway, check the **Gateway** tab on the AWS Storage Gateway console\. If you deployed your gateway on an Amazon EC2 instance, you can find the public IP or DNS address in the **Description** tab on the Amazon EC2 console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_20.png)

   The IP address now appears in the **Target portals** list on the **Discovery** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_30.png)

1. Connect the new target portal to the storage volume target on the gateway:

   1. Choose the **Targets** tab\.

      The new target portal is shown with an inactive status\. Note that the target name shown should be the same as the name you specified for your storage volume in step 1\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_33.png)

   1. Select the target, and then choose **Connect**\. 

      If the target name is not populated already, type the name of the target as shown in step 1 in the **Connect to Target** dialog box, select the check box next to **Add this connection to the list of Favorite Targets**, and then choose **OK**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_34.png)

   1. In the **Targets** tab, ensure that the target **Status** has the value **Connected** indicating the target is connected, and then choose **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_35-small.png)

You can now initialize and format this storage volume for Windows so you can begin saving data on it\. You do this by using the Windows Disk Management tool\. 

**Note**  
Although it is not required for this exercise, we highly recommend that you customize your iSCSI settings for a real\-world application as discussed in [Customizing Your Windows iSCSI Settings](#CustomizeWindowsiSCSISettings)\.

## Connecting Your VTL Devices to a Windows client<a name="ConfiguringiSCSIClient-vtl"></a>

A tape gateway exposes several tape drives and a media changer, referred to collectively as VTL devices, as iSCSI targets\. For more information, see [Requirements](Requirements.md)\. 

**Note**  
You connect only one application to each iSCSI target\. 

The following diagram highlights the iSCSI target in the larger picture of the AWS Storage Gateway architecture\. For more information on AWS Storage Gateway architecture, see [Tape Gateways](StorageGatewayConcepts.md#storage-gateway-vtl-concepts)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/Gateway-VTL-iSCSI-vtl-diagram.png)

**To connect your Windows client to the VTL devices**

1. On the **Start** menu of your Windows client computer, type **iscsicpl\.exe** in the **Search Programs and files** box, locate the iSCSI initiator program, and then run it\.
**Note**  
You must have administrator rights on the client computer to run the iSCSI initiator\.

1. If prompted, choose **Yes** to start the Microsoft iSCSI initiator service\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_09.png)

1. In the **iSCSI Initiator Properties** dialog box, choose the **Discovery** tab, and then choose the **Discover Portal** button\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_10.png)

1. In the **Discover Target Portal** dialog box, type the IP address of your tape gateway for **IP address or DNS name**, and then choose **OK**\. To get the IP address of your gateway, check the **Gateway** tab on the AWS Storage Gateway console\. If you deployed your gateway on an Amazon EC2 instance, you can find the public IP or DNS address in the **Description** tab on the Amazon EC2 console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GSClientConfigure_20.png)

1. Choose the **Targets** tab, and then choose **Refresh**\. All ten tape drives and the medium changer appear in the **Discovered targets** box\. The status for the targets is **Inactive**\.

   The following screenshot shows the discovered targets\.

       
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/iscsciInitiatorDiscoveryVTL.png)

1. Select the first device and choose **Connect**\. You connect the devices one at a time\. 

1. In the **Connect to Target** dialog box, choose **OK**\.

1. Repeat steps 6 and 7 for each of the devices to connect all of them, and then choose **OK** in the **iSCSI Initiator Properties** dialog box\.

1. On a Windows client, the driver provider for the tape drive must be Microsoft\. Use the following procedure to verify the driver provider, and update the driver and provider if necessary\. 

   1. On your Windows client, start Device Manager\.

   1. Expand **Tape drives**, choose the context \(right\-click\) menu for a tape drive, and choose **Properties**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/TapeDriveDriverProvider10.png)

   1. In the **Driver** tab of the **Device Properties** dialog box, verify **Driver Provider** is Microsoft\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/TapeDriveDriverProvider20.png)

   1. If **Driver Provider** is not Microsoft, set the value as follows:

      1. Choose **Update Driver**\.

      1. In the **Update Driver Software** dialog box, choose **Browse my computer for driver software**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/TapeDriveDriverProvider30.png)

      1. In the **Update Driver Software** dialog box, choose **Let me pick from a list of device drivers on my computer**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/TapeDriveDriverProvider40.png)

      1. Select **LTO Tape drive** and choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/TapeDriveDriverProvider50.png)

   1. Choose **Close** to close the **Update Driver Software** window, and verify that the **Driver Provider** value is now set to Microsoft\.

   1.  Repeat steps 9\.2 through 9\.5 to update all the tape drives\.

## Connecting Your Volumes or VTL Devices to a Linux Client<a name="ConfiguringiSCSIClientInitiatorRedHatClient"></a>

**Topics**

When using Red Hat Enterprise Linux \(RHEL\), you use the iscsi\-initiator\-utils RPM package to connect to your gateway iSCSI targets \(volumes or VTL devices\)\. 

**To connect a Linux client to the iSCSI targets**

1. Install the iscsi\-initiator\-utils RPM package, if it isn't already installed on your client\. 

   You can use the following command to install the package\.

   ```
   sudo yum install iscsi-initiator-utils
   ```

1. Ensure that the iSCSI daemon is running\.

   1. Verify that the iSCSI daemon is running using one of the following commands\.

      For RHEL 5 or 6, use the following command\.

      ```
      sudo /etc/init.d/iscsi status
      ```

      For RHEL 7, use the following command\.

      ```
      sudo service iscsid status
      ```

   1. If the status command doesn't return a status of *running*, then start the daemon using one of the following commands\.

      For RHEL 5 or 6, use the following command\.

      ```
      sudo /etc/init.d/iscsi start
      ```

      For RHEL 7, use the following command\. For RHEL 7, you usually don't need to explicitly start the iscsid service\.

      ```
      sudo /etc/init.d/iscsi start
      ```

1. To discover the volume or VTL device targets defined for a gateway, use the following discovery command\.

   ```
   sudo /sbin/iscsiadm --mode discovery --type sendtargets --portal [GATEWAY_IP]:3260
   ```

   Substitute your gateway's IP address for the *\[GATEWAY\_IP\]* variable in the preceding command\. You can find the gateway IP in the **iSCSI Target Info** properties of a volume on the AWS Storage Gateway console\. 

   The output of the discovery command will look like the following example output\.

   For volume gateways: `[GATEWAY_IP]:3260, 1 iqn.1997-05.com.amazon:myvolume `

   For tape gateways: `iqn.1997-05.com.amazon:[GATEWAY_IP]-tapedrive-01`

   Your iSCSI qualified name \(IQN\) will be different than what is shown preceding, because IQN values are unique to an organization\. The name of the target is the name that you specified when you created the volume\. You can also find this target name in the **iSCSI Target Info** properties pane when you select a volume on the AWS Storage Gateway console\.

1. To connect to a target, use the following command\.

   Note that you need to specify the correct *\[GATEWAY\_IP\]* and IQN in the connect command\.
**Warning**  
For gateways that are deployed on an Amazon EC2 instance, accessing the gateway over a public Internet connection is not supported\. The elastic IP address of the Amazon EC2 instance cannot be used as the target address\. 

   ```
   sudo /sbin/iscsiadm --mode node --targetname iqn.1997-05.com.amazon:[ISCSI_TARGET_NAME] --portal [GATEWAY_IP]:3260,1 --login
   ```

1. To verify that the volume is attached to the client machine \(the initiator\), use the following command\.

   ```
   ls -l /dev/disk/by-path
   ```

   The output of the command will look like the following example output\.

   `lrwxrwxrwx. 1 root root 9 Apr 16 19:31 ip-[GATEWAY_IP]:3260-iscsi-iqn.1997-05.com.amazon:myvolume-lun-0 -> ../../sda`

   We highly recommend that after you set up your initiator you customize your iSCSI settings as discussed in [Customizing Your Linux iSCSI Settings](#CustomizeLinuxiSCSISettings)\.

## Customizing iSCSI Settings<a name="recommendediSCSISettings"></a>

After setting up your initiator, we highly recommend that you customize your iSCSI settings to prevent the initiator from disconnecting from targets\.

By increasing the iSCSI timeout values as shown in the following steps, you make your application better at dealing with write operations that take a long time and other transient issues such as network interruptions\.

**Note**  
Before making changes to the registry, you should make a backup copy of the registry\. For information on making a backup copy and other best practices to follow when working with the registry, see [Registry best practices](http://technet.microsoft.com/en-us/library/cc780921(WS.10).aspx) in the *Microsoft TechNet Library*\.

**Topics**
+ [Customizing Your Windows iSCSI Settings](#CustomizeWindowsiSCSISettings)
+ [Customizing Your Linux iSCSI Settings](#CustomizeLinuxiSCSISettings)

### Customizing Your Windows iSCSI Settings<a name="CustomizeWindowsiSCSISettings"></a>

When using a Windows client, you use the Microsoft iSCSI initiator to connect to your gateway volume\. For instructions on how to connect to your volumes, see [Connecting Your Volumes to Your Client](GettingStarted-use-volumes.md#GettingStartedAccessVolumes)\.

For a tape gateway setup, connecting to your VTL devices by using a Microsoft iSCSI initiator is a two\-step process: 

1. Connect your tape gateway devices to your Windows client\.

1. If you are using a backup application, configure the application to use the devices\.

The Getting Started example setup provides instructions for both these steps\. It uses the Symantec NetBackup backup application\. For more information, see [Connecting Your VTL Devices](GettingStarted-create-tape-gateway.md#GettingStartedAccessTapesVTL) and [Configuring NetBackup Storage Devices](backup_netbackup-vtl.md#configure-netback-storage-devices)\. 

**To customize your Windows iSCSI settings**

1. Increase the maximum time for which requests are queued\.

   1. Start Registry Editor \(`Regedit.exe`\)\.

   1. Navigate to the globally unique identifier \(GUID\) key for the device class that contains iSCSI controller settings, shown following\.
**Warning**  
Make sure you are working in the **CurrentControlSet** subkey and not another control set such as **ControlSet001** or **ControlSet002**\.

      ```
      HKEY_Local_Machine\SYSTEM\CurrentControlSet\Control\Class\{4D36E97B-E325-11CE-BFC1-08002BE10318}
      ```

   1. Find the subkey for the Microsoft iSCSI initiator, shown following as *\[<Instance Number\]*\.

      The key is represented by a four\-digit number, such as `0000`\. 

      ```
      HKEY_Local_Machine\SYSTEM\CurrentControlSet\Control\Class\{4D36E97B-E325-11CE-BFC1-08002BE10318}\[<Instance Number]
      ```

      Depending on what is installed on your computer, the Microsoft iSCSI initiator might not be the subkey `0000`\. You can ensure that you have selected the correct subkey by verifying that the string `DriverDesc` has the value `Microsoft iSCSI Initiator`, as shown in the following example\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/iscsi-windows-registry-10.png)

   1. To show the iSCSI settings, choose the **Parameters** subkey\.

   1. Open the context \(right\-click\) menu for the **MaxRequestHoldTime** DWORD \(32\-bit\) value, choose **Modify**, and then change the value to 600\.

      This value represents a hold time of 600 seconds\. The example following shows the **MaxRequestHoldTime** DWORD value with a value of 600\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/iscsi-windows-registry-20.png)

1. Increase the disk timeout value, as shown following:

   1. Start Registry Editor \(`Regedit.exe`\), if you haven't already\.

   1. Navigate to the **Disk** subkey in the **Services** subkey of the **CurrentControlSet**, shown following\.

      ```
      HKEY_Local_Machine\SYSTEM\CurrentControlSet\Services\Disk
      ```

   1. Open the context \(right\-click\) menu for the **TimeOutValue** DWORD \(32\-bit\) value, choose **Modify**, and then change the value to 600\.

      This value represents a timeout of 600 seconds\. The example following shows the **TimeOutValue** DWORD value with a value of 600\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/iscsi-windows-registry-30.png)

1. To ensure that the new configuration values take effect, restart your system\.

   Before restarting, you must make sure that the results of all write operations to volumes are flushed\. To do this, take any mapped storage volume disks offline before restarting\.

### Customizing Your Linux iSCSI Settings<a name="CustomizeLinuxiSCSISettings"></a>

After setting up your initiator, we highly recommend that you customize your iSCSI settings to prevent the initiator from disconnecting from targets\. By increasing the iSCSI timeout values as shown following, you make your application better at dealing with write operations that take a long time and other transient issues such as network interruptions\.

**Note**  
Commands might be slightly different for other types of Linux\. The following examples are based on Red Hat Linux\.

**To customize your Linux iSCSI settings**

1. Increase the maximum time for which requests are queued\.

   1. Open the `/etc/iscsi/iscsid.conf` file and find the following lines\.

      ```
      node.session.timeo.replacement_timeout = [replacement_timeout_value] 
      node.conn[0].timeo.noop_out_interval = [noop_out_interval_value] 
      node.conn[0].timeo.noop_out_timeout = [noop_out_timeout_value]
      ```

   1. Set the *\[replacement\_timeout\_value\]* value to 600\. 

      Set the *\[noop\_out\_interval\_value\]* value to 60\.

      Set the *\[noop\_out\_timeout\_value\]* value to 600\. 

      All three values are in seconds\.
**Note**  
The `iscsid.conf` settings must be made before discovering the gateway\. If you have already discovered your gateway or logged in to the target, or both, you can delete the entry from the discovery database using the following command\. Then you can rediscover or log in again to pick up the new configuration\.  

      ```
      iscsiadm -m discoverydb -t sendtargets -p [GATEWAY_IP]:3260 -o delete
      ```

1. Increase the disk timeout value in the rules file\.

   1. If you are using the RHEL 5 initiator, open the `/etc/udev/rules.d/50-udev.rules` file and find the following line\.

      ```
      ACTION=="add", SUBSYSTEM=="scsi" , SYSFS{type}=="0|7|14", \         
      RUN+="/bin/sh -c 'echo [timeout] > /sys$$DEVPATH/timeout'"
      ```
**Note**  
This rules file does not exist in RHEL 6 or 7 initiators, so you must create it\.

      To modify the timeout value in RHEL 6, use the following command and then add the lines of code shown preceding\. 

      ```
      sudo vim /etc/udev/rules.d/50-udev.rules
      ```

      To modify the timeout value in RHEL 7, use the following command and then add the lines of code shown preceding\. 

      ```
      sudo su -c "echo 600 > /sys/block/[device name]/device/timeout"
      ```

   1. Set the *\[timeout\]* value to 600\.

      This value represents a timeout of 600 seconds\.

1. Restart your system to ensure that the new configuration values take effect\.

   Before restarting, you must make sure that the results of all write operations to your volumes are flushed\. To do this, unmount storage volumes before restarting\.

1. You can test the configuration by using the following command\. 

   ```
   udevadm test [PATH_TO_ISCSI_DEVICE]
   ```

   This command shows the udev rules that are applied to the iSCSI device\.

## Configuring CHAP Authentication for Your iSCSI Targets<a name="ConfiguringiSCSIClientInitiatorCHAP"></a>

AWS Storage Gateway supports authentication between your gateway and iSCSI initiators by using Challenge\-Handshake Authentication Protocol \(CHAP\)\. CHAP provides protection against playback attacks by periodically verifying the identity of an iSCSI initiator as authenticated to access a volume and VTL device target\. 

To set up CHAP, you must configure it both on the AWS Storage Gateway console and in the iSCSI initiator software that you use to connect to the target\. Storage Gateway uses mutual CHAP, which is when the initiator authenticates the target and the target authenticates the initiator\.

**To set up mutual CHAP for your targets**

1. Configure CHAP on the AWS Storage Gateway console, as discussed in [To configure CHAP for a volume target on the AWS Storage Gateway console](#ConfiguringiSCSIClientInitiatorCHAPConsole)\.

1. In your client initiator software, complete the CHAP configuration:
   + To configure mutual CHAP on a Windows client, see [To configure mutual CHAP on a Windows client](#ConfiguringiSCSIClientInitiatorCHAPWindows)\.
   + To configure mutual CHAP on a Red Hat Linux client, see [To configure mutual CHAP on a Red Hat Linux client](#ConfiguringiSCSIClientInitiatorCHAPLinux)\.<a name="ConfiguringiSCSIClientInitiatorCHAPConsole"></a>

**To configure CHAP for a volume target on the AWS Storage Gateway console**

In this procedure, you specify two secret keys that are used to read and write to a volume\. These same keys are used in the procedure to configure the client initiator\.

1. On the AWS Storage Gateway console, choose **Volumes** in the navigation pane\.

1. On the **Actions** menu, choose **Configure CHAP Authentication**\.

1. Provide the requested information in the **Configure CHAP Authentication** dialog box, shown in the screenshot following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-chap.png)

   1. For **Initiator Name**, type the name of your iSCSI initiator\.

      You can find the initiator name by using your iSCSI initiator software\. For example, for Windows clients, the name is the value on the **Configuration** tab of the iSCSI initiator\. For more information, see [To configure mutual CHAP on a Windows client](#ConfiguringiSCSIClientInitiatorCHAPWindows)\.
**Note**  
To change an initiator name, you must first disable CHAP, change the initiator name in your iSCSI initiator software, and then enable CHAP with the new name\.

   1. For **Secret used to Authenticate Initiator**, type the secret requested\.

      This secret must be a minimum of 12 characters and a maximum of 16 characters long\. This value is the secret key that the initiator \(that is, the Windows client\) must know to participate in CHAP with the target\.

   1. For **Secret used to Authenticate Target \(Mutual CHAP\)**, type the secret requested\.

      This secret must be a minimum of 12 characters and a maximum of 16 characters long\. This value is the secret key that the target must know to participate in CHAP with the initiator\.
**Note**  
The secret used to authenticate the target must be different than the secret to authenticate the initiator\.

   1. Choose **Save**\.

1. Choose the **Details** tab and confirm that **iSCSI CHAP authentication** is set to **true**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/chaps-volume-true.png)

**To configure CHAP for a VTL device target on the AWS Storage Gateway console**

In this procedure, you specify two secret keys that are used to read and write to a virtual tape\. These same keys are used in the procedure to configure the client initiator\.

1. In the navigation pane, choose **Gateways**\.

1. Choose your gateway, and then choose the **VTL Devices** tab to display all your VTL devices\.

1. Choose the device you want to configure CHAP for\.

1. Provide the requested information in the **Configure CHAP Authentication** dialog box, shown in the screenshot following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/configure-chap.png)

   1. For **Initiator Name**, type the name of your iSCSI initiator\.

      You can find the initiator name by using your iSCSI initiator software\. For example, for Windows clients, the name is the value on the **Configuration** tab of the iSCSI initiator\. For more information, see [To configure mutual CHAP on a Windows client](#ConfiguringiSCSIClientInitiatorCHAPWindows)\.
**Note**  
To change an initiator name, you must first disable CHAP, change the initiator name in your iSCSI initiator software, and then enable CHAP with the new name\.

   1. For **Secret used to Authenticate Initiator**, type the secret requested\.

      This secret must be a minimum of 12 characters and a maximum of 16 characters long\. This value is the secret key that the initiator \(that is, the Windows client\) must know to participate in CHAP with the target\.

   1. For **Secret used to Authenticate Target \(Mutual CHAP\)**, type the secret requested\.

      This secret must be a minimum of 12 characters and a maximum of 16 characters long\. This value is the secret key that the target must know to participate in CHAP with the initiator\.
**Note**  
The secret used to authenticate the target must be different than the secret to authenticate the initiator\.

   1. Choose **Save**\.

1. On the **VTL Devices** tab, confirm that the iSCSI CHAP authentication field is set to **true**\.<a name="ConfiguringiSCSIClientInitiatorCHAPWindows"></a>

**To configure mutual CHAP on a Windows client**

In this procedure, you configure CHAP in the Microsoft iSCSI initiator using the same keys that you used to configure CHAP for the volume on the console\.

1. If the iSCSI initiator is not already started, on the **Start** menu of your Windows client computer, choose **Run**, type **iscsicpl\.exe**, and then choose **OK** to run the program\.

1. Configure mutual CHAP configuration for the initiator \(that is, the Windows client\):

   1. Choose the **Configuration** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_20.png)
**Note**  
The **Initiator Name** value is unique to your initiator and company\. The name shown preceding is the value that you used in the **Configure CHAP Authentication** dialog box of the AWS Storage Gateway console\.  
The name shown in the example image is for demonstration purposes only\.

   1. Choose **CHAP**\.

   1. In the **iSCSI Initiator Mutual Chap Secret** dialog box, type the mutual CHAP secret value\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_25.png)

      In this dialog box, you enter the secret that the initiator \(the Windows client\) uses to authenticate the target \(the storage volume\)\. This secret allows the target to read and write to the initiator\. This secret is the same as the secret typed into the **Secret used to Authenticate Target \(Mutual CHAP\)** box in the **Configure CHAP Authentication** dialog box\. For more information, see [Configuring CHAP Authentication for Your iSCSI Targets](#ConfiguringiSCSIClientInitiatorCHAP)\.

   1. If the key that you typed is less than 12 characters or more than 16 characters long, an **Initiator CHAP secret** error dialog box appears\.

      Choose **OK**, and then type the key again\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_27.png)

1. Configure the target with the initiator's secret to complete the mutual CHAP configuration\.

   1. Choose the **Targets** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_29.png)

   1. If the target that you want to configure for CHAP is currently connected, disconnect the target by selecting it and choosing **Disconnect**\.

   1. Select the target that you want to configure for CHAP, and then choose **Connect**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_30.png)

   1. In the **Connect to Target** dialog box, choose **Advanced**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_35.png)

   1. In the **Advanced Settings** dialog box, configure CHAP\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_45.png)

      1. Select **Enable CHAP log on**\.

      1. Type the secret that is required to authenticate the initiator\. This secret is the same as the secret typed into the **Secret used to Authenticate Initiator** box in the **Configure CHAP Authentication** dialog box\. For more information, see [Configuring CHAP Authentication for Your iSCSI Targets](#ConfiguringiSCSIClientInitiatorCHAP)\.

      1. Select **Perform mutual authentication**\.

      1. To apply the changes, choose **OK**\.

   1. In the **Connect to Target** dialog box, choose **OK**\. 

1. If you provided the correct secret key, the target shows a status of **Connected**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/ConfigureChap_50.png)<a name="ConfiguringiSCSIClientInitiatorCHAPLinux"></a>

**To configure mutual CHAP on a Red Hat Linux client**

In this procedure, you configure CHAP in the Linux iSCSI initiator using the same keys that you used to configure CHAP for the volume on the AWS Storage Gateway console\.

1. Ensure that the iSCSI daemon is running and that you have already connected to a target\. If you have not completed these two tasks, see [Connecting to a Microsoft Windows Client](GettingStarted-create-tape-gateway.md#iscsi-vtl-windows)\.

1. Disconnect and remove any existing configuration for the target for which you are about to configure CHAP\.

   1. To find the target name and ensure it is a defined configuration, list the saved configurations using the following command\.

      ```
      sudo /sbin/iscsiadm --mode node
      ```

   1. Disconnect from the target\.

      The following command disconnects from the target named **myvolume** that is defined in the Amazon iSCSI qualified name \(IQN\)\. Change the target name and IQN as required for your situation\.

      ```
      sudo /sbin/iscsiadm --mode node --logout GATEWAY_IP:3260,1 iqn.1997-05.com.amazon:myvolume
      ```

   1. Remove the configuration for the target\.

      The following command removes the configuration for the **myvolume** target\.

      ```
      sudo /sbin/iscsiadm --mode node --op delete --targetname iqn.1997-05.com.amazon:myvolume
      ```

1. Edit the iSCSI configuration file to enable CHAP\.

   1. Get the name of the initiator \(that is, the client you are using\)\.

      The following command gets the initiator name from the `/etc/iscsi/initiatorname.iscsi` file\.

      ```
      sudo cat /etc/iscsi/initiatorname.iscsi
      ```

      The output from this command looks like this:

      `InitiatorName=iqn.1994-05.com.redhat:8e89b27b5b8`

   1. Open the `/etc/iscsi/iscsid.conf` file\.

   1. Uncomment the following lines in the file and specify the correct values for *username*, *password*, *username\_in*, and *password\_in*\.

      ```
      node.session.auth.authmethod = CHAP
      node.session.auth.username = username
      node.session.auth.password = password
      node.session.auth.username_in = username_in
      node.session.auth.password_in = password_in
      ```

      For guidance on what values to specify, see the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/initiator-connection-common.html)

   1. Save the changes in the configuration file, and then close the file\.

1. Discover and log in to the target\. To do so, follow the steps in [Connecting to a Microsoft Windows Client](GettingStarted-create-tape-gateway.md#iscsi-vtl-windows)\.