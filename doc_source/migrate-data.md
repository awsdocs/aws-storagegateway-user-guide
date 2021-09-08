# Migrating your Amazon S3 File Gateway<a name="migrate-data"></a>

You can move data between gateways as your data and performance needs grow, or if you receive an AWS notification to migrate your gateway\. You might need to do this for the following reasons:
+ To move your data to better host platforms or newer Amazon EC2 instances\.
+ To refresh the underlying hardware for your server\.

The steps you follow to migrate your gateway depend on the gateway type\.

**Note**  
Data can be moved only between gateways of the same type\.

## <a name="migrate-data-file-gateway"></a>

**To migrate an Amazon S3 File Gateway**

1. Stop any applications that are writing to the existing file gateway\.

1. Verify that the `CachePercentDirty` metric on the **Monitoring** tab for the existing file gateway is `0`\.

1. Shut down the existing file gateway by powering off the host virtual machine \(VM\) using its hypervisor controls\.

   For more information about shutting down an Amazon EC2 instance, see [Stop and start your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html) in the *Amazon EC2 User Guide*\.

   For more information about shutting down a KVM, VMware, or Hyper\-V VM, see your hypervisor documentation\.

1. Detach all disks, including the root disk, cache disks, and upload buffer disks from the old gateway VM\.
**Note**  
Make a note of the root disk's volume ID, as well as the gateway ID associated with that root disk\. You will need to detach this disk from the new storage gateway hypervisor in a later step\.

   If you are using an Amazon EC2 instance as the VM for your file gateway, see [Detach an Amazon EBS volume from a Windows instance](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-detaching-volume.html) or [Detach an Amazon EBS volume from a Linux instance](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-detaching-volume.html) in the *Amazon EC2 User Guide*\.

   For information about detaching disks from a KVM, VMware, or Hyper\-V VM, see the documentation for your hypervisor\. 

1. Create a new AWS Storage Gateway hypervisor VM instance, but don't activate it as a gateway\. In a later step, this new VM will assume the identity of the old gateway\.

   For more information about creating a new Storage Gateway hypervisor VM, see [Choosing a Host Platform and Downloading the VM](https://docs.aws.amazon.com/filegateway/latest/files3/hosting-options-file.html)\.
**Note**  
Do not add cache disks for the new VM\. This VM will use the same cache disks that were used by the old VM\.

1. Configure your new Storage Gateway VM to use the same network settings as the old VM\.

   The default network configuration for the gateway is Dynamic Host Configuration Protocol \(DHCP\)\. With DHCP, your gateway is automatically assigned an IP address\.

   If you need to manually configure a static IP address for your gateway VM, see [Configuring Your Gateway Network](https://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-common.html#MaintenanceConfiguringStaticIP-common)\.

   If your gateway VM must use a Socket Secure version 5 \(SOCKS5\) proxy to connect to the internet, see [Routing Your On\-Premises Gateway Through a Proxy](https://docs.aws.amazon.com/storagegateway/latest/userguide/manage-on-premises-common.html#MaintenanceRoutingProxy-common)\.

1. Start the new Storage Gateway VM\.

1. Attach the disks that you detached from the old gateway VM to the new gateway VM\.
**Note**  
To migrate successfully, all disks must remain unchanged\. Changing the disk size or other values causes inconsistencies in metadata that prevent successful migration\.

1. Initiate the gateway migration process by connecting to the new VM with a URL that uses the following format:

   **http://*your\-VM\-IP\-address*/migrate?gatewayId=*your\-gateway\-ID***

   You can use the same IP address for the new gateway VM that you used for the old gateway VM\. Your URL should look similar to the following example:

   **http://*198\.51\.100\.123*/migrate?gatewayId=*sgw\-12345678***

   Use this URL from a browser, or from the command line using cURL\.

   When the gateway migration initiates successfully, the following message appears:

   ```
   Successfully imported Storage Gateway information. Please refer to Storage Gateway documentation to perform the next steps to complete the migration.
   ```

1. Stop the new Storage Gateway VM\.

1. Detach the old gateway's root disk, whose volume ID you noted previously, from the new gateway\.

1. Start the new Storage Gateway VM\.

1. If your gateway was joined to an Active Directory domain, re\-join the domain\. For instructions, see [Configuring Microsoft Active Directory access ](https://docs.aws.amazon.com/filegateway/latest/files3/CreatingAnSMBFileShare.html#configure-SMB-file-share-AD-access)\.
**Note**  
You must complete this step even if the status of the file gateway appears as **Joined**\.

1. Confirm that your shares are available at the new gateway VM's IP address, then delete the old gateway VM\. 
**Warning**  
When a gateway is deleted, there is no way to recover it\.

   For more information about deleting an Amazon EC2 instance, see [Terminate your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide*\. For more information about deleting a KVM, VMware, or Hyper\-V VM, see the documentation for your hypervisor\.