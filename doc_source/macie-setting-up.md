--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Setting Up Amazon Macie Classic<a name="macie-setting-up"></a>

When you sign up for Amazon Web Services, your Amazon Web Services account is automatically signed up for all services in Amazon Web Services\. The Amazon Web Services account that you used when you enabled Macie Classic is automatically designated as your Macie Classic administrator account\. For more information, see [Concepts and Terminology](macie-concepts.md)\.

When you enabled Macie Classic, Macie Classic created a service\-linked role\. To learn about the IAM policy for this role, see [Service\-Linked Roles for Amazon Macie Classic](using-service-linked-roles.md)\.

After you enabled Macie Classic, it immediately began pulling and analyzing independent streams of data from AWS CloudTrail to generate alerts\. Because Macie Classic consumes this data only to determine if there are potential security issues, Macie Classic doesn't manage CloudTrail for you or make its events and logs available to you\. If you enabled CloudTrail independently of Macie Classic, you continue to have the option to configure its settings through the CloudTrail console or APIs\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

You can disable Macie Classic at any time to stop it from processing and analyzing CloudTrail events\. For more information, see [Disabling Amazon Macie Classic and Deleting Collected Metadata](macie-disable.md)\.

## Integrate Amazon S3 with Macie Classic<a name="macie-integrates3"></a>

To classify and protect your data, Macie Classic analyzes and processes information from CloudTrail and Amazon S3\. Enabling CloudTrail in your account is required to use Macie Classic\. Integrating S3 with Macie Classic is not required\. However, we strongly recommend that you integrate with Amazon S3 as part of setting up Macie Classic\. For more information about how Macie Classic classifies your data, see [Classifying Data with Amazon Macie Classic](macie-classify-data.md)\.

When you integrate with Amazon S3, Macie Classic creates a trail and a bucket to store the logs about the Amazon S3 object\-level API activity \(data events\) that it will analyze, along with other CloudTrail logs that it processes\.

**Prerequisites**
+ The IAM identity \(user, role, group\) that you use to integrate must have the required permissions\. To grant the required permissions, attach the **AmazonMacieFullAccess** managed policy to this identity\. For more information, see [Predefined AWS Managed Policies for Macie Classic](macie-access-control.md#managed-policies)\.

**To integrate with Amazon S3**

1. Log in to Amazon Web Services \(AWS\) with the credentials of the account that is serving as your Macie Classic administrator account\.

1. Open the Amazon Macie console and choose **Macie Classic** in the navigation pane\.

1. Choose **Integrations** from the navigation pane\.

1. Choose **S3 Resources** and choose **Select** next to the account \(administrator or member\)\.

1. On the **Integrate S3 resources with Macie Classic** page, choose **Add**\. Select up to 250 Amazon S3 resources in the current AWS Region and then choose **Add**\.

1. For **Classification of existing objects**, keep the default setting, **Full**\. The one\-time classification method is applied only once to all of the existing objects in the selected S3 buckets\.

   Macie Classic displays the following information for each selected bucket:
   + **Total objects** – Total number of objects\.
   + **Processed estimate** – Total size of the data that Macie Classic will classify\.
   + **Cost estimate** – Cost estimate for classifying all of the objects\.

   Macie Classic also displays the following totals across all selected buckets:
   + **Total size** – Total size of the data\.
   + **Total number of objects** – Total number of objects\.
   + **Processed estimate** – Total size of the data that Macie Classic will classify\.
   + **Total cost estimate** – Cost estimate for classifying all of the objects\.

   The cost estimate for each bucket is based on its processed estimate value\. The total cost estimates are provided only for S3 buckets, not for prefixes\. For more information, see [Amazon Macie Classic Pricing](http://aws.amazon.com/macie/pricing/)\.

   The one\-time classification cost estimates are only calculated per S3 bucket, not bucket prefixes\. If you select a bucket prefix, the cost estimate for the entire S3 bucket is included in the total cost estimate\. If you select multiple prefixes of the same S3 bucket, the cost estimate for the entire S3 bucket is included only once in the total cost estimate\.

1. When you have finished your selections, choose **Review**\.

1. When you have finished reviewing your selections, choose **Start classification**\.