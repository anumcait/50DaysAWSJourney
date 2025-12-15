# Day 3: Create Subnet

## Objective
Create a **subnet** for the Nautilus DevOps team as part of their incremental AWS migration strategy.

---

## Requirements

- **Subnet Name:** nautilus-subnet  
- **VPC:** Default VPC  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

## Step 2: Get Default VPC ID
```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```
Save the returned **VPC ID.**

---

## Step 3: List Existing Subnets

Check which CIDRs are already in use in the default VPC:
```bash
aws ec2 describe-subnets \
--filters Name=vpc-id,Values=<VPC_ID> \
--query "Subnets[].CidrBlock" \
--output table
```

Example output:

```diff
--------------------
|  DescribeSubnets |
+------------------+
|  172.31.0.0/20   |
|  172.31.16.0/20  |
|  172.31.32.0/20  |
|  172.31.48.0/20  |
|  172.31.64.0/20  |
|  172.31.80.0/20  |
+------------------+
```

---

## Step 4: Pick an Available CIDR Block

From the above, choose a CIDR that is not listed, for example:

- 172.31.96.0/20
- 172.31.112.0/20
- 172.31.128.0/20

---

## Step 5: Create the Subnet

Replace <VPC_ID> and <AZ> accordingly. Example:
```bash
aws ec2 create-subnet \
--vpc-id <VPC_ID> \
--cidr-block 172.31.96.0/20 \
--availability-zone us-east-1a
```

Save the returned **SubnetId.**

---

## Step 6: Tag the Subnet
```bash
aws ec2 create-tags \
--resources <SUBNET_ID> \
--tags Key=Name,Value=nautilus-subnet
```

Important: Do not put spaces after the comma in Key=Name,Value=nautilus-subnet

---

## Step 7: Verify Subnet
```bash
aws ec2 describe-subnets \
--subnet-ids <SUBNET_ID> \
--query "Subnets[0].Tags"
```

**Expected output:**
```
[
  {
    "Key": "Name",
    "Value": "nautilus-subnet"
  }
]
```

---

**Result**

- Subnet nautilus-subnet exists
- Subnet is created under default VPC
- Subnet is in us-east-1 region
- CIDR block does not conflict with existing subnets
- Tag Name=nautilus-subnet is applied

---

**Notes**

- All resources must be created in us-east-1
- Check existing subnets before picking a CIDR to avoid InvalidSubnet.Conflict
- Only unused CIDRs within 172.31.0.0/16 can be used

---

## Screenshots

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/bf8af275-56ca-4d5d-b4fb-b8dbf5cb56d0" />
