# VOL-5-LAB-EFS-
AWS EFS with EC2 – Clean Lab Setup
# Step 1: Create EC2 Instance

Launch EC2

AMI: Amazon Linux

Instance type: t2.micro

Key pair: create/select

Network: choose VPC

Subnet: any public subnet

Launch instance

# Step 2: Create EFS (Elastic File System)

Go to EFS → Create File System

Give a name

Important: Select the same VPC as EC2

Create EFS

# Step 3: Configure Security Groups (Important Part)

A. EC2 Security Group

Inbound rules:

SSH (Port 22) → Anywhere (0.0.0.0/0)

NFS (Port 2049) → EFS Security Group (best practice)

B. EFS Security Group

Go to EFS → Network → Security Group

Edit inbound rules:

Type: NFS

Port: 2049

Source: EC2 Security Group

This creates secure communication between EC2 and EFS.

# Step 4: Connect to EC2

ssh -i key.pem ec2-user@<public-ip>


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
Go to EFS → Attach → Copy mount via DNS (EFS helper)

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
```bash
ls
```

# Step 7: Create Second EC2 Instance

Same VPC

Same subnet (or another subnet in same VPC)

Attach same EC2 security group

# Step 8: Mount EFS on Second Server

Connect:
```bash 
sudo su
```
```bash
yum install amazon-efs-utils -y
```
```bash
mkdir /store
```
# Mount:
```bash 
mount -t efs fs-xxxx:/ /store
```

Check:
```bash
cd /store
```
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
















































