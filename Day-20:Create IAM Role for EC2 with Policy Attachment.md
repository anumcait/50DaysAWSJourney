# Day 20: Create IAM Role for EC2 with Policy Attachment

### Task Description
Identity and Access Management (IAM) plays a critical role in securely managing access to AWS services.  
In this task, the Nautilus DevOps team requires an IAM role to be created for an EC2 service and an existing policy to be attached to it.

---

### Requirements
- **IAM Role Name:** `iamrole_rose`
- **Entity Type:** AWS Service
- **Use Case:** EC2
- **IAM Policy to Attach:** `iampolicy_rose`
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

## Step 1: Create Trust Policy for EC2

Create a trust policy file named ec2-trust-policy.json:

vi ec2-trust-policy.json

Add the following content:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Save and exit the file.

---

## Step 2: Create IAM Role

Create the IAM role using the trust policy:
```
aws iam create-role \
  --role-name iamrole_rose \
  --assume-role-policy-document file://ec2-trust-policy.json
```
---

## Step 3: Get Policy ARN

List local IAM policies to find the ARN for iampolicy_rose:
```
aws iam list-policies --scope Local
```

Copy the PolicyArn associated with iampolicy_rose.

---

## Step 4: Attach Policy to IAM Role

Replace <POLICY_ARN> with the actual ARN:
```
aws iam attach-role-policy \
  --role-name iamrole_rose \
  --policy-arn <POLICY_ARN>
```
---

## Step 5: Verify Role and Policy Attachment

Verify role details:
```
aws iam get-role \
  --role-name iamrole_rose
```

Verify attached policies:
```
aws iam list-attached-role-policies \
  --role-name iamrole_rose
```

Ensure iampolicy_rose is listed.

## ✅ The task is successfully completed once:

- IAM role iamrole_rose exists
- The role is trusted by EC2
- Policy iampolicy_rose is attached to the role

### Notes

- IAM is a global AWS service, but follow lab instructions to work in us-east-1.
- This role can now be attached to EC2 instances to grant permissions defined in the policy.

---

<img width="1050" height="516" alt="image" src="https://github.com/user-attachments/assets/b18d2cc6-c7c0-40e1-b6f1-891de8294e9e" />

<img width="1050" height="518" alt="image" src="https://github.com/user-attachments/assets/5f77746d-cf78-4c0d-93e0-b862c8c086a2" />



