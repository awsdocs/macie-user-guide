# Integrating Member Accounts and Amazon S3 with Amazon Macie<a name="macie-integration"></a>

You can use the Macie console's **Integrations** tab to integrate member accounts with Macie and to integrate Amazon S3 with Macie for both your master account and member accounts\. For more information about the master and member accounts, see [Concepts and Terminology](macie-concepts.md)\.

**Topics**
+ [Integrating Member Accounts with Macie](#macie-integration-member)
+ [Specifying Data for Macie to Monitor](#macie-integration-services)
+ [Encrypted Objects](#macie-encrypted-objects)

## Integrating Member Accounts with Macie<a name="macie-integration-member"></a>

When you integrate member accounts with Macie, you're enabling Macie to monitor resources and activity in these member accounts\. 

**To integrate member accounts with Macie**

1. Log in to AWS with the credentials of the AWS account that you want to integrate with Macie as a member account\.

1. 
**Important**  
This is a required step if you're adding an account as a Macie member account for the first time\.  
You can skip this step if you're readding an account as a Macie member account after disassociating it from Macie\.

   Create the IAM role called `AmazonMacieHandshakeRole` that grants this account the required permissions to be successfully integrated with Macie\. You can create this role by launching the AWS CloudFormation stack templates found at the URLs listed below\. You need to create this role only once for use in all Regions\. For more information about AWS CloudFormation and AWS CloudFormation stacks, see [What is AWS CloudFormation?](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) and [Working with Stacks](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)\. 
**Important**  
Make sure to specify the master account ID when running the following stack templates:  
US East \(N\. Virginia\): [Macie AWS CloudFormation template for a member account](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=MacieServiceRolesMembers&templateURL=https://s3.amazonaws.com/us-east-1.macie-redirection/cfntemplates/MacieServiceRolesMember.template)
US West \(Oregon\): [Macie AWS CloudFormation template for a member account](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=MacieServiceRolesMember&templateURL=https://s3-us-west-2.amazonaws.com/us-west-2.macie-redirection/cfntemplates/MacieServiceRolesMember.template)

1. Log in to AWS with the Macie master account, navigate to the Macie console, and choose the **Integrations** tab\.

1. To integrate a member account, choose the **\+** icon next to **Member accounts**\.

1. In the **Add member AWS account\(s\)** pop\-up window, enter one or more account IDs\. Separate multiple account numbers with commas\. Choose **Add accounts**\.

**Important**  
Once Macie is enabled in this member account, Macie is assigned a service\-linked role called `AWSServiceRoleForAmazonMacie`\. This service\-linked role includes the permissions and the trust policy that Macie requires to discover, classify, and protect sensitive data in AWS in this account and to generate alerts about potential security issues\. The following are the details of `AWSServiceRoleForAmazonMacie`\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Resource": "*",
            "Action": [
                "cloudtrail:DescribeTrails",
                "cloudtrail:GetEventSelectors",
                "cloudtrail:GetTrailStatus",
                "cloudtrail:ListTags",
                "cloudtrail:LookupEvents",
                "iam:ListAccountAliases",
                "s3:Get*",
                "s3:List*"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": "arn:aws:cloudtrail:*:*:trail/AWSMacieTrail-DO-NOT-EDIT",
            "Action": [
                "cloudtrail:CreateTrail",
                "cloudtrail:StartLogging",
                "cloudtrail:StopLogging",
                "cloudtrail:UpdateTrail",
                "cloudtrail:DeleteTrail",
                "cloudtrail:PutEventSelectors"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::awsmacie-*",
                "arn:aws:s3:::awsmacietrail-*",
                "arn:aws:s3:::*-awsmacietrail-*"
            ],
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:DeleteBucketPolicy",
                "s3:DeleteBucketWebsite",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:DeleteObjectVersion",
                "s3:DeleteObjectVersionTagging",
                "s3:DeleteReplicationConfiguration",
                "s3:PutBucketPolicy"
            ]
        }
    ]
}
```
 For more information, see [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)\. For more information about service\-linked roles, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)\. 

If you disable Macie, the service no longer has access to the resources in your member account\. However, after you disable Macie, the `AmazonMacieHandshakeRole` IAM role that you created for this member account in step 2 and the `AWSServiceRoleForAmazonMacie` service\-linked role that was automatically created for this member account when it was integrated with Macie remain intact\. These existing roles are used again if you decide to reenable Macie for this member account\. To reenable Macie for a member account, you must use the previous steps to integrate this member account with Macie\.

## Specifying Data for Macie to Monitor<a name="macie-integration-services"></a>

You can use the **S3 Resources** tab on the **Integrations** tab to specify the S3 buckets and prefixes that contain the data that you want Macie to monitor\. 

**Important**  
Currently, Macie can only monitor objects stored in Amazon S3\.

**Important**  
Macie has a default limit on the amount of data that it can classify in an account\. Once this data limit is reached, Macie stops classifying the data in this account\. The default data classification limit is 3 TB\. You can contact AWS Support and request an increase to the default limit\. 

You can also specify S3 buckets and prefixes for Macie to monitor during the initial Macie setup\. For more information and instructions, see [Setting Up Amazon Macie](macie-setting-up.md)\.

1. Log in to AWS with the credentials of the account that is your Macie master account\.

1. In the Macie console's **Integrations** tab, choose the **S3 Resources** tab\. 

1. In the **S3 Resources** tab, select the account ID \(master or member\) in the **Select an account** dropdown and then choose **Select**\.

1. On the **Integrate S3 resources with Macie** page, choose either **Edit** to edit the buckets/prefixes that are already integrated with Macie or **Add** to select new buckets/prefixes to integrate with Macie\. 
**Note**  
You can select up to 250 S3 buckets and prefixes\. You can only select S3 buckets in your current AWS Region\.

1. Configure the methods for classifying your new and existing S3 objects\. The continuous classification method is applied to new objects that are added or updated after your buckets selection is complete\. Continuous classification of new data enables S3 object\-level logging for all selected buckets and prefixes\. The one\-time classification method is applied only once to all of the existing objects in the selected S3 buckets/prefixes\.
**Important**  
In the current Macie release, you can only select **Full** as the setting for the continuous classification \(classifying new objects\), and you can select **Full** or **None** as the settings for the one\-time classification method \(classifying all existing objects\)\.
**Important**  
If you enable one\-time classification, note the following values that are displayed for each selected bucket/prefix:  
Total objects – Total number of objects in this S3 bucket/prefix\.
Processed estimate – Total size of the data from this bucket that Macie will actually classify\. 
Cost estimate – Cost estimate for classifying objects in this bucket\.
Macie also provides you with several totals for all of the selected buckets:  
**Total size** – Total size of the data within all of the selected buckets\.
**Total number of objects** – Total number of objects in all of the selected S3 buckets\.
**Processed estimate** – Total size of the data from all of the selected buckets that Macie will actually classify\. 
**Total cost estimate** – Total content classification cost estimate for all selected buckets\.
The **Total cost estimate** for each bucket is based on the **Processed estimate** value for that bucket and the general Macie pricing of $5 per GB processed by the content classification engine\. The total cost estimates are provided only for S3 buckets, not for prefixes\. For more information, see [Amazon Macie Pricing](https://aws.amazon.com/macie/pricing/)\.   
The **Processed estimate** value for each bucket is calculated as follows:  
If an object's size is less than 1 KB, 1 KB is added to the **Processed estimate** value\. Otherwise, the object's actual size is added to the **Processed estimate** value\.
If the object's size is greater than 20 MB, 20 MB is added to the ** Processed estimate** value\. Otherwise, the object's actual size is added to the **Processed estimate** value\.
For objects in Amazon S3 Glacier vaults, 0 is added to the ** Processed estimate** value\.
It's possible for the **Processed estimate** value of an S3 bucket to be higher than the **Total size** value\.  
The one\-time classification cost estimates are only calculated per S3 buckets and *not* per S3 bucket prefixes\. If you select an S3 bucket prefix, the cost estimate for the entire S3 bucket is included in the total cost estimate summary for the selected resources\. If you select multiple prefixes of the same S3 bucket, the cost estimate for this entire S3 bucket is included only once in the total cost estimate summary for the selected resources\.

1. When you have finished your selections, choose **Review**\.

1. When you have finished reviewing your selections, choose **Start classification**\.

## Encrypted Objects<a name="macie-encrypted-objects"></a>

If objects stored in your Amazon S3 buckets are encrypted, Macie might not be able to read and classify those objects for the following reasons: 
+ If your Amazon S3 objects are encrypted using [Amazon S3–managed encryption keys \(SSE\-S3\)](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html), Macie can read and classify the objects using the roles created during the setup process\.
+ If your Amazon S3 objects are encrypted using [AWS KMS–managed keys \(SSE\-KMS\)](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html), Macie can read and classify the objects only if you add the `AWSMacieServiceCustomerServiceRole` IAM role or the `AWSServiceRoleForAmazonMacie` service\-linked role as a [key user](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users) for the KMS customer master key \(CMK\)\. If you don't add either of these roles as a key user for the KMS CMK, Macie can't read and classify the objects\. However, Macie still stores metadata on the object, including which KMS CMK was used to protect the object\.
+ If your Amazon S3 objects are encrypted using client\-side encryption, Macie can't read and classify the objects, but still stores metadata on the object\.