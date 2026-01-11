# Day 26: Configuring an EC2 Instance as a Web Server with Nginx

## Objective
Launch an EC2 instance using an Ubuntu AMI and configure it as a web server by installing and starting **Nginx** through a **user data script**. Ensure the instance is publicly accessible over HTTP.

---

## Requirements
- **Instance Name**: `devops-ec2`
- **AMI**: Ubuntu (any available version)
- **Region**: us-east-1
- **Web Server**: Nginx
- **Port Access**: HTTP (80) open to the internet
- **User Data Script**:
  - Install Nginx
  - Start Nginx service

---

## Step 1: Sign in to AWS Console

1. Open the AWS Console:
https://841089144903.signin.aws.amazon.com/console?region=us-east-1
2. Log in using the provided credentials.
3. Verify the region is set to **us-east-1**.

---

## Step 2: Launch EC2 Instance

1. Navigate to **EC2 → Instances**
2. Click **Launch instance**
3. Configure the instance details:

### Basic Settings
- **Name**: `devops-ec2`
- **AMI**: Ubuntu Server (e.g., Ubuntu Server 22.04 LTS)
- **Instance type**: t2.micro (or any allowed type)

### Key Pair
- Proceed without a key pair (unless SSH access is required)

---

## Step 3: Configure Network and Security Group

1. Under **Network settings**:
- VPC: Default VPC
- Subnet: Auto-assign
2. Create or select a security group:
- **Security group name**: `devops-web-sg`
- **Inbound rule**:
  - Type: HTTP
  - Protocol: TCP
  - Port range: 80
  - Source: Anywhere (`0.0.0.0/0`)
- Outbound rules: Default (allow all)

---

## Step 4: Add User Data Script

Scroll to **Advanced details → User data** and paste:

```bash
#!/bin/bash
apt-get update -y
apt-get install -y nginx
systemctl start nginx
systemctl enable nginx
```
---

## Step 5: Launch the Instance

1. Review configuration
2. Click Launch instance
3. Wait until instance state shows Running

---

## Step 6: Verify Nginx Installation

1. Go to EC2 → Instances
2. Select devops-ec2
3. Copy the Public IPv4 address
4. Open a browser and navigate to:
```
http://<PUBLIC-IP>
```
5. You should see the Nginx welcome page

---

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1581d5cb-b980-497a-b56f-050665fc29bb" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/6d573f35-5cc3-498b-ac65-292cab1746e4" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/178b8364-4d82-4b25-809e-5d4dcffe327e" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/12adc414-02b1-412d-a4ce-41b6ddcdd136" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a9b4fe42-58bf-481f-809e-bc98cc1c41e5" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ef1335e0-c0e7-43a4-9f54-cad636064002" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/08aaf7f9-945c-40c6-b4b4-f41fe250214a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c9b96343-b0c7-4a76-bfc2-bf56a68f7dfe" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1e48d710-d409-4c47-a72e-28add4784aae" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8d8d1f74-7728-414f-abf6-20c15aaf7ef6" />










