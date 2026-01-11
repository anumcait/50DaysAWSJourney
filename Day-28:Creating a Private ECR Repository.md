# Day 28: Creating a Private ECR Repository

## Objective
Create a **private Amazon ECR repository** using the **AWS Console**, then build a Docker image from a Dockerfile located on the **aws-client host** and push it to the ECR repository with the **latest** tag.

---

## Requirements
- **Repository name**: `nautilus-ecr`
- **Repository type**: Private
- **Dockerfile path**: `/root/pyapp` (on aws-client)
- **Image tag**: `latest`
- **Region**: `us-east-1`

---

## Step 1: Sign in to AWS Console

1. Open the AWS Console:
https://316890205783.signin.aws.amazon.com/console?region=us-east-1
2. Log in using the provided credentials.
3. Confirm the region is set to **us-east-1** (top-right corner).

---

## Step 2: Create a Private ECR Repository

1. Navigate to:
Amazon ECR → Repositories
2. Click **Create repository**
3. Configure:
- **Visibility settings**: Private
- **Repository name**: `nautilus-ecr`
- **Tag immutability**: Disabled (default)
- **Image scan settings**: Default
4. Click **Create repository**

✅ The private ECR repository `nautilus-ecr` is now created.

---

## Step 3: View Repository URI

1. Click on the repository **nautilus-ecr**
2. Copy the **Repository URI**, it will look like:
<ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr

You will need this URI when tagging and pushing the image.

---

## Step 4: Authenticate Docker to ECR (from aws-client)

On the **aws-client host** terminal:

```bash
aws sts get-caller-identity
```
Login to ECR:
aws ecr get-login-password --region us-east-1 | docker login \
--username AWS \
--password-stdin <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com

## Step 5: Build Docker Image from Dockerfile

Navigate to the Dockerfile directory:
```
cd /root/pyapp
```

Build the image:
```
docker build -t nautilus-ecr:latest .
```
---

## Step 6: Tag Docker Image for ECR
```
docker tag nautilus-ecr:latest \
<ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```
---

## Step 7: Push Docker Image to ECR
```
docker push \
<ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

## Step 8: Verify Image via AWS Console

1. Go back to:

```
Amazon ECR → Repositories → nautilus-ecr
```

2. Open Images tab

3. Confirm:

  - Image tag: latest
  - Image status: Available

---

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/8faabbbe-9585-4701-a296-86e06396263f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/9112c7c8-07a1-4c18-a8eb-cb6f66c6481c" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/1e98231a-fd88-4ed5-9c93-264405bf64e4" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/e2e6fe25-f0f4-4c85-81e5-f2f3d13bcf26" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/ad2445c1-94b4-4f3a-a49f-63e3e5012888" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/202275a1-a399-4920-9cc6-a47392f7283d" />






