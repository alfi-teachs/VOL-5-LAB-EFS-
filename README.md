Lab: EC2 with EFS using Security Groups (SSH + NFS

# Step 1: Create Security Group for EC2

Go to EC2 → Security Groups → Create

Name: EC2-SG

Inbound Rules:

SSH → Port 22 → Source: 0.0.0.0/0

NFS → Port 2049 → Source: 0.0.0.0/0

Outbound:

Allow all (default)

Create SG.

# Step 2: Create Security Group for EFS

Create another Security Group.

Name: EFS-SG

Inbound Rules:

NFS → Port 2049
Source: Select EC2-SG (not IP)

This is important:
→ Only EC2 instances with that SG can access EFS

# Step 3: Create EFS

Go to EFS → Create File System

Name: MyEFS

Select same VPC as EC2

In Network settings:

Assign EFS-SG

Create file system.

# Step 4: Launch EC2 Instance

Go to EC2 → Launch Instance

AMI: Amazon Linux

Instance type: t2.micro

Key pair: select/create

Network: same VPC

Security Group: Select EC2-SG

Launch instance.

# Step 4: Connect to EC2
```bash
ssh -i key.pem ec2-user@<public-ip>
```

Switch to root:
```bash
sudo su
```

# Step 5: Install EFS Utilities
```bash
yum install amazon-efs-utils -y
```

# Step 6: Mount EFS on EC2

Create directory:
```bash 
mkdir /efs
```
```bash
cd /efs
```
# Go to EFS → Attach → Copy mount via DNS (EFS helper)

Example:
```bash
mount -t efs fs-xxxx:/ /efs
```


Verify:
```bash

df -h
```
Create test files:
```bash
touch f1 f2 f3
```
list files :

```bash
ls
```
# Step 7: Create Second EC2 Instance

Same VPC

Same subnet (or another subnet in same VPC)

Attach same EC2 security group

# Step 8: Mount EFS on Second Server

Become root user

This gives full permissions to install packages and mount file systems.
Connect:

```bash 
sudo su
```
# step 9
Install EFS utilities

This installs the required package to mount Amazon EFS easily.

```bash
yum install amazon-efs-utils -y
```
# step 10 
Create mount directory

This creates a folder where the EFS will be attached.
```bash
mkdir /store
```

# step 11
Mount the EFS

This connects your EFS file system to the directory /store.
Replace fs-xxxx with your actual EFS File System ID.

```bash 
mount -t efs fs-xxxx:/ /store
```
# step 12
Go inside the mounted directory

This moves you into the EFS storage location.
Check:

```bash
cd /store
```
# step 13
List files

This shows the files stored in EFS.
```bash 
ls
```
You should see:

f1 f2 f3

# What this really means
EFS is shared storage

Multiple EC2 instances can access the same data

It behaves like a network drive (NFS)

# Common Mistakes You Had (Fixed)

Don’t allow NFS from Anywhere (0.0.0.0/0) → security risk

Always use Security Group as source

No need to remove default SG randomly

Keep architecture clean and controlled

# What This Really Means

You’ve built shared storage using Amazon EFS:

Multiple EC2 instances read/write same data

Works like a network file system (NFS)

Fully managed and scalable

No need to manage disks manually

# Important Additions You Were Missing

Here’s the part that quietly upgrades your lab from basic to strong:

1. Mount Targets

EFS must have mount targets in correct AZ
Without this → mounting fails

2. Persistent Mount (/etc/fstab)

Otherwise data disappears after reboot

3. TLS Mount Option

Adds encryption in transit

4. Security Group Linking (Best Practice)

Use SG → SG, not 0.0.0.0/0

# Common Mistakes (Now Fully Fixed)

Opening NFS to the world (security issue)

Forgetting mount targets

Not persisting mount

Mixing VPCs (EFS and EC2 must match)

Missing EFS utilities
















































