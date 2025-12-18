# KodeKloud 50 Days AWS Challenge – Day 12: Attach Volume to EC2 Instance

## Objective
Attach an existing **EBS volume** to an existing **EC2 instance** with a specific device name as part of the incremental AWS migration tasks.

---

## Requirements

- **EC2 Instance Name:** devops-ec2  
- **EBS Volume Name:** devops-volume  
- **Device Name:** /dev/sdb  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

(Optional) Verify AWS credentials:

```bash
showcreds
```

Set the AWS region:

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

## Step 3: Get EBS Volume ID

Retrieve the Volume ID for devops-volume:
```
aws ec2 describe-volumes \
--filters Name=tag:Name,Values=devops-volume \
--query "Volumes[].VolumeId" \
--output text
```

Save the returned VolumeId.

## Step 4: Attach the Volume to the EC2 Instance

Attach the volume using device name /dev/sdb:
```
aws ec2 attach-volume \
--volume-id <VOLUME_ID> \
--instance-id <INSTANCE_ID> \
--device /dev/sdb
```

**Replace:**

<VOLUME_ID> with the EBS Volume ID

<INSTANCE_ID> with the EC2 Instance ID

---

## Step 5: Verify Volume Attachment

Verify that the volume is attached to the instance:
```
aws ec2 describe-volumes \
--volume-ids <VOLUME_ID> \
--query "Volumes[].Attachments[].{InstanceId:InstanceId,Device:Device,State:State}" \
--output table
```

Expected output:
-----------------------------
|      DescribeVolumes     |
+-------------+------------+
| InstanceId  | <INSTANCE> |
| Device      | /dev/sdb   |
| State       | attached   |
+-------------+------------+

---

**✅ Result**

- EBS volume devops-volume is attached to devops-ec2
- Device name is correctly set to /dev/sdb
- Volume attachment state is attached
- All actions performed in us-east-1

**Notes**

- The EC2 instance must be running or stopped (running is preferred)
- Device name must match the requirement exactly
- AWS may map /dev/sdb internally as /dev/xvdb on Amazon Linux

> Attach Volume to EC2 Instance completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/0d1adbc6-e0f7-4ef5-a75a-c7ea700f7408" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/de753a0a-f349-4cf5-b877-b2681f1626aa" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b657a4ea-4618-4497-ad50-07efbf188339" />

---






