# Day 8: Enable Stop Protection for EC2 Instance

## Objective
Enable **Stop Protection** for an existing EC2 instance to prevent it from being accidentally stopped during ongoing migration activities.

---

## Requirements

- **Instance Name:** xfusion-ec2  
- **Action:** Enable Stop Protection  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

(Optional) Verify AWS credentials:

```bash
showcreds
```

Set the correct region:

```
export AWS_DEFAULT_REGION=us-east-1
```
---

## Step 2: Get the Instance ID

Retrieve the Instance ID for the EC2 instance named xfusion-ec2:
```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=xfusion-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Save the returned **InstanceId**.

---

## Step 3: Enable Stop Protection

Run the following command to enable stop protection:
```
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--disable-api-stop
```

Replace <INSTANCE_ID>

---

## Step 4: Verify Stop Protection Status
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[].Instances[].DisableApiStop.Value" \
--output text


**Expected output:**

True


This confirms that stop protection is enabled.

**✅ Result**

- EC2 instance xfusion-ec2 has stop protection enabled
- Instance remains unaffected and running
- Protection prevents accidental stop operations

**Notes**

- Stop protection prevents API-based stop actions only
- The instance can still be terminated unless termination protection is enabled
- All actions must be performed in us-east-1

---
> Enable Stop Protection for EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/9be74f22-1d07-4981-851c-c923af7117c1" />
