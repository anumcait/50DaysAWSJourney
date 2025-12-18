# Day 9: Enable Termination Protection for EC2 Instance

## Objective
Enable **termination protection** for an existing EC2 instance to prevent accidental deletion during the migration process.

---

## Requirements

- **Instance Name:** datacenter-ec2  
- **Action:** Enable termination protection  
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

Retrieve the Instance ID for the EC2 instance named datacenter-ec2:
```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=datacenter-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```
Save the returned **InstanceId**.

---

## Step 3: Enable Termination Protection

Run the following command to enable termination protection:
```
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--disable-api-termination
```

> Replace <INSTANCE_ID> with the value from **Step 2**.

## Step 4: Verify Termination Protection Status
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[].Instances[].DisableApiTermination.Value" \
--output text

**Expected output:**
True

This confirms that termination protection is enabled.

---

**✅ Result**

- EC2 instance datacenter-ec2 has termination protection enabled
- Instance cannot be terminated using API/CLI
- Instance remains unaffected and running
- All actions performed in us-east-1

**Notes**

- Termination protection only blocks terminate actions, not stop or reboot
- Instance must be unprotected before termination is possible
- Always verify protection after enabling

---

> Enable Termination Protection for EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/caf2f197-b0c1-438b-a184-50319258ab09" />

