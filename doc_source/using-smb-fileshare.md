# Mounting Your SMB File Share on Your Client<a name="using-smb-fileshare"></a>

Now you mount your SMB file share and map to a drive accessible to your client\. The console's file gateway section shows the supported mount commands that you can use for SMB clients\. Following, you can find some additional options to try\.

You can use several different methods for mounting SMB file shares, including the following:
+ The `net use` command – Doesn't persist across system reboots, unless you use the `/persistent:(yes:no)` switch\. The specific command that you use depends on whether you plan to use your file share for Microsoft Active Directory \(AD\) access or guest access\.
+ The `CmdKey` command line utility – Creates a persistent connection to a mounted SMB file share that remains after a reboot\.
+ A network drive mapped in File Explorer – Configures the mounted file share to reconnect at sign\-in and to require that you enter your network credentials\.
+ PowerShell script – Can be persistent, and can be either visible or invisible to the operating system while mounted\.

**Note**  
If you are a Microsoft AD user, check with your administrator to ensure that you have access to the SMB file share before mounting the file share to your local system\.  
If you are a guest user, make sure that you have the guest user account password before attempting to mount the file share\.

**To mount your SMB file share for Microsoft AD users using the net use command**

1. Make sure that you have access to the SMB file share before mounting the file share to your local system\.

1. For Microsoft AD clients, type the following command at the command prompt:

   **net use *\[WindowsDriveLetter\]*: \\\\*\[Gateway IP Address\]*\\*\[File share name\]***

**To mount your SMB file share for guest users using the net use command**

1. Make sure that you have the guest user account password before mounting the file share\.

1. For Windows guest clients, type the following command at the command prompt\.

   **net use *\[WindowsDriveLetter\]*: \\\\$*\[Gateway IP Address\]*\\$*\[path\]* /user:$*\[Gateway ID\]*\\smbguest**

**To mount an SMB file share on Windows using CmdKey:**

1. Press the Windows key and type **cmd** to view the command prompt menu item\.

1. Open the context \(right\-click\) menu for **Command Prompt** and choose **Run as administrator**\.

1. Type the following command:

   **C:\\>cmdkey /add:*\[Gateway VM IP address\]* /user:*\[DomainName\]*\\*\[UserName\]* /pass:*\[Password\]***

**Note**  
When mounting file shares, be aware of the following:  
You might have a case where a folder and an object exist in an Amazon S3 bucket and have the same name\. In this case, if the object name doesn't contain a trailing slash, only the folder is visible in a file gateway\. For example, if a bucket contains an object named `test` or `test/` and a folder named `test/test1`, only `test/` and `test/test1` are visible in a file gateway\.
You might need to remount your file share after a reboot of your client\.

**To mount an SMB file share using Windows File Explorer**

1. Press the Windows key and type **File Explorer** in the **Search Windows** box, or press **Win\+E**\.

1. In the navigation pane, choose **This PC**, then choose **Map Network Drive** for **Map Network Drive** in the **Computer** tab, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/userguide/images/map-on-windows-explorer.png)  
  


1. In the **Map Network Drive** dialog box, choose a drive letter for **Drive**\. 

1. For **Folder**, type **\\\\*\[File Gateway IP\]*\\*\[SMB File Share Name\]***, or choose **Browse** to select your SMB file share from the dialog box\.

1. \(Optional\) Select **Reconnect at sign\-up** if you want your mount point to persist after reboots\.

1. \(Optional\) Select **Connect using different credentials** if you want a user to enter the Microsoft AD logon or guest account user password\.

1. Choose **Finish** to complete your mount point\.

You can edit file share settings, edit allowed and denied users and groups, and change the guest access password from the Storage Gateway Management Console\. You can also refresh the data in the file share's cache and delete a file share from the console\.

**To modify your SMB file share's properties**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **File Shares**\.

1. On the **File Share** page, select the check box by the SMB file share that you want to modify\.

1. For Actions, choose the action that you want:
   + Choose **Edit file share settings** to modify share access\. 
   + Choose **Edit allowed/denied users** to add or delete users and groups, and then type the allowed and denied users and groups into the **Allowed Users**, **Denied Users**, **Allowed Groups**, and **Denied Groups** boxes\. Use the **Add Entry** buttons to create new access rights, and the **\(X\)** button to remove access\.

1. When you're finished, choose **Save**\.

   When you enter allowed users and groups, you are creating a whitelist\. Without a whitelist, all authenticated Microsoft AD users can access the SMB file share\. Any users and groups that are marked as denied are added to a blacklist and can't access the SMB file share\. In instances where a user or group is on both the blacklist and whitelist, the blacklist takes precedence\.

   You can enable Access Control Lists\(ACLs\) on your SMB file share\. For information about how to enable ACLs, see [Using Microsoft Windows ACLs to Control Access to an SMB File Share](smb-acl.md)\.

**Next Step**

[Testing Your File Gateway](GettingStartedTestFileShare.md)