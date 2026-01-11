# Day 22: Configuring Secure SSH Access to an EC2 Instance

Requirement:
- Region: us-east-1
- Instance type: t2.micro
- EC2 name: datacenter-ec2
- SSH key location: /root/.ssh/
- Passwordless SSH: aws-client → EC2 (root user)

## Step 1: Verify AWS Client & Region
```
showcreds
aws configure set region us-east-1
aws configure get region
```

Output must be:
```
us-east-1
```
---

## Step 2: Create SSH Key on aws-client

```
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```

Create the key (only if not exists):
```
ssh-keygen -t rsa -b 2048 -f /root/.ssh/datacenter_key -N ""
```

Verify:
```
ls -l /root/.ssh/datacenter_key*
```
---

## Step 3: Get Latest Amazon Linux 2 AMI (Stable Method)
```
AMI_ID=$(aws ec2 describe-images \
--owners amazon \
--filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
--query 'Images | sort_by(@, &CreationDate)[-1].ImageId' \
--output text)
```
echo $AMI_ID

---

## Step 4: Create User Data Script (Adds SSH Key to root)

This is CRITICAL for passwordless SSH.
```
cat > /root/user-data.sh <<EOF
#!/bin/bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
echo "$(cat /root/.ssh/datacenter_key.pub)" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
sed -i 's/^#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
systemctl restart sshd
EOF
```
---

## Step 5: Launch EC2 Instance
aws ec2 run-instances \
--image-id $AMI_ID \
--instance-type t2.micro \
--count 1 \
--tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=datacenter-ec2}]" \
--user-data file:///root/user-data.sh


⏳ Wait 30–40 seconds

---

## Step 6: Get EC2 Public IP
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=datacenter-ec2" \
--query 'Reservations[].Instances[].PublicIpAddress' \
--output text


Save it:

EC2_IP=<PASTE_IP_HERE>

---

## Step 7: Test Passwordless SSH (FINAL)
ssh -i /root/.ssh/datacenter_key root@$EC2_IP


✅ No password prompt = SUCCESS

---

## Step 8: Verify Inside EC2
```
whoami
hostname
```

Expected:

```
root
ip-xxx-xxx-xxx-xxx
```

## Result

- ✔ EC2 instance datacenter-ec2 created
- ✔ SSH key generated on aws-client
- ✔ Root passwordless SSH configured
- ✔ Region us-east-1 compliance
- ✔ Matches KodeKloud evaluation criteria
