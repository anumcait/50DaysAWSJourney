# Day 4: Allocate Elastic IP

## Objective
Allocate an **Elastic IP (EIP)** in AWS for the Nautilus DevOps team migration task, and name it **xfusion-eip**.

---

## Requirements

- Allocate an **Elastic IP**  
- **Name Tag:** xfusion-eip  
- **Region:** us-east-1  
- **Domain:** vpc (for default VPC compatibility)  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

---

## Step 2: Allocate Elastic IP

Run the following command:
```bash
aws ec2 allocate-address \
--domain vpc
```

**Sample output:**

{
    "PublicIp": "18.218.45.123",
    "AllocationId": "eipalloc-0abc123def456ghi7",
    "Domain": "vpc"
}

> Save the AllocationId (e.g., eipalloc-0abc123def456ghi7) for tagging and later use.

---

## Step 3: Tag the Elastic IP

Apply a Name tag to the Elastic IP:

```bash
aws ec2 create-tags \
--resources eipalloc-0abc123def456ghi7 \
--tags Key=Name,Value=xfusion-eip
```

> Replace eipalloc-0abc123def456ghi7 with your actual AllocationId.

---

## Step 4: Verify Elastic IP and Tag
aws ec2 describe-addresses \
--allocation-ids eipalloc-0abc123def456ghi7

**Expected output:**
```
{
    "Addresses": [
        {
            "PublicIp": "18.218.45.123",
            "AllocationId": "eipalloc-0abc123def456ghi7",
            "Domain": "vpc",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "xfusion-eip"
                }
            ]
        }
    ]
}
```

**✅ Expected Result**

- Elastic IP allocated in us-east-1
- Domain is vpc
- Tag Name=xfusion-eip is applied
- Ready to be associated with EC2 instances or NAT gateways

**Notes**

- Elastic IPs are region-specific
- Allocated EIPs incur charges only when not associated with a running instance
- Always save the AllocationId for future use

## Screen shots:

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/512b3cec-c8ee-45af-9a5f-004e463b2a29" />

