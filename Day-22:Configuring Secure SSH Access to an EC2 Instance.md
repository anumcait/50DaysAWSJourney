# Day 22: Configuring Secure SSH Access to an EC2 Instance

Requirement:
- Region: us-east-1
- Instance type: t2.micro
- EC2 name: datacenter-ec2
- SSH key location: /root/.ssh/
- Passwordless SSH: aws-client → EC2 (root user)

## Step 1: Login to AWS from aws-client

On the aws-client host, load credentials:
```
showcreds
aws configure
```

Enter values from showcreds
Set region explicitly:
```
aws configure set region us-east-1
```

Verify:
```
aws sts get-caller-identity
```
---

## Step 2: Create SSH key on aws-client (if not present)
```
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```

Check and create key:
```
[ ! -f /root/.ssh/id_rsa ] && ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa -N ""
```

Public key:
```
cat /root/.ssh/id_rsa.pub
```
---

## Step 3: Create a Security Group (SSH access)
```
SG_ID=$(aws ec2 create-security-group \
  --group-name xfusion-ssh-sg \
  --description "Allow SSH access" \
  --query 'GroupId' \
  --output text)
```

Allow SSH:
```
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0
```
---

## Step 4: Get Latest Amazon Linux 2 AMI
```
AMI_ID=$(aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
            "Name=state,Values=available" \
  --query 'Images | sort_by(@,&CreationDate)[-1].ImageId' \
  --output text)
```
---

## Step 5: Prepare User-Data to add SSH key to root
```
cat <<EOF > /root/userdata.sh
#!/bin/bash
mkdir -p /root/.ssh
echo "$(cat /root/.ssh/id_rsa.pub)" >> /root/.ssh/authorized_keys
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
EOF
```
---

## Step 6: Launch EC2 Instance
```
INSTANCE_ID=$(aws ec2 run-instances \
  --image-id $AMI_ID \
  --instance-type t2.micro \
  --security-group-ids $SG_ID \
  --user-data file:///root/userdata.sh \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=xfusion-ec2}]' \
  --query 'Instances[0].InstanceId' \
  --output text)
```

Wait until running:
```
aws ec2 wait instance-running --instance-ids $INSTANCE_ID
```
---

## Step 7: Get Public IP
```
PUBLIC_IP=$(aws ec2 describe-instances \
  --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].PublicIpAddress' \
  --output text)
```
---

## Step 8: Verify Passwordless SSH Access
```
ssh -i /root/.ssh/id_rsa root@$PUBLIC_IP
```

## ✅ You should log in without a password

- EC2 instance name: xfusion-ec2
- Instance type: t2.micro
- Region: us-east-1
- SSH key created under /root/.ssh/
- Key added to root authorized_keys
- Passwordless SSH from aws-client works

---

<img width="1050" height="532" alt="image" src="https://github.com/user-attachments/assets/b0853275-4887-4682-85e9-8891fcb28d13" />
<img width="1049" height="530" alt="image" src="https://github.com/user-attachments/assets/3f43c535-747f-4df9-a55a-3bb7a97a3869" />
<img width="1050" height="524" alt="image" src="https://github.com/user-attachments/assets/43812f33-6f1c-4d4e-95dd-a5fe2caf22db" />
<img width="1050" height="525" alt="image" src="https://github.com/user-attachments/assets/b9a11068-7bba-4736-b52c-311d4acf7300" />







