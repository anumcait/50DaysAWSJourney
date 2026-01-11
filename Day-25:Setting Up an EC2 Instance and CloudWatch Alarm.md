# Day 25: Setting Up an EC2 Instance and CloudWatch Alarm

## Objective
Launch an EC2 instance using an Ubuntu AMI and configure a CloudWatch alarm to monitor CPU utilization. The alarm should trigger when CPU usage reaches or exceeds **90% for one consecutive 5-minute period** and send notifications using an existing SNS topic.

---

## Requirements
- EC2 instance name: **devops-ec2**
- AMI: **Ubuntu (any supported version)**
- Region: **us-east-1**
- CloudWatch alarm name: **devops-alarm**
- Metric: **CPU Utilization**
- Statistic: **Average**
- Threshold: **>= 90%**
- Period: **5 minutes**
- Evaluation periods: **1**
- SNS Topic: **devops-sns-topic** (already created)

---

## Step 1: Sign in to AWS Console

1. Open the AWS Console:
https://141563439981.signin.aws.amazon.com/console?region=us-east-1
2. Log in using the provided credentials.
3. Ensure the region is set to **us-east-1**.

---

## Step 2: Launch EC2 Instance (Ubuntu)

1. Navigate to **EC2 → Instances**
2. Click **Launch instance**
3. Configure the instance:
- **Name**: `devops-ec2`
- **AMI**: Ubuntu Server (e.g., Ubuntu Server 22.04 LTS)
- **Instance type**: t2.micro (or any allowed type)
- **Key pair**: Proceed without a key pair (unless required)
- **Network**: Default VPC
- **Security group**: Default (or allow SSH if needed)
4. Click **Launch instance**
5. Wait until the instance state becomes **Running**

---

## Step 3: Verify CPU Metrics Availability

1. Go to **CloudWatch → Metrics**
2. Select:
EC2 → Per-Instance Metrics
3. Locate the instance **devops-ec2**
4. Confirm the **CPUUtilization** metric is available

---

## Step 4: Create CloudWatch Alarm

1. Navigate to **CloudWatch → Alarms**
2. Click **Create alarm**
3. Click **Select metric**
4. Choose:
EC2 → Per-Instance Metrics → CPUUtilization
5. Select the **devops-ec2** instance
6. Click **Select metric**

---

## Step 5: Configure Alarm Conditions

- **Statistic**: Average
- **Period**: 5 minutes
- **Threshold type**: Static
- **Condition**: Greater than or equal to
- **Threshold value**: 90
- **Evaluation periods**: 1
- **Datapoints to alarm**: 1

---

## Step 6: Configure Alarm Actions

1. Under **Notification**:
- Select **In alarm**
- Choose **Select an existing SNS topic**
- Topic name: `devops-sns-topic`
2. Leave OK and Insufficient Data actions as default
3. Click **Next**

---

## Step 7: Name and Create the Alarm

- **Alarm name**: `devops-alarm`
- **Description**: Alarm for high CPU utilization on devops-ec2
- Click **Create alarm**

---

## Step 8: Verify Alarm Status

- Alarm state should initially show **OK**
- Alarm will move to **ALARM** when CPU utilization exceeds 90% for 5 minutes

---

## Result
The EC2 instance has been successfully deployed, and a CloudWatch alarm is actively monitoring CPU utilization and will notify the DevOps team via SNS if usage exceeds the defined threshold.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/81e30b4f-bd12-4c74-b34b-aa39d6d9a1f1" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d875b3dd-4170-4994-af78-b02727cdfd18" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/86b80613-373e-442e-a9ae-a5e9425d74af" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/53bfd933-11a7-4dd4-a322-fd92b415202a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/96b07243-4d95-49c2-8e2b-1ea747a937ea" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/963a64ef-f01a-43c1-858b-11aa8a477995" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1e50cba2-4f1a-4ae4-99f2-452308de79fd" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/3f06c003-b423-408c-941e-19288ababdff" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2fb5b849-cecb-4cb1-a45c-a572f1c5b12f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6da7fbc8-d8be-46f4-829d-4550eabe3112" />











