# Using Tags to Control Access to File Gateway Resources<a name="restrict-fgw-access"></a>

You can use AWS Identity and Access Management \(IAM\) policies to control access to file gateway resources and actions based on tags\. You can provide the control in two ways:

1. Control access to file gateway resources based on the tags on those resources\.

1. Control what tags can be passed in an IAM request condition\.

For information about how to use tags to control access, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.

## Controlling Access Based on Tags on a Resource<a name="resorce-tag-control"></a>

You can use the tags on a file gateway resource to control what actions a user or role can perform on the resource\. For example, you might want to allow or deny specific API actions on a file gateway resource based on the key\-value pair of the tag on the resource\.

The following example allows a user or a role to perform the `ListTagsForResource`, `ListFileShares` and the `DescribeNFSFileShares` actions on all resources\. The policy applies only if the tag on the resource has its key set to `allowListAndDescribe` and the value set to `yes`\.

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

## Controlling Access Based on Tags in an IAM Request<a name="request-based-control"></a>

You can use conditions in an IAM policy to control what an IAM user can do based on tags on a file gateway resource\. For example, you can write a policy that allows or denies an IAM user the ability to perform specific API operations based on the tag they provided when they were creating the resource\.

In the following example, the first statement allows a user to create a gateway only if the key\-value pair of the tag they provided when creating the gateway is **Department** and **Finance**\. When using the API, you add this tag to the activation request\.

The second statement allows the user to create an Network File System \(NFS\) or Server Message Block \(SMB\) file share on a gateway only if the key\-value pair of the tag on the gateway matches **Department**and **Finance**\. Additionally, the user must add a tag to the file share, and the key\-value pair the tag of must be **Department** and **Finance**\. You add tags to a file share when creating the file share\. There aren't permissions for the `AddTagsToResource` or `RemoveTagsFromResource` operations, so the user can't perform these operations on the gateway or the file share\.

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