---
title: "Grafana Alert with SNS"
date: 2024-09-23
---

To configure **Grafana Alerts** with **Amazon SNS**, follow the steps below.

### 1. Create an SNS Topic
- Go to the [AWS SNS Dashboard](https://console.aws.amazon.com/sns/home).
- Click **Create topic**.
- Choose the **Standard** topic type and enter a name for the topic.
- After the topic is created, click **Create subscription**.
- Select a protocol (e.g., **Email** or **SMS**) and provide the corresponding endpoint (email address or phone number).
- Confirm the subscription through the email/SMS sent by AWS.

### 2. Set Up AWS Credentials in Grafana
- In the [AWS IAM Console](https://console.aws.amazon.com/iam/), create an IAM user with a custom policy that grants **sns:Publish** permissions to your SNS topic.
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "<SNS-TOPIC-ARN>"
        }
    ]
}
```
- After creating the user, download the **Access Key ID** and **Secret Access Key**.

In Grafana:
- Go to **Configuration** → **Data Sources**.
- Add a **CloudWatch** data source.
![Data Source](/datasource.png)
- Enter the **Access Key ID**, **Secret Access Key**, and **AWS Region**.

### 3. Create a Notification Channel in Grafana
- In Grafana, go to **Alerting** → **Notification Channels**.
- Click **Add Channel** and select **Amazon SNS**.
- Fill in the following fields:
    - **SNS Topic ARN**: The ARN of the SNS topic.
    - **AWS Access Key** and **AWS Secret Key** from the IAM user.
    - **AWS Region**: The region where your SNS topic is located.
- Click **Save**.

### 4. Configure Grafana Alert
- Open the Grafana **dashboard** or **panel** you want to add an alert to.
- Go to the **Alert** tab and click **Create Alert**.
- Set the conditions for the alert (e.g., when a metric crosses a threshold).
- Under **Notifications**, select the **Notification Channel** you created earlier.
- Save the dashboard.

### 5. Test Your Alert
To ensure everything is configured correctly, you can test the alert by setting a condition that will trigger the alert, such as lowering the threshold for a few minutes and verifying that the SNS notification is sent.

