# Day 13: Create AMI from EC2 Instance

## Objective
Create an **Amazon Machine Image (AMI)** from an existing EC2 instance to support backup, scaling, and migration activities.

---

## Requirements

- **Source EC2 Instance Name:** nautilus-ec2  
- **AMI Name:** nautilus-ec2-ami  
- **AMI State:** available  
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

Retrieve the Instance ID for the EC2 instance named nautilus-ec2:
```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=nautilus-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Save the returned **InstanceId**.

---

## Step 3: Create AMI from EC2 Instance

Create an AMI named nautilus-ec2-ami from the instance:
```
aws ec2 create-image \
--instance-id <INSTANCE_ID> \
--name nautilus-ec2-ami \
--description "AMI created from nautilus-ec2 instance" \
--no-reboot
```

Save the returned ImageId.

> --no-reboot ensures the instance is not restarted during AMI creation.

## Step 4: Wait for AMI to Become Available

Check the AMI status:
```
aws ec2 describe-images \
--image-ids <IMAGE_ID> \
--query "Images[].State" \
--output text
```

Wait until the output shows:
```
available
```

(Optional) You can also wait automatically:
```
aws ec2 wait image-available \
--image-ids <IMAGE_ID>
```
---

**✅ Result**

- AMI nautilus-ec2-ami is successfully created
- AMI state is available
- Source instance nautilus-ec2 remains intact
- All actions performed in us-east-1

---

**Notes**

- AMI creation may take a few minutes depending on instance size
- You can use this AMI to launch identical EC2 instances
- Ensure AMI state is available before submitting the task

---

> Create AMI from EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0908c27d-3a47-4b80-8aa4-77f114f2acac" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e19f2cb2-9429-4a4c-9698-fdd175cbcd0f" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a321f89a-2ed7-479e-9e54-9c36b0aa2035" />

---





