# Day 14: Terminate EC2 Instance

## Objective
Terminate an obsolete **EC2 instance** and ensure it reaches the **terminated** state before task submission.

---

## Requirements

- **EC2 Instance Name:** datacenter-ec2  
- **Action:** Terminate EC2 instance  
- **Final State:** terminated  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

(Optional) Verify AWS credentials:

```bash
showcreds
```
Set the correct AWS region:
```
export AWS_DEFAULT_REGION=us-east-1
```

---

## Step 2: Get EC2 Instance ID

Retrieve the Instance ID for datacenter-ec2:

```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=datacenter-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Save the returned InstanceId.

## Step 3: Disable Termination Protection (If Enabled)

If termination protection was enabled earlier, disable it first:
```
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--no-disable-api-termination
```

Skip this step if termination protection is already disabled.

---

## Step 4: Terminate the EC2 Instance

Terminate the instance:
```
aws ec2 terminate-instances \
--instance-ids <INSTANCE_ID>
```
---

## Step 5: Wait for Instance to Terminate

Wait until the instance reaches the terminated state:
```
aws ec2 wait instance-terminated \
--instance-ids <INSTANCE_ID>
```
---

## Step 6: Verify Instance State

Confirm the instance state:
```
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[].Instances[].State.Name" \
--output text
```

**Expected output:**
> terminated

---

**✅ Result**

- EC2 instance datacenter-ec2 is successfully terminated
- Instance state shows terminated
- All actions performed in us-east-1

---

**Notes**

- Terminated instances cannot be recovered
- Ensure the correct instance is selected before termination
- Associated EBS volumes may persist depending on delete-on-termination settings

> Terminate EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d4597c67-e991-48d0-a378-274114f0e8ca" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5fa81831-3020-43d8-a869-95d4700994cc" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6cce18ca-b014-4c15-95b9-cac077a5aebd" />

---





