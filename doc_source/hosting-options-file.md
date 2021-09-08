# Choose a host platform and download the VM<a name="hosting-options-file"></a>

If you create your gateway on premises, you deploy the hardware appliance, or download and deploy a gateway VM, and then activate the gateway\. If you create your gateway on an Amazon EC2 instance, you launch an Amazon Machine Image \(AMI\) that contains the gateway VM image and then activate the gateway\. For information about supported host platforms, see [Supported hypervisors and host requirements](Requirements.md#requirements-host)\.

**Note**  
You can run only file, cached volume, and tape gateways on an Amazon EC2 instance\.

**To choose a host platform and download the VM**

1. For **Select host platform**, choose the virtualization platform that you want to run your gateway on\.

1. Do one of the following:
   + If you choose the hardware appliance, activate it by following the instructions in [Activating your hardware appliance](appliance-activation.md)\.
   + If you choose one of the other options, choose **Download image** next to your virtualization platform to download a \.zip file that contains the \.ova file for your virtualization platform\.
**Note**  
The \.zip file is over 500 MB in size and might take some time to download, depending on your network connection\. 

     For Amazon EC2, you create an instance from the provided AMI\.

1. If you choose a hypervisor option, deploy the downloaded image to your hypervisor\. Add at least one local disk for your cache and one local disk for your upload buffer during the deployment\. A file gateway requires only one local disk for a cache\. For information about local disk requirements, see [Hardware and storage requirements](Requirements.md#requirements-hardware-storage)\.

   Depending on your hypervisor, you can set specific options:
   + If you choose VMware, do the following:
     + Store your disk using the **Thick provisioned format** option\. When you use thick provisioning, the disk storage is allocated immediately, resulting in better performance\. In contrast, thin provisioning allocates storage on demand\. On\-demand allocation can affect the normal functioning of Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in thick\-provisioned format\.
   + If you choose Microsoft Hyper\-V, do the following: 
     + Configure the disk type using the **Fixed size** option\. When you use fixed\-size provisioning, the disk storage is allocated immediately, resulting in better performance\. If you don't use fixed\-size provisioning, the storage is allocated on demand\. On\-demand allocation can affect the functioning of Storage Gateway\. For Storage Gateway to function properly, the VM disks must be stored in fixed\-size provisioned format\. 
     + When allocating disks, choose **virtual hard disk \(\.vhd\) file**\. Storage Gateway supports the \.vhdx file type\. By using this file type, you can create larger virtual disks than with other file types\. If you create a \.vhdx type virtual disk, make sure that the size of the virtual disks that you create doesn't exceed the recommended disk size for your gateway\.
   + If you choose Linux Kernel\-based Virtual Machine \(KVM\), do the following: 
     + Don't configure your disk to use `sparse` formatting\. When you use fixed\-size \(nonsparse\) provisioning, the disk storage is allocated immediately, resulting in better performance\.
     + Use the parameter `sparse=false` to store your disk in nonsparse format when creating new virtual disks in the VM with the `virt-install` command for provisioning new virtual machines\.
     + Use `virtio` drivers for disk and network devices\.
     + We recommend that you don't set the `current_memory` option\. If necessary, set it equal to the RAM provisioned to the gateway in the `--ram` parameter\.

     Following is an example `virt-install` command for installing KVM\.

     ```
     virt-install --name "SGW_KVM" --description "SGW KVM" --os-type=generic --ram=32768 --vcpus=16 --disk path=fgw-kvm.qcow2,bus=virtio,size=80,sparse=false --disk path=fgw-kvm-cache.qcow2,bus=virtio,size=1024,sparse=false --network default,model=virtio --graphics none --import
     ```

**Note**  
For VMware, Microsoft Hyper\-V, and KVM, synchronizing the VM time with the host time is required for successful gateway activation\. Make sure that your host clock is set to the correct time and synchronize it with a Network Time Protocol \(NTP\) server\. 

For information about deploying your gateway to an Amazon EC2 host, see [Deploying a file gateway on an Amazon EC2 host](ec2-gateway-file.md)\.