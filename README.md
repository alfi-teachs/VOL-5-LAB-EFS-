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

sudo su

# Step 5: Install EFS Utilities

yum install amazon-efs-utils -y

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

df -h


Create test files:

touch f1 f2 f3
ls

Step 7: Create Second EC2 Instance
Same VPC
Same subnet (or another subnet in same VPC)
Attach same EC2 security group
Step 8: Mount EFS on Second Server

Connect:

sudo su
yum install amazon-efs-utils -y
mkdir /store


Mount:

mount -t efs fs-xxxx:/ /store


Check:

cd /store
ls


You should see:

f1 f2 f3

What this really means
EFS is shared storage
Multiple EC2 instances can access the same data
It behaves like a network drive (NFS)
Common Mistakes You Had (Fixed)
Don’t allow NFS from Anywhere (0.0.0.0/0) → security risk
Always use Security Group as source
No need to remove default SG randomly
Keep architecture clean and controlled





# CREATE  EC2
AMAZON LINUX 
KEYPAIR 
T2.MICRO 
LAUNCH

# SEARCH EFS AND SELECT
CREATE FILE SYSYTEM
NAME
# VPC ----SHOULD SAME VPC AS EC2 INSTANCE
CREATE FILE SYSTEM

# CONNECT TO INSTANCE

EFS HAS SECURITY GROUP
swelect efs created

create securirty group

in networking check security group

create security grop
in default 
inbound : ssh anyehere ipv4   

insisde ec2 

change securirty group
remove default 
check one we created and create --save


# go to security group 
name efs -sever 
and change default of efs securuity group

inbound 
nfs
port ramnge : 2049
source type: customer 
source : select security group created 

save 

# go to ec2 go change in securirty group
inbound
type  NFS
anywhere ipv4
save

coonect server
sudo su
yum install amazon -efs-utils -y

create directory
mkdir /efs
cd /efs



how to attach on server 
efs 
attach
mount via dns
az of server
using efs mount helper because we installed utility installled 
copy and paste to server 
df -h
cd /efsc
touch f1 f2 f3 
ls

create new server and attach the efs on it to check efs data is available
subnet same as ec2
select existing security group
create instance

connect to instance
sudo su
yum install amazon -efs-utils
mkdir /store


now we can mount go to efs
attach
mount via dns
copy paste server

cd /store
check ls u wil see same file fi f2 f3













































































