# Day 24: Setting Up an Application Load Balancer for an EC2 Instance

## Objective
Configure an **Application Load Balancer (ALB)** using the **AWS Management Console** to route HTTP traffic on port 80 to an EC2 instance running Nginx.

---

## Requirements
- ALB name: **xfusion-alb**
- Target group name: **xfusion-tg**
- Security group name: **xfusion-sg**
- Listener: **HTTP : 80**
- Target port: **80**
- Region: **us-east-1**
- Existing EC2 instance: **xfusion-ec2**

---

## Step 1: Sign in to AWS Console

1. Open the AWS Console URL: https://715121938043.signin.aws.amazon.com/console?region=us-east-1
2. Log in using the provided username and password.
3. Confirm the region is set to **us-east-1** (top-right corner).

---

## Step 2: Create Security Group for ALB

1. Go to **EC2 → Security Groups**
2. Click **Create security group**
3. Configure:
- **Security group name**: `xfusion-sg`
- **Description**: Allow HTTP access to ALB
- **VPC**: Default VPC
4. **Inbound rules**:
- Type: **HTTP**
- Port: **80**
- Source: **Anywhere (0.0.0.0/0)**
5. Leave outbound rules as default.
6. Click **Create security group**

---

## Step 3: Create Target Group

1. Navigate to **EC2 → Target Groups**
2. Click **Create target group**
3. Choose **Instances**
4. Configure:
- **Target group name**: `xfusion-tg`
- **Protocol**: HTTP
- **Port**: 80
- **VPC**: Default VPC
- **Health check protocol**: HTTP
- **Health check path**: `/`
5. Click **Next**
6. Select instance **xfusion-ec2**
7. Click **Include as pending below**
8. Click **Create target group**

---

## Step 4: Create Application Load Balancer

1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Select **Application Load Balancer**
4. Click **Create**

### Basic Configuration
- **Load balancer name**: `xfusion-alb`
- **Scheme**: Internet-facing
- **IP address type**: IPv4

### Network Mapping
- **VPC**: Default VPC
- **Availability Zones**: Select at least **two subnets**

### Security Groups
- Remove default SG
- Attach **xfusion-sg**

### Listeners and Routing
- Listener: **HTTP : 80**
- Default action: **Forward to xfusion-tg**

5. Click **Create load balancer**

---

## Step 5: Update EC2 Security Group (If Required)

1. Go to **EC2 → Instances**
2. Select **xfusion-ec2**
3. Click **Security** tab
4. Open the attached security group
5. Edit **Inbound rules**
6. Add rule:
- Type: **HTTP**
- Port: **80**
- Source: **Security group → xfusion-sg**
7. Save rules

---

## Step 6: Verify ALB Configuration

1. Go to **EC2 → Load Balancers**
2. Select **xfusion-alb**
3. Copy the **DNS name**
4. Open browser and access:

http://<ALB-DNS-NAME>

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/928ba2f7-fbe3-4fe7-8afc-6050f7b74867" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0f4024a5-d934-43f0-964d-9cb1f5c5f67c" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/85a3acd0-1125-4790-ba7e-4eea45dd4984" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/88b2d19b-6455-4741-8377-b82bac86c27f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f29758f6-f424-4639-b861-ed0233603949" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/063166e3-ec16-4561-841d-801fee654cba" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d0fe9549-8990-449d-a8fd-423aaf47c553" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/20963f95-181a-445f-a69e-37b72e484d2f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2ea585b9-6c44-45d0-9ea6-ee80aeda32ea" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/24bcd22f-3aee-4956-8cc7-29c5a8309bd9" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/71d34853-feec-48f0-80d9-b7122381c900" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5bfec4f3-7fa5-4434-93ec-fb077f0e9d35" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a0af06b2-f21b-42cd-9d15-88281f5b310a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d1fe2364-a9bd-4a02-adef-fde296240bfb" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f11eb262-2254-43b5-b51c-b572d26417cf" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/490c9526-b54c-4f5a-933e-6c4627a1c077" />
















