--------

This is the user guide for Amazon Macie Classic\. For information about the new Amazon Macie, see the [Amazon Macie User Guide](https://docs.aws.amazon.com/macie/latest/user/)\. To access the Macie Classic console, open the Macie console at [https://console\.aws\.amazon\.com/macie/](https://console.aws.amazon.com/macie/), and then choose **Macie Classic** in the navigation pane\.

--------

# Monitoring Amazon Macie Classic Alerts with Amazon CloudWatch Events<a name="macie-cloudwatch"></a>

Amazon Macie Classic sends notifications based on CloudWatch Events when any change in Macie Classic alerts takes place\. This includes newly generated alerts and updates to existing alerts\. Notifications are sent for all Macie Classic alert types, including predictive alerts and basic alerts, both managed and custom\. For more information about alert types, see [Amazon Macie Classic Alerts](macie-alerts.md)\.

Macie Classic sends notifications based on CloudWatch Events for the alerts generated in both Macie Classic administrator and member accounts\. However, only the Macie Classic administrator account has access to the generated events in CloudWatch Events\. For more information about Macie Classic administrator and member accounts, see [Concepts and Terminology](macie-concepts.md)\.

## Event Format<a name="macie-cloudwatch-event-format"></a>

The [event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) for Macie Classic in CloudWatch Events has the following format\. The fictional account ID `111122223333` represents the ID of the Macie Classic administrator account\.

```
{
  "version": "0",
  "id": "CWE-event-id",
  "detail-type": "Macie Alert",
  "source": "aws.macie",
  "account": "111122223333",
  "time": "2017-04-24T22:28:49Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id/alert/alert_id",
    "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id"
  ],
  "detail": {
    "notification-type": "ALERT_CREATED",
    "name": "Scanning bucket policies",
    "tags": [
      "Custom_Alert",
      "Insider"
    ],
    "url": "https://lb00.us-east-1.macie.aws.amazon.com/111122223333/posts/alert_id",
    "alert-arn": "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id/alert/alert_id",
    "risk-score": 8,
    "trigger": {
      "rule-arn": "arn:aws:macie:us-east-1:111122223333:trigger/trigger_id",
      "alert-type": "basic",
      "created-at": "2017-01-02 19:54:00.644000",
      "description": "Alerting on failed enumeration of large number of bucket policies",
      "risk": 8
    },
    "created-at": "2017-04-18T00:21:12.059000",
    "actor": "555566667777:assumed-role:superawesome:aroaidpldc7nsesfnheji",
    "summary": {ALERT_DETAILS_JSON}
  }
}
```

## Configure CloudWatch Events<a name="macie-configure-cloudwatch-events"></a>

Complete the following procedure to configure your Macie Classic administrator account to receive events in CloudWatch Events from Macie Classic and pipe those events into an Amazon Simple Queue Service \(Amazon SQS\) queue\.

**Prerequisite**  
Create an Amazon SQS queue for the events from Macie Classic\. For more information, see [Tutorial: Creating an Amazon SQS Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html)\.

**To configure CloudWatch events for your Macie Classic administrator account**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**, **Rules** and then choose **Create rule**\.

1. Choose **Edit** and enter the following event pattern for the Macie Classic events\.

   ```
   {
      "source": [
          "aws.macie"
      ]
   }
   ```

1. In the **Targets** pane, choose **Add target**, select **SQS queue** in the target dropdown, and specify your queue for the events from Macie Classic\.