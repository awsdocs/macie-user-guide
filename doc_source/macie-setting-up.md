# Setting Up Amazon Macie<a name="macie-setting-up"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including Amazon Macie\. If you don't have an account, use the following procedure to create one\. 

**To sign up for AWS**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
If you previously signed in to the AWS Management Console using AWS account root user credentials, choose **Sign in to a different account**\. If you previously signed in to the console using IAM credentials, choose **Sign\-in using root account credentials**\. Then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code using the phone keypad\.

**Topics**
+ [Step 1: Enable Macie](#macie-setting-up-enable)
+ [Step 2: Integrate Amazon S3 with Macie](#macie-integrates3)
+ [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)

## Step 1: Enable Macie<a name="macie-setting-up-enable"></a>

When you launch the Macie console for the first time, choose **Get Started** and complete the following procedure to enable Macie\. 

**Important**  
The AWS account that you use to enable Macie is automatically designated as your master account\. For more information, see [Concepts and Terminology](macie-concepts.md)\.

**To enable Amazon Macie**

1. The IAM identity \(user, role, group\) that you use to enable Macie must have the required permissions\. To grant the permissions required to enable Macie, attach the `AmazonMacieFullAccess` managed policy to this IAM user, group, or role\. For more information, see [ Managed \(Predefined\) Policies for Macie](macie-access-control.md#managed-policies)\. 

1. Use the credentials of the IAM identity from step 1 to sign in to the Macie console\. When you open the Macie console for the first time, choose **Get Started**\. 

1. On the **Enable Amazon Macie** page, verify Region preferences by reviewing the value in the dropdown menu under the **Region** section\.
**Note**  
The region that you're signed in to is automatically selected\.

1. Choose **Enable Macie**\.

   Note the following about enabling Macie:
   + Macie is assigned a service\-linked role called `AWSServiceRoleForAmazonMacie`\. This service\-linked role includes the permissions and trust policy that Macie requires to discover, classify, and protect sensitive data in AWS on your behalf and to generate alerts about potential security issues\. To view the details of `AWSServiceRoleForAmazonMacie`, on the **Enable Amazon Macie** page, choose **View service role permissions**\. For more information, see [Using Service\-Linked Roles for Amazon Macie](using-service-linked-roles.md)\. For more information about service\-linked roles, see [Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html)\. 
   + After you enable Macie, it immediately begins pulling and analyzing independent streams of data from AWS CloudTrail to generate alerts\. Because Macie consumes this data only to determine if there are potential security issues, Macie doesn't manage CloudTrail for you or make its events and logs available to you\. If you have enabled CloudTrail independent of Macie, you continue to have the option to configure its settings through the CloudTrail console or APIs\. For more information, see [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
   + You can disable Macie at any time to stop it from processing and analyzing CloudTrail events\. For more information, see [Disabling Amazon Macie and Deleting Collected Metadata](macie-disable.md)\.

## Step 2: Integrate Amazon S3 with Macie<a name="macie-integrates3"></a>

To classify and protect your data, Macie analyzes and processes information from CloudTrail and Amazon S3\. Enabling CloudTrail in your account is required to enable Macie\. Integrating S3 with Macie \(in other words, initially specifying one or more S3 buckets for Macie to monitor\) is not required to enable Macie\. However, we strongly recommend that you integrate with Amazon S3 as part of setting up Macie and specify at least one S3 bucket that contains objects that you want Macie to classify and monitor\. For more information and details about how Macie classifies your data, see [Classifying Data with Amazon Macie](macie-classify-data.md)\.

When you integrate with Amazon S3, Macie creates a trail and a bucket to store the logs about the Amazon S3 object\-level API activity \(data events\) that it will analyze, along with other CloudTrail logs that it processes\. 

**Important**  
Macie has a default limit on the amount of data that it can classify in an account\. Once this data limit is reached, Macie stops classifying the data in this account\. The default data classification limit is 3 TB\. You can contact AWS Support and request an increase to the default limit\. 

You can use the following procedure to integrate with Amazon S3 as part of setting up Macie:

1. Log in to AWS with the credentials of the account that is serving as your Macie master account\.

1. On the Macie console's **Integrations** tab, choose the **S3 Resources** tab\. 

1. On the **S3 Resources** tab, select the account ID \(master or member\) in the **Select an account** dropdown and then choose **Select**\.

1. On the **Integrate S3 resources with Macie** page, choose either **Edit** to edit the buckets/prefixes that are already integrated with Macie or **Add** to select new buckets/prefixes to integrate with Macie\. 
**Note**  
You can select up to 250 S3 buckets and prefixes\. You can only select S3 buckets in your current AWS Region\.

1. Next, you must configure the methods for classifying your new and existing S3 objects\. The continuous classification method is applied to new objects that are added or updated after your buckets selection is complete\. Continuous classification of new data enables S3 object\-level logging for all selected buckets and prefixes\. The one\-time classification method is applied only once to all of the existing objects in the selected S3 buckets/prefixes\.
**Important**  
In the current Macie release, you can only select **Full** as the setting for the continuous classification \(classifying new objects\), and you can select **Full** or **None** as the settings for the one\-time classification method \(classifying all existing objects\)\.
**Important**  
If you enable one\-time classification, the following values are displayed for each selected bucket/prefix:  
**Total objects** – Total number of objects in this S3 bucket/prefix\.
**Processed estimate** – Total size of the data from this bucket that Macie will actually classify\. 
**Cost estimate** – Cost estimate for classifying objects in this bucket\.
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