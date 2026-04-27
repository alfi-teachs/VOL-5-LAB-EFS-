# VOL-5-LAB-EFS-


AWS EFS with EC2 – Step-by-Step Lab
# Step 1: Launch EC2 Instance
Go to EC2 Dashboard
Click Launch Instance
Configure:
AMI: Amazon Linux
Instance type: t2.micro
Key pair: Create or select existing
Under Network settings:
Choose a VPC
Create a Security Group (EC2-SG):
SSH (Port 22) → Anywhere (IPv4)
Launch the instance

# Step 2: Create EFS File System
Go to EFS Dashboard
Click Create File System
Provide:
Name (e.g., my-efs)
Select same VPC as EC2
Click Create

# Step 3: Configure EFS Security Group
Go to Security Groups
Find the security group attached to EFS (or create new: EFS-SG)
Add Inbound Rule:
Type: NFS
Port: 2049
Source: EC2-SG (Security Group of EC2 instance)

This ensures only your EC2 can access EFS.

# Step 4: Update EC2 Security Group
Go to EC2 → Instance → Security
Edit EC2-SG
Add Inbound Rule:
Type: NFS
Port: 2049
Source: Anywhere (IPv4) (for lab purpose)
(In real-world, restrict to EFS-SG)

# Step 5: Connect to EC2 Instance
ssh -i your-key.pem ec2-user@<public-ip>

# Step 6: Install NFS Client
sudo yum install -y nfs-utils






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













































































