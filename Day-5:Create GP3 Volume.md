# KodeKloud 50 Days AWS Challenge – Day 5: Create GP3 Volume

## Objective
Create an **EBS volume** as part of the Nautilus DevOps team’s incremental AWS migration strategy.

---

## Requirements

- **Volume Name:** devops-volume  
- **Volume Type:** gp3  
- **Volume Size:** 2 GiB  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

---


## Step 2: Identify an Availability Zone

An EBS volume must be created in a specific Availability Zone.

List available AZs:
```
aws ec2 describe-availability-zones \
--query "AvailabilityZones[].ZoneName" \
--output text
```

Choose one AZ (example: us-east-1a).

---

Step 3: Create the GP3 Volume

Replace <AZ> with your chosen Availability Zone.
```
aws ec2 create-volume \
--availability-zone <AZ> \
--size 2 \
--volume-type gp3
```

Sample output:

{
    "VolumeId": "vol-0123456789abcdef0",
    "Size": 2,
    "VolumeType": "gp3",
    "AvailabilityZone": "us-east-1a",
    "State": "creating"
}

---

> Save the VolumeId. This is required for tagging.

---

## Step 4: Tag the Volume
```
aws ec2 create-tags \
--resources <VOLUME_ID> \
--tags Key=Name,Value=devops-volume
```

Replace <VOLUME_ID> with the actual VolumeId from Step 3.

---

## Step 5: Verify Volume Creation
```
aws ec2 describe-volumes \
--volume-ids <VOLUME_ID>
```

**Verify:**

- Volume type is gp3
- Size is 2 GiB
- Tag Name=devops-volume is present

**✅ Result**

EBS volume devops-volume exists

- Volume type is gp3
- Volume size is 2 GiB
- Volume is created in us-east-1

---

Notes

> EBS volumes are AZ-specific
> 
> Volume does not need to be attached unless explicitly required
>
> Ensure no spaces in Key=Name,Value=devops-volume


## screenshots
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/791f3887-3a10-4191-a35a-6be9b0bfbe68" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/bdd39d44-2d16-4abc-8e9c-bc92d3397d7f" />





