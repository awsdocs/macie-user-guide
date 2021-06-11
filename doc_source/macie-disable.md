--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Disabling Amazon Macie Classic and Deleting Collected Metadata<a name="macie-disable"></a>

Before you disable your Amazon Macie Classic account, you can optionally export your existing data classification results to an S3 bucket\. If you're moving to the new Amazon Macie, we recommend that you do this before you disable Macie Classic and move to the new Amazon Macie\. To learn how, see [Moving to the New Amazon Macie](macie-migration.md)\. 

This topic explains how to disable Macie Classic without exporting your data classification results\. 

**Important**  
Only a Macie Classic administrator account can disable Macie Classic\. To disable Macie Classic for a member account, the administrator account must first disassociate the member account\.

If you disable Macie Classic, it no longer has access to resources in the administrator account and all member accounts\. In addition, you cannot re\-enable Macie Classic\. To start using Amazon Macie again after you disable your Macie Classic account, enable the new Amazon Macie for your account\. For information about how to do this, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\.

If you disable Macie Classic, it stops processing resources in the administrator account and all member accounts\. After Macie Classic is disabled, the metadata that Macie Classic collected while monitoring the data in your administrator and member accounts is deleted\. Within 90 days of disabling Macie Classic, all of this metadata is expired and removed from Macie Classic system backups\. 

**Important**  
Disabling Macie Classic doesn't prompt deletion of your data in your AWS services\. After Macie Classic is disabled, only the metadata that was collected by Macie Classic while it monitored your accounts is deleted\.

1. Sign in to the AWS Management Console and open the Amazon Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/)\.

1. By using the AWS Region selector in the upper\-right corner of the page, switch to the Region in which you want to disable your Macie Classic account—**US East \(N\. Virginia\)** or **US West \(Oregon\)**\.

1. In the navigation pane, choose **Macie Classic**\.

   If you don't see this link, choose your user name in the upper\-right corner of the page, and then sign in to the Amazon Web Services account and role that you want to disable Macie Classic for\.

1. In Macie Classic, navigate to the **general settings** page by choosing the down arrow next to your user name\.

1. On the **general settings** page, review and select the following check boxes:
   + **I understand that if I disable Macie, the service will no longer have access to the resources in the administrator account and all member accounts\. You must add member accounts again if you decide to re\-enable Macie\.**
   +  **I understand that if I disable Macie, the service will stop processing the resources in the administrator account and all member accounts\. All metadata that Macie collected while monitoring the data in these accounts will be deleted\. **

1. Choose **Disable Amazon Macie**\.