# email-notify-aws-ec2-instance-state

How can I get customized email notifications when my EC2 instance changes states?
Last updated: 2022-01-10

I want to receive email notifications when my Amazon Elastic Compute Cloud (Amazon EC2) instance changes states. How can I do this?

Short description
To receive email notifications when your EC2 instance changes states:

Create an Amazon Simple Notification Service (Amazon SNS) topic. The SNS topic sends messages to subscribing endpoints or clients.
Create an Amazon EventBridge using the EC2 Instance State-change Notification event type.
Resolution
Create an SNS topic
1.    Open the Amazon SNS console, and then choose Topics from the navigation pane.

2.    Select Create topic.

3.    For Name, enter a name for your topic.

4.    For Display name, enter a display name for your topic.

5.    Select Create topic.

6.    On the Subscriptions tab, choose Create subscription.

7.    For Protocol, choose Email.

8.    For Endpoint, enter the email address where you want to receive the notifications.

9.    Select Create subscription.

A subscription confirmation email is sent to the address you entered. Choose Confirm subscription in the email. Note the SNS topic that you created. You use this topic when creating the EventBridge rule.

Create an EventBridge event
1.    Open the EventBridge console, and then choose Events from the navigation pane.

2.    Select Create rule.

3.    Enter a Name for your rule. You can optionally enter a Description.

4.    In Define pattern, select Event pattern.

5.    For Event matching pattern, select Pre-defined pattern by service.

6.    For Service provider, choose AWS.

7.    For Service name, choose EC2.

8.    For Event type, choose EC2 Instance State-change Notification.

9.    Choose Any state.

10.    Choose Any instance.

11.    In Select targets, choose SNS topic from the Target dropdown list.

12.    For Topic, choose the topic name that you created earlier.

13.    For Configure input, choose Input Transformer.

14.    For Input Path, enter the following:

{"instance-id":"$.detail.instance-id", "state":"$.detail.state", "time":"$.time", "region":"$.region", "account":"$.account"}
15.    For Input Template, enter the following:

"At <time>, the status of your EC2 instance <instance-id> on account <account> in the AWS Region <region> has changed to <state>."
Note: The Input Template also allows custom inputs.

16.    Select Create rule.

You can test the rule by starting or stopping an instance. This rule generates an email notification every time an instance changes to any state, including stopped.
