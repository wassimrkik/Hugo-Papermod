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
![Data Source 2](/datasource2.png)
- At the bottom of the page, click on Save & test, you should see a displayed message saying **1. Successfully queried the CloudWatch metrics API. 2. Successfully queried the CloudWatch logs API.**

### 3. Configure Grafana Alert
- Go to **contact points** under **Alerting** then click on *Add contact point*.
- Enter a name for your contact point.
- Choose **AWS SNS** in Integration menu.
- In The Amazon SNS API URL, enter this : https://sns.<SNS-TOPIC-REGION>.amazonaws.com
- In the SNS Topic ARN, enter the ARN of the SNS topic that you created in step **1**.
- Click **Save contact point**
- You can test the integration by clicking on **Test** on the top right of the screen

### 4. Test Your Alert
To ensure everything is configured correctly, you can test the alert by setting a condition that will trigger the alert, such as lowering the threshold for a few minutes and verifying that the SNS notification is sent.

