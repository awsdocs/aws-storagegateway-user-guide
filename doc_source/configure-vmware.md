# Configuring VMware for Storage Gateway<a name="configure-vmware"></a>

When configuring VMware for Storage Gateway, make sure to synchronize your VM time with your host time, configure VM to use paravirtualized disk controllers when provisioning storage and provide protection from failures in the infrastructure layer supporting a gateway VM\.

**Topics**
+ [Synchronizing VM Time with Host Time](#GettingStartedSyncVMTime-common)
+ [Using Storage Gateway with VMware High Availability](#UsingWithVMwareHAVmware-common)

## Synchronizing VM Time with Host Time<a name="GettingStartedSyncVMTime-common"></a>

To successfully activate your gateway, you must ensure that your VM time is synchronized to the host time, and that the host time is correctly set\. In this section, you first synchronize the time on the VM to the host time\. Then you check the host time and, if needed, set the host time and configure the host to synchronize its time automatically to a Network Time Protocol \(NTP\) server\. 

**Important**  
Synchronizing the VM time with the host time is required for successful gateway activation\.

**To synchronize VM time with host time**

1. Configure your VM time\.

   1. In the vSphere client, open the context \(right\-click\) menu for your gateway VM, and choose **Edit Settings**\.

      The **Virtual Machine Properties** dialog box opens\.

         
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSProvisionStorageforAppliance_11.png)

   1. Choose the **Options** tab, and choose **VMware Tools** in the options list\.

   1. Check the **Synchronize guest time with host** option, and then choose **OK**\.

      The VM synchronizes its time with the host\. 

         
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSyncVMTime15_small.png)

1. Configure the host time\. 

   It is important to make sure that your host clock is set to the correct time\. If you have not configured your host clock, perform the following steps to set and synchronize it with an NTP server\.

   1. In the VMware vSphere client, select the vSphere host node in the left pane, and then choose the **Configuration** tab\.

   1. Select **Time Configuration** in the **Software** panel, and then choose the **Properties** link\. 

      The **Time Configuration** dialog box appears\. 

         
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSettingGatewayTime10_3.png)

   1. In the **Date and Time** panel, set the date and time\.

         
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSettingGatewayTime15_3.png)

   1. Configure the host to synchronize its time automatically to an NTP server\.

      1. Choose **Options** in the **Time Configuration** dialog box, and then in the **NTP Daemon \(ntpd\) Options** dialog box, choose **NTP Settings** in the left pane\.

            
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSettingGatewayTime20_3.png)

      1. Choose **Add** to add a new NTP server\.

      1. In the **Add NTP Server** dialog box, type the IP address or the fully qualified domain name of an NTP server, and then choose **OK**\. 

         You can use `pool.ntp.org` as shown in the following example\.

            
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSettingGatewayTime25_3.png)

      1. In the **NTP Daemon \(ntpd\) Options** dialog box, choose **General** in the left pane\.

      1. In the **Service Commands** pane, choose **Start** to start the service\.

         Note that if you change this NTP server reference or add another later, you will need to restart the service to use the new server\.

            
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/files3/images/GSSettingGatewayTime30_3.png)

   1. Choose **OK** to close the **NTP Daemon \(ntpd\) Options** dialog box\.

   1. Choose **OK** to close the **Time Configuration** dialog box\.

## Using Storage Gateway with VMware High Availability<a name="UsingWithVMwareHAVmware-common"></a>

VMware High Availability \(HA\) is a component of vSphere that can provide protection from failures in the infrastructure layer supporting a gateway VM\. VMware HA does this by using multiple hosts configured as a cluster so that if a host running a gateway VM fails, the gateway VM can be restarted automatically on another host within the cluster\. For more information about VMware HA, see [VMware HA: Concepts and Best Practices](http://www.vmware.com/resources/techresources/402) on the VMware website\.

To use Storage Gateway with VMware HA, we recommend doing the following things:

 
+ Deploy the VMware ESX `.ova` downloadable package that contains the Storage Gateway VM on only one host in a cluster\.
+ When deploying the `.ova` package, select a data store that is not local to one host\. Instead, use a data store that is accessible to all hosts in the cluster\. If you select a data store that is local to a host and the host fails, then the data source might not be accessible to other hosts in the cluster and failover to another host might not succeed\. 
+ With clustering, if you deploy the `.ova` package to the cluster, select a host when you are prompted to do so\. Alternately, you can deploy directly to a host in a cluster\. 