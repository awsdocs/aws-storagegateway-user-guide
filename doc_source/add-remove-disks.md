# Removing Disks from Your Gateway<a name="add-remove-disks"></a>

Although we don’t recommend removing the underlying disks from your gateway, you might want to remove a disk from your gateway, for example if you have a failed disk\.

## Removing a Disk from a Gateway Hosted on VMware ESXi<a name="remove-disk-vmware"></a>

You can use the following procedure to remove a disk from your gateway hosted on VMware hypervisor\.<a name="CachedLocalDiskUploadBufferSizing-commonRemovingConsoleVMware"></a>

**To remove a disk allocated for the upload buffer \(VMware ESXi\)**

1. In the vSphere client, open the context \(right\-click\) menu, choose the name of your gateway VM, and then choose **Edit Settings**\. 

1. On the **Hardware** tab of the **Virtual Machine Properties** dialog box, select the disk allocated as upload buffer space, and then choose **Remove**\.

   Verify that the **Virtual Device Node** value in the **Virtual Machine Properties** dialog box has the same value that you noted previously\. Doing this helps ensure that you remove the correct disk\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_40.png)

1. Choose an option in the **Removal Options** panel, and then choose **OK** to complete the process of removing the disk\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/GatewayMaintenance_41.png)

## Removing a Disk from a Gateway Hosted on Microsoft Hyper\-V<a name="remove-disk-hyperV"></a>

Using the following procedure, you can remove a disk from your gateway hosted on a Microsoft Hyper\-V hypervisor\.<a name="CachedLocalDiskUploadBufferSizing-commonRemovingConsoleHyperV"></a>

**To remove an underlying disk allocated for the upload buffer \(Microsoft Hyper\-V\)**

1. In the Microsoft Hyper\-V Manager, open the context \(right\-click\) menu, choose the name of your gateway VM, and then choose **Settings**\. 

1. In the **Hardware** list of the **Settings** dialog box, select the disk to remove, and then choose **Remove**\.

   The disks you add to a gateway appear under the **SCSI Controller** entry in the **Hardware** list\. Verify that the **Controller** and **Location** value are the same value that you noted previously\. Doing this helps ensure that you remove the correct disk\. 

   The first SCSI controller displayed in the Microsoft Hyper\-V Manager is controller 0\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/hyperv-vm-settings14.png)

1. Choose **OK** to apply the change\.