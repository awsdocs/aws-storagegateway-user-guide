# Using tags to control access to your gateway and resources<a name="restrict-fgw-access"></a>

To control access to gateway resources and actions, you can use AWS Identity and Access Management \(IAM\) policies based on tags\. You can provide the control in two ways:

1. Control access to gateway resources based on the tags on those resources\.

1. Control what tags can be passed in an IAM request condition\.

For information about how to use tags to control access, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.

## Controlling access based on tags on a resource<a name="resorce-tag-control"></a>

To control what actions a user or role can perform on a gateway resource, you can use tags on the gateway resource\. For example, you might want to allow or deny specific API operations on a file gateway resource based on the key\-value pair of the tag on the resource\.

The following example allows a user or a role to perform the `ListTagsForResource`, `ListFileShares`, and `DescribeNFSFileShares` actions on all resources\. The policy applies only if the tag on the resource has its key set to `allowListAndDescribe` and the value set to `yes`\.

```
{
  "Version": "2012-10-17",
   "Statement": [
      {
          "Effect": "Allow",
                    "Action": [
                        "storagegateway:ListTagsForResource",
                        "storagegateway:ListFileShares",
                        "storagegateway:DescribeNFSFileShares"
                    ],
                    "Resource": "*",
                    "Condition": {
                        "StringEquals": {
                            "aws:ResourceTag/allowListAndDescribe": "yes"
                        }
                    }
      },
      {
          "Effect": "Allow",
          "Action": [
              "storagegateway:*"
          ],
          "Resource": "arn:aws:storagegateway:region:account-id:*/*"
      }
  ]
}
```

## Controlling access based on tags in an IAM request<a name="request-based-control"></a>

To control what an IAM user can do on a gateway resource, you can use conditions in an IAM policy based on tags\. For example, you can write a policy that allows or denies an IAM user the ability to perform specific API operations based on the tag they provided when they created the resource\.

In the following example, the first statement allows a user to create a gateway only if the key\-value pair of the tag they provided when creating the gateway is **Department** and **Finance**\. When using the API operation, you add this tag to the activation request\.

The second statement allows the user to create a Network File System \(NFS\) or Server Message Block \(SMB\) file share on a gateway only if the key\-value pair of the tag on the gateway matches **Department**and **Finance**\. Additionally, the user must add a tag to the file share, and the key\-value pair of the tag must be **Department** and **Finance**\. You add tags to a file share when creating the file share\. There aren't permissions for the `AddTagsToResource` or `RemoveTagsFromResource` operations, so the user can't perform these operations on the gateway or the file share\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "storagegateway:ActivateGateway"
         ],
         "Resource":"*",
         "Condition":{
            "StringEquals":{
               "aws:RequestTag/Department":"Finance"
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "storagegateway:CreateNFSFileShare",
            "storagegateway:CreateSMBFileShare"
         ],
         "Resource":"*",
         "Condition":{
            "StringEquals":{
               "aws:ResourceTag/Department":"Finance",
               "aws:RequestTag/Department":"Finance"
            }
         }
      }
   ]
}
```