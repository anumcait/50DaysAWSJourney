## AWS EBS: Create Volume Snapshot

### Task Description
The Nautilus DevOps team needs to configure automated backups for important AWS EBS volumes.  
As part of this task, create a snapshot of an existing EBS volume.

---

### Requirements
- **Volume Name:** `xfusion-vol`
- **Region:** `us-east-1`
- **Snapshot Name:** `xfusion-vol-ss`
- **Snapshot Description:** `xfusion Snapshot`
- **Snapshot State:** `completed`

---

### AWS Credentials
Credentials are provided via the `aws-client` host.

To retrieve them:
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
aws sts get-caller-identity

---

Step 1: Identify the Volume ID

List volumes and find the one named xfusion-vol:

```
aws ec2 describe-volumes \
  --filters Name=tag:Name,Values=xfusion-vol
```

Note the VolumeId from the output.

## Step 2: Create the Snapshot

Replace <VOLUME_ID> with the actual Volume ID:

```
aws ec2 create-snapshot \
  --volume-id <VOLUME_ID> \
  --description "xfusion Snapshot" \
  --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=xfusion-vol-ss}]'
```
Copy the returned `SnapshotId.`

---

Step 3: Wait for Snapshot Completion

Replace <SNAPSHOT_ID> with the Snapshot ID:
```
aws ec2 wait snapshot-completed \
  --snapshot-ids <SNAPSHOT_ID>
```
The command completes silently once the snapshot is ready.

## Step 4: Verify Snapshot
```
aws ec2 describe-snapshots \
  --snapshot-ids <SNAPSHOT_ID>
```
---

### Ensure:

- Name tag is xfusion-vol-ss
- Description is xfusion Snapshot
- State is completed

### The task is successfully completed once the snapshot state shows completed in the us-east-1 region.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/c11557f1-51d3-4f72-9d61-1928761e084b" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/df6bac37-7ee6-4843-bdf7-112c0b712a71" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/f52e442e-68f1-4502-9c87-451c485ffd9f" />
<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/7a9f3539-7dbe-4a18-9100-af9461f8cdfe" />



