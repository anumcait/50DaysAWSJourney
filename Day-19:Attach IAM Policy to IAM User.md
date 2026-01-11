# Day 19: Attach IAM Policy to IAM User

### Task Description
As part of the ongoing AWS migration, the Nautilus DevOps team is managing IAM permissions by attaching policies directly to users.  
In this task, an existing IAM policy must be attached to an existing IAM user.

---

### Requirements
- **IAM User Name:** `iamuser_james`
- **IAM Policy Name:** `iampolicy_james`
- **Region:** `us-east-1`

---

### AWS Credentials
Retrieve credentials from the `aws-client` host.

```bash
showcreds
```

Export the credentials:
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

## Step 1: Get IAM Policy ARN

List IAM policies and identify the ARN for iampolicy_james:
```
aws iam list-policies --scope Local
```

Copy the PolicyArn associated with iampolicy_james.

---

## Step 2: Attach Policy to IAM User

Replace <POLICY_ARN> with the actual ARN:
```
aws iam attach-user-policy \
  --user-name iamuser_james \
  --policy-arn <POLICY_ARN>
```
---

## Step 3: Verify Policy Attachment

List policies attached to the user:

```
aws iam list-attached-user-policies \
  --user-name iamuser_james
```

Confirm that iampolicy_james appears in the output.

---

### ✅ The task is successfully completed once the IAM policy iampolicy_james is attached to the IAM user iamuser_james.

### Notes
- IAM is a global AWS service, but follow lab instructions to operate in the us-east-1 region.
- No changes or edits to the policy are required—only attachment.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/2b794a29-3df3-485d-9ab7-7315a5137904" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/74ce0479-507e-4e0b-bf5b-bd32c4820f2f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/4596343c-cdd4-437a-9a74-8237a2481463" />



