# Tagging Storage Gateway Resources<a name="tagging-resources-common"></a>

In AWS Storage Gateway, you can use tags to manage your resources\. Tags let you add metadata to your resources and categorize your resources to make them easier to manage\. Each tag consists of a key\-value pair, which you define\. You can add tags to gateways, volumes, and virtual tapes\. You can search and filter these resources based on the tags you add\.

As an example, you can use tags to identify Storage Gateway resources used by each department in your organization\. You might tag gateways and volumes used by your accounting department like this: \(`key=department` and `value=accounting`\)\. You can then filter with this tag to identify all gateways and volumes used by your accounting department and use the information to determine cost\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) and [Working with Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\.

If you archive a virtual tape that is tagged, the tape maintains its tags in the archive\. Similarly, if you retrieve a tape from the archive to another gateway, the tags are maintained in the new gateway\. 

For file gateway, you can use tags to control access to resources\. For information about how to do this, see [Using Tags to Control Access to Your Gateway and Resources](restrict-fgw-access.md)\.

Tags don’t have any semantic meaning but rather are interpreted as strings of characters\.

The following restrictions apply to tags:
+ Tag keys and values are case\-sensitive\.
+ The maximum number of tags for each resource is 50\.
+ Tag keys cannot begin with `aws:`\. This prefix is reserved for AWS use\.
+ Valid characters for the key property are UTF\-8 letters and numbers, space, and special characters \+ \- = \. \_ : / and @\.

## Working with Tags<a name="working-with-tags-common"></a>

You can work with tags by using the Storage Gateway console, the Storage Gateway API, or the [Storage Gateway Command Line Interface \(CLI\)](https://docs.aws.amazon.com/cli/latest/reference/storagegateway/index.html)\. The following procedures show you how to add, edit, and delete a tag on the console\.

**To add a tag**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose the resource you want to tag\. 

   For example, to tag a gateway, choose **Gateways**, and then choose the gateway you want to tag from the list of gateways\.

1. Choose **Tags**, and then choose **Add/edit tags**\.

1. In the **Add/edit tags** dialog box, choose **Create tag**\.

1. Type a key for **Key** and a value for **Value**\. For example, you can type **Department** for the key and **Accounting** for the value\.
**Note**  
You can leave the **Value** box blank\.

1. Choose **Create Tag** to add more tags\. You can add multiple tags to a resource\.

1. When you’re done adding tags, choose **Save**\.

**To edit a tag**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose the resource whose tag you want to edit\.

1. Choose **Tags** to open the **Add/edit tags** dialog box\.

1. Choose the pencil icon next to the tag you want edit, and then edit the tag\. 

1. When you’re done editing the tag, choose **Save**\.

**To delete a tag**

1. Open the AWS Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose the resource whose tag you want to delete\.

1. Choose **Tags**, and then choose **Add/edit tags** to open the **Add/edit tags** dialog box\.

1. Choose the **X** icon next to the tag you want to delete, and then choose **Save**\. 

## See Also<a name="see-also-tags"></a>

[Using Tags to Control Access to Your Gateway and Resources](restrict-fgw-access.md)