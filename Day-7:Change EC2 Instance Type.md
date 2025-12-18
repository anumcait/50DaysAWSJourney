# Day 7: Change EC2 Instance Type

## Objective
Modify an existing **EC2 instance type** to optimize resource utilization and ensure the instance remains in a running state after the change.

---

## Requirements

- **Instance Name:** devops-ec2  
- **Current Instance Type:** t2.micro  
- **New Instance Type:** t2.nano  
- **Final State:** running  
- **Region:** us-east-1  

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

(Optional) Verify AWS credentials:

```bash
showcreds
```
Set the correct region:
```
export AWS_DEFAULT_REGION=us-east-1
```
---

## Step 2: Get the Instance ID

Retrieve the Instance ID for devops-ec2:
```
aws ec2 describe-instances \
--filters Name=tag:Name,Values=devops-ec2 \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Save the returned **InstanceId**.

## Step 3: Ensure Status Checks Are Completed

Verify that the instance status checks are complete:
```
aws ec2 describe-instance-status \
--instance-ids <INSTANCE_ID>
```

Proceed only if:

- InstanceStatus.Status = **ok**
- SystemStatus.Status = **ok**

---

## Step 4: Stop the EC2 Instance

An instance must be stopped before changing its type.

```
aws ec2 stop-instances \
--instance-ids <INSTANCE_ID>
```

Wait until the instance is fully stopped:

```
aws ec2 wait instance-stopped \
--instance-ids <INSTANCE_ID>
```
---

## Step 5: Change the Instance Type

Modify the instance type from t2.micro to t2.nano:
```
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--instance-type "{\"Value\":\"t2.nano\"}"
```
---

## Step 6: Start the EC2 Instance

```
aws ec2 start-instances \
--instance-ids <INSTANCE_ID>
```


Wait until the instance is running:
```
aws ec2 wait instance-running \
--instance-ids <INSTANCE_ID>
```
---

## Step 7: Verify Instance Type and State
```
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[].Instances[].{State:State.Name,Type:InstanceType}" \
--output table
```

**Expected output:**

-------------------------
|  DescribeInstances   |
+-----------+----------+
|  State    |  running |
|  Type     | t2.nano  |
+-----------+----------+

---

**✅ Result**

- EC2 instance devops-ec2 is running
- Instance type successfully changed to t2.nano
- Status checks passed before modification
- Instance remains healthy after restart

**Notes**

- EC2 instance must be stopped before modifying instance type
- Do not attempt modification while status checks are initializing
- Changes apply only within us-east-1

> Change EC2 Instance Type completed successfully ✅

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b661614e-8dfb-40a7-acdd-c7c92d9db399" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/86bbf9b8-e407-4197-a4bb-1f457d0540db" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d81f2978-afe4-474d-977a-186ab1692ee9" />

---





