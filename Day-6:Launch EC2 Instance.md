# Day 6: Launch EC2 Instance

## Objective
Launch an **EC2 instance** (xfusion) for the Nautilus DevOps team as part of their incremental AWS cloud migration.

---

## Requirements

- **Instance Name:** xfusion-ec2  
- **AMI:** Amazon Linux  
- **Instance Type:** t2.micro  
- **Key Pair:** xfusion-kp (RSA)  
- **Security Group:** Default security group  
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

## Step 2: Create RSA Key Pair

Create a new RSA key pair and save the private key locally:
```
aws ec2 create-key-pair \
--key-name xfusion-kp \
--key-type rsa \
--query "KeyMaterial" \
--output text > xfusion-kp.pem
```
---

## Step 3: Get Amazon Linux AMI ID

Fetch the latest Amazon Linux AMI ID for us-east-1:
```
aws ec2 describe-images \
--owners amazon \
--filters "Name=name,Values=al2023-ami-*" "Name=architecture,Values=x86_64" \
--query "Images | sort_by(@, &CreationDate)[-1].ImageId" \
--output text
```

Save the returned **AMI ID**.

---

## Step 4: Get Default Security Group ID

```
aws ec2 describe-security-groups \
--filters Name=group-name,Values=default \
--query "SecurityGroups[0].GroupId" \
--output text
```

Save the **Security Group ID**.

---

## Step 5: Launch the EC2 Instance

Replace <AMI_ID> and <SECURITY_GROUP_ID> with values from previous steps.
```
aws ec2 run-instances \
--image-id <AMI_ID> \
--instance-type t2.micro \
--key-name xfusion-kp \
--security-group-ids <SECURITY_GROUP_ID> \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=xfusion-ec2}]'
```

Save the returned **InstanceId**.

---

Step 6: Verify EC2 Instance
aws ec2 describe-instances \
--filters Name=tag:Name,Values=xfusion-ec2 \
--query "Reservations[].Instances[].State.Name" \
--output text


**Expected output:**

running

---


**✅ Result**

- EC2 instance xfusion-ec2 is running
- Instance type is t2.micro
- Amazon Linux AMI is used
- Key pair xfusion-kp exists
- Default security group is attached
- Instance is launched in us-east-1

---

**Notes**

- The .pem file cannot be downloaded again after creation
- Ensure there are no spaces in CLI flags
- If you encounter a Blocked error, restart the lab and refresh credentials

---

> EC2 instance xfusion-ec2 launched successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/3f1a19fa-2f53-4669-b051-ed95a7c17ac3" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/fcbe3fc5-dd6e-4979-966c-a122ea0cd894" />

---





