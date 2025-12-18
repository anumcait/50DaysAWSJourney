# Day 10: Attach Elastic IP to EC2 Instance

## Objective
Attach an existing **Elastic IP** to an existing **EC2 instance** as part of the incremental AWS migration process.

---

## Requirements

- **EC2 Instance Name:** datacenter-ec2  
- **Elastic IP Name:** datacenter-ec2-eip  
- **Action:** Attach Elastic IP to EC2 instance  
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

Save the **returned InstanceId**.

---

## Step 3: Get Elastic IP Allocation ID

Retrieve the Allocation ID for the Elastic IP named datacenter-ec2-eip:

aws ec2 describe-addresses \
--filters Name=tag:Name,Values=datacenter-ec2-eip \
--query "Addresses[].AllocationId" \
--output text

Save the returned AllocationId.

---

## Step 4: Attach Elastic IP to EC2 Instance

Associate the Elastic IP with the EC2 instance:
```
aws ec2 associate-address \
--instance-id <INSTANCE_ID> \
--allocation-id <ALLOCATION_ID>
```

Replace:

<INSTANCE_ID> with the EC2 instance ID

<ALLOCATION_ID> with the Elastic IP Allocation ID

---

## Step 5: Verify Elastic IP Attachment

Verify that the Elastic IP is now associated with the instance:
```
aws ec2 describe-addresses \
--allocation-ids <ALLOCATION_ID> \
--query "Addresses[].InstanceId" \
--output text
```

**Expected output:**

**<INSTANCE_ID>**

---

**✅ Result**

- Elastic IP datacenter-ec2-eip is attached to datacenter-ec2
- EC2 instance is reachable via the Elastic IP
- No errors during association
- All resources are in us-east-1

**Notes**

- An Elastic IP can be associated with only one instance at a time
- Ensure the EC2 instance is running for association
- Charges may apply if Elastic IP is not associated

> Attach Elastic IP to EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c2724667-a551-407f-a5d3-e93a3d8fb7ec" />

