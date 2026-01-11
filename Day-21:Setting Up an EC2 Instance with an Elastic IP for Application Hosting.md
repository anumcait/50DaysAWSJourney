# Day 21: Setting Up an EC2 Instance with an Elastic IP for Application Hosting

## Objective
Launch an EC2 instance, allocate and associate an Elastic IP, and host an application using a static public IP.

---

## Step 1: Launch an EC2 Instance

1. Navigate to **EC2 → Instances → Launch instance**
2. Configure the instance:
   - **AMI**: Amazon Linux 2
   - **Instance Type**: t2.micro
   - **Key Pair**: Select or create one
   - **Network**: Default VPC
   - **Subnet**: Public Subnet
   - **Auto-assign Public IP**: Disabled
3. **Security Group Rules**:
   - SSH (22) – Source: Your IP
   - HTTP (80) – Source: 0.0.0.0/0
4. Launch the instance

---

## Step 2: Allocate an Elastic IP

1. Go to **EC2 → Elastic IPs**
2. Click **Allocate Elastic IP address**
3. Use default settings
4. Click **Allocate**

---

## Step 3: Associate Elastic IP with EC2 Instance

1. Select the allocated Elastic IP
2. Click **Actions → Associate Elastic IP**
3. Configure:
   - **Resource type**: Instance
   - **Instance**: Select your EC2 instance
   - **Private IP**: Auto-selected
4. Click **Associate**

---

## Step 4: Connect to the EC2 Instance

```bash
ssh -i key.pem ec2-user@<Elastic-IP>
```
---

## Step 5: Install Web Server (Apache)
```
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

Create a test web page:

` echo "<h1>EC2 with Elastic IP - KodeKloud Day 21</h1>" | sudo tee /var/www/html/index.html `

---

## Step 6: Verify Application Access

Open a browser and visit:
```
http://<Elastic-IP>
```
Expected output: Web page served successfully.

---

## Validation Checklist

- EC2 instance is running
- Elastic IP is associated
- Static IP remains after reboot
- Security Group allows HTTP (80)
- Application accessible via browser

---

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/771c2103-5754-4d3e-9683-e24382e2370e" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f9251deb-f302-477d-a3a9-04a661bd8698" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ce43b64e-2048-408b-b2c6-947c086dc5f7" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/7cbb0de0-6749-42cc-a996-dda5924f770a" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1abed14d-e970-47a2-807b-dcfca07958b2" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/528bd6ab-c1b9-4c6f-a3b6-e03574f866bb" />





