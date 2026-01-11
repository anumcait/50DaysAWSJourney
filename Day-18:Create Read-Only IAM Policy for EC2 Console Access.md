# Day 18: Create Read-Only IAM Policy for EC2 Console Access

### Task Description
Identity and Access Management (IAM) is a foundational AWS service for controlling access to resources.  
The Nautilus DevOps team requires a custom IAM policy that provides **read-only access** to the Amazon EC2 console so users can view instances, AMIs, and snapshots.

---

### Requirements
- **IAM Policy Name:** `iampolicy_javed`
- **Access Level:** Read-only
- **Services:** Amazon EC2 (instances, AMIs, snapshots)
- **Region:** `us-east-1`

---

### AWS Credentials
Retrieve credentials from the `aws-client` host.

```bash
showcreds
```

Export credentials:

```
export AWS_ACCESS_KEY_ID=<ACCESS_KEY>
export AWS_SECRET_ACCESS_KEY=<SECRET_KEY>
export AWS_DEFAULT_REGION=us-east-1
```

Verify access:

```
aws sts get-caller-identity
```
---

## Step 1: Create Policy JSON File

Create a file named ec2-readonly-policy.json:

vi ec2-readonly-policy.json


Add the following content:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
```

Save and exit the file.

---

## Step 2: Create IAM Policy

Run the following command to create the IAM policy:

```
aws iam create-policy \
  --policy-name iampolicy_javed \
  --policy-document file://ec2-readonly-policy.json
```
---

## Step 3: Verify IAM Policy Creation

List IAM policies:

```
aws iam list-policies --scope Local
```

Or get details of the policy:

```
aws iam get-policy \
  --policy-arn <POLICY_ARN>
```
---

### ✅ The task is successfully completed once the IAM policy iampolicy_javed is created and visible in the IAM policies list.

#### Notes

- IAM is a global service, but follow lab instructions to work within us-east-1.
- This policy provides view-only access and does not allow any EC2 modifications.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8c225054-4b12-4cdc-b6b6-280778f56d9f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e662796e-b22c-4f3b-b3d9-c4ba76111717" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8c26b258-351c-4f5f-865e-fe9589b388b9" />



