## AWS IAM: Create IAM User

### Task Description
Identity and Access Management (IAM) is a core AWS service used to manage users, permissions, and access controls.  
The Nautilus DevOps team requires the creation of a new IAM user as part of their initial AWS setup.

---

### Requirements
- **IAM User Name:** `iamuser_siva`
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

## Step 1: Create IAM User
Run the following command to create the IAM user:

```
aws iam create-user \
  --user-name iamuser_siva
```
---

## Step 2: Verify IAM User Creation
List IAM users and confirm the new user exists:
```
aws iam list-users
```
---

Or describe the specific user:

```
aws iam get-user \
  --user-name iamuser_siva
```
---

## The task is successfully completed once the IAM user iamuser_siva is created and visible in the IAM user list.

## Notes
- IAM is a global service, but always use the provided region (us-east-1) as per lab instructions.
- No policies or access keys are required for this task.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d35153ff-9ec6-493a-85c1-6d7ce825736b" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/18aa10d4-8958-48bc-a484-48a0cdcb0e1a" />



