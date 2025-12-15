# KodeKloud AWS Challenge – Day 1: Create Key Pair

## Objective
Create an **EC2 Key Pair** that will be used to securely access EC2 instances as part of the Nautilus DevOps team’s AWS migration plan.

---

## Requirements

- **Key Pair Name:** nautilus-key
- **Key Pair Type:** RSA
- **Private Key Format:** PEM
- **Region:** us-east-1

---

## Step 1: Login to aws-client

Open the terminal on the `aws-client` host.

## Step 2: Create EC2 Key Pair

Run the following command to create the key pair and save the private key locally:

```bash
aws ec2 create-key-pair \
--key-name nautilus-key \
--key-type rsa \
--query "KeyMaterial" \
--output text > nautilus-key.pem
```
---

## Step 3: Secure the Private Key File

Set appropriate permissions on the key file:

```bash
chmod 400 nautilus-key.pem
```
---

## Step 4: Verify Key Pair Creation

Confirm that the key pair exists in AWS:
```bash
aws ec2 describe-key-pairs \
--key-names nautilus-key
```
---
## Result

- EC2 key pair named nautilus-key is created
- Private key file nautilus-key.pem exists on the aws-client host
- File permissions are set to 400
- Key pair is available in the us-east-1 region

---

**Notes**

- The private key file cannot be downloaded again after creation
- Keep the .pem file secure
- If you receive an InvalidKeyPair.Duplicate error, the key pair already exists

---

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e1dac62c-3550-4e15-8e6a-bcd8d296e8b3" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a9c734b7-bf42-44b1-9983-0225b002a868" />

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d2e3c8de-72b6-418f-89ae-696164b88c34" />


---


