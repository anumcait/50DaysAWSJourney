# Day 23: Data Migration Between S3 Buckets Using AWS CLI

## Objective
Migrate all data from an existing S3 bucket to a new S3 bucket using AWS CLI and verify data consistency after migration.

---

## Requirements
- Existing bucket: **nautilus-s3-32175**
- New bucket: **nautilus-sync-18658**
- Region: **us-east-1**
- Bucket type: **Private**
- Ensure complete and accurate data migration
- Use **AWS CLI** only

---

## Step 1: Configure AWS CLI on aws-client

```bash
showcreds
aws configure
aws configure set region us-east-1
aws sts get-caller-identity
```
---

## Step 2: Create New Private S3 Bucket
```
aws s3api create-bucket \
  --bucket nautilus-sync-18658 \
  --region us-east-1
```

Verify bucket creation:
```
aws s3 ls | grep nautilus-sync-18658
```
---

## Step 3: Verify Source Bucket Contents
```
aws s3 ls s3://nautilus-s3-32175 --recursive
```
---

## Step 4: Migrate Data Using S3 Sync
```
aws s3 sync s3://nautilus-s3-32175 s3://nautilus-sync-18658
```

This command copies all objects while preserving directory structure and metadata.

## Step 5: Verify Data Consistency
Count objects in source bucket:
```
aws s3 ls s3://nautilus-s3-32175 --recursive | wc -l
```
Count objects in destination bucket:

```
aws s3 ls s3://nautilus-sync-18658 --recursive | wc -l
```
---

## Step 6: Optional Deep Verification (Dry Run)
```
aws s3 sync s3://nautilus-s3-32175 s3://nautilus-sync-18658 --dryrun
```
If no output is shown, both buckets are fully in sync.

Verification Checklist

- New bucket nautilus-sync-18658 created in us-east-1
- Bucket is private by default
- All objects copied from source bucket
- Object count matches between buckets
- Dry run confirms no missing or extra files

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/360d2253-27f0-4f17-9804-7782a2a09d4e" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/a15f6da0-ce03-4f22-ae32-a823ada2c6a2" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f8071bb1-f85b-471b-9341-8a220fd35e11" />



