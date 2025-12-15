# Day-2:Create Security Group

## Objective
Create a security group in the **default VPC** in the **us-east-1** region with specific inbound rules for the Nautilus DevOps team migration task.

---

## Requirements

- **Security Group Name:** datacenter-sg  
- **Description:** Security group for Nautilus App Servers  
- **VPC:** Default VPC  
- **Region:** us-east-1  

### Inbound Rules
| Type | Protocol | Port | Source |
|----|----|----|----|
| HTTP | TCP | 80 | 0.0.0.0/0 |
| SSH | TCP | 22 | 0.0.0.0/0 |

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

## Step 2: Find Default VPC ID
```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```
Save the output VPC ID for the next step.

---

## Step 3: Create the Security Group

Replace <VPC_ID> with the default VPC ID.

``` bash

aws ec2 create-security-group \
--group-name datacenter-sg \
--description "Security group for Nautilus App Servers" \
--vpc-id <VPC_ID>

```
Note the returned **GroupId**.

---

## Step 4: Add Inbound Rule – HTTP (Port 80)
```bash
aws ec2 authorize-security-group-ingress \
--group-id <SECURITY_GROUP_ID> \
--protocol tcp \
--port 80 \
--cidr 0.0.0.0/0
```
---

## Step 5: Add Inbound Rule – SSH (Port 22)
aws ec2 authorize-security-group-ingress \
--group-id <SECURITY_GROUP_ID> \
--protocol tcp \
--port 22 \
--cidr 0.0.0.0/0


| Important: Do not put spaces between -- and the option name.

---

## Step 6: Verify Security Group Rules
```bash
aws ec2 describe-security-groups \
--group-ids <SECURITY_GROUP_ID>
```
---

Expected Result

- Security group datacenter-sg exists in the default VPC
- Description is correctly set
- Inbound rules allow:
  - HTTP (80) from 0.0.0.0/0
  - SSH (22) from 0.0.0.0/0
 
---

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a816c261-7912-407d-9e07-e22c08db651a" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/aaf5d487-9d71-4c04-a269-fd62cbff9aaf" />



