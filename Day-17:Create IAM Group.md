# KodeKloud 100 Days of Cloud – Day 17  
## AWS IAM: Create IAM Group

### Task Description
As part of the AWS cloud migration, the Nautilus DevOps team is organizing access management using IAM groups.  
This task focuses on creating an IAM group to simplify permission management for multiple users.

---

### Requirements
- **IAM Group Name:** `iamgroup_ravi`
- **Region:** `us-east-1`

---

### AWS Credentials
Use the credentials provided on the `aws-client` host.

Retrieve credentials:
```bash
showcreds
```

Export them:

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

## Step 1: Create IAM Group

Run the following command to create the IAM group:

```
aws iam create-group \
  --group-name iamgroup_ravi
```
---

## Step 2: Verify IAM Group Creation

List IAM groups:

```
aws iam list-groups
```
Or get details of the specific group:

```
aws iam get-group \
  --group-name iamgroup_ravi
```
---

### ✅ The task is successfully completed once the IAM group iamgroup_ravi is created and visible in the IAM group list.

### Notes

- IAM is a global AWS service, but follow lab instructions to use the us-east-1 region.
- No users or policies are required to be attached for this task.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/fe274438-2df2-4bc3-bda0-a1013c5ab943" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/33d38191-bfa4-437a-b7b5-44072b626ddc" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/df8587d8-c952-4741-9fd9-1889dddd71e3" />






