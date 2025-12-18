# Day 11: Attach Elastic Network Interface to EC2 Instance

## Objective
Attach an existing **Elastic Network Interface (ENI)** to an existing **EC2 instance** after ensuring the instance initialization is complete and the ENI status becomes **attached**.

---

## Requirements

- **EC2 Instance Name:** devops-ec2  
- **Elastic Network Interface:** devops-eni  
- **Final ENI Status:** attached  
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

Retrieve the Instance ID for devops-ec2:
```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=devops-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Save the returned InstanceId.

## Step 3: Ensure Instance Initialization Is Complete

Check instance status checks:
```
aws ec2 describe-instance-status \
--instance-ids <INSTANCE_ID>
```

Proceed only if:

- InstanceStatus.Status = ok
- SystemStatus.Status = ok

---

## Step 4: Get Elastic Network Interface ID

Retrieve the Network Interface ID for devops-eni:
```
aws ec2 describe-network-interfaces \
--filters Name=tag:Name,Values=devops-eni \
--query "NetworkInterfaces[].NetworkInterfaceId" \
--output text
```

Save the returned NetworkInterfaceId.

---

## Step 5: Attach ENI to EC2 Instance

Attach the ENI to the EC2 instance (using device index 1):
```
aws ec2 attach-network-interface \
--network-interface-id <NETWORK_INTERFACE_ID> \
--instance-id <INSTANCE_ID> \
--device-index 1
```

Save the returned AttachmentId.

---

## Step 6: Verify ENI Attachment Status

Verify that the ENI status is attached:
```
aws ec2 describe-network-interfaces \
--network-interface-ids <NETWORK_INTERFACE_ID> \
--query "NetworkInterfaces[].Status" \
--output text
```

**Expected output:**

in-use

This confirms the ENI is successfully attached.

**✅ Result**

- Elastic Network Interface devops-eni is attached to devops-ec2
- ENI status shows attached / in-use
- EC2 instance is fully initialized and running
- All actions performed in us-east-1

**Notes**

- Do not attach ENI before instance initialization completes
- device-index 0 is reserved for the primary network interface
- Use device-index 1 or higher for secondary ENIs

> Elastic Network Interface devops-eni successfully attached to devops-ec2 ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/df42c8e7-9b82-4853-8823-3b51415ed7e4" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/944b9d05-9766-406c-8c89-ec54edac5ac2" />



