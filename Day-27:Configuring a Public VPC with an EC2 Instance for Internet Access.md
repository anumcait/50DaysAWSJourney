# Day 27: Configuring a Public VPC with an EC2 Instance for Internet Access

## Objective
Create a **public VPC** with a **public subnet**, ensure automatic public IP assignment, and launch an **EC2 instance** that is accessible over the internet via **SSH (port 22)**.

---

## Requirements
- **VPC name**: `datacenter-pub-vpc`
- **Subnet name**: `datacenter-pub-subnet`
- **Subnet type**: Public (auto-assign public IPv4 enabled)
- **EC2 instance name**: `datacenter-pub-ec2`
- **Instance type**: `t2.micro`
- **Region**: `us-east-1`
- **SSH access**: Port 22 open to the internet (`0.0.0.0/0`)

---

## Step 1: Sign in to AWS Console

1. Open:
https://672261773768.signin.aws.amazon.com/console?region=us-east-1
2. Log in using the provided credentials.
3. Ensure the region is set to **us-east-1**.

---

## Step 2: Create a Public VPC

1. Navigate to **VPC → Your VPCs**
2. Click **Create VPC**
3. Configure:
- **Name tag**: `datacenter-pub-vpc`
- **IPv4 CIDR block**: `10.0.0.0/16`
- **Tenancy**: Default
4. Click **Create VPC**

---

## Step 3: Create and Attach Internet Gateway

1. Go to **VPC → Internet Gateways**
2. Click **Create internet gateway**
3. Set **Name**: `datacenter-pub-igw`
4. Click **Create**
5. Select the IGW → **Actions → Attach to VPC**
6. Attach to **datacenter-pub-vpc**

---

## Step 4: Create Public Subnet

1. Navigate to **VPC → Subnets**
2. Click **Create subnet**
3. Configure:
- **VPC**: `datacenter-pub-vpc`
- **Subnet name**: `datacenter-pub-subnet`
- **Availability Zone**: us-east-1a
- **IPv4 CIDR block**: `10.0.1.0/24`
4. Click **Create subnet**

---

## Step 5: Enable Auto-Assign Public IP on Subnet

1. Select **datacenter-pub-subnet**
2. Click **Actions → Edit subnet settings**
3. Enable:
- ✅ **Auto-assign public IPv4 address**
4. Click **Save**

---

## Step 6: Create Route Table for Public Access

1. Go to **VPC → Route Tables**
2. Click **Create route table**
3. Configure:
- **Name**: `datacenter-pub-rt`
- **VPC**: `datacenter-pub-vpc`
4. Click **Create route table**

### Add Internet Route
1. Select the route table → **Routes → Edit routes**
2. Add route:
- **Destination**: `0.0.0.0/0`
- **Target**: Internet Gateway (`datacenter-pub-igw`)
3. Save routes

### Associate Subnet
1. Go to **Subnet associations**
2. Click **Edit subnet associations**
3. Select **datacenter-pub-subnet**
4. Save

---

## Step 7: Create Security Group for SSH Access

1. Go to **EC2 → Security Groups**
2. Click **Create security group**
3. Configure:
- **Name**: `datacenter-pub-sg`
- **Description**: Allow SSH access
- **VPC**: `datacenter-pub-vpc`
4. **Inbound rules**:
- Type: SSH
- Protocol: TCP
- Port: 22
- Source: `0.0.0.0/0`
5. Outbound rules: Default (allow all)
6. Click **Create security group**

---

## Step 8: Launch EC2 Instance in Public Subnet

1. Navigate to **EC2 → Instances**
2. Click **Launch instance**
3. Configure:
- **Name**: `datacenter-pub-ec2`
- **AMI**: Amazon Linux 2 (or Ubuntu)
- **Instance type**: `t2.micro`
- **Key pair**: Select existing or create new (required for SSH)
4. **Network settings**:
- **VPC**: `datacenter-pub-vpc`
- **Subnet**: `datacenter-pub-subnet`
- **Auto-assign public IP**: Enable
- **Security group**: `datacenter-pub-sg`
5. Click **Launch instance**

---

## Step 9: Verify Public Access

1. Wait for instance state to be **Running**
2. Confirm **Public IPv4 address** is assigned
3. Test SSH access:
```bash
ssh -i <key.pem> ec2-user@<PUBLIC-IP>
```
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0cc29091-678e-44a0-b08b-aeca9d5b936f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f5ffdadb-bc43-4f5e-b566-dbba218af22a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6192e71e-9f29-46d7-89fd-66689c51b57d" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/7bf8152e-e5d8-40cb-b68a-e05e32d52f32" />




