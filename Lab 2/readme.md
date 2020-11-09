# Lab 2: Bastion Host Configuration
- <a href=#introduction>Introduction</a>
- <a href=#blueprint>Blueprint</a>
- <a href=#elastic-ip>Elastic IP</a>
- <a href=#key-pair>Key Pair</a>
- <a href=#security-group>Security Group</a>
- <a href=#iam-policy>IAM Policy</a>
- <a href=#iam-role>IAM Role</a>
- <a href=#launch-template>Launch Template</a>
- <a href=#auto-scaling-group>Auto Scaling Group</a>
- <a href=#private-key-forwarding>Private Key Forwarding (Putty/Pageant)</a>


## Introduction

In this lab, we will configure a bastion host that is singly redundant by leveraging on auto-scaling group.

A bastion host is a server whose purpose is to provide access to a private network from an external network, such as the Internet. 
Because of its exposure to potential attack, a bastion host must minimize the chances of penetration.

## Blueprint
<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-1.PNG>
</p>

## Elastic IP

An Elastic IP address is a static IPv4 address designed for dynamic cloud computing. By using an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account. 
An Elastic IP address is allocated to your AWS account, and is yours until you release it.

1) In **EC2** service page, navigate to **Elastic IPs** page and click on **Allocate Elastic IP address**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-2.PNG>
</p>

2) Use **default options** (Use Amazon's Pool of IPv4 Addresses)
3) Click "Allocate"

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-3.PNG>
</p>

**Note: Save the IP Address for accessing the instance and resource ID for cli attachment of elastic IP onto the bastion host later**

## Key Pair
A key pair, consisting of a private key and a public key, is a set of security credentials that you use to prove your identity when connecting to an instance.

1) In **EC2** service page, navigate to **Key Pairs** page and click on **Create Key Pair**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-4.PNG>
</p>

2) Tag VPC resource with a **name:** bastion-keys
3) Select **ppk** file format 
4) Click **Create Key Pair**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-32.PNG>
</p>

**Note: Key pair will be downloaded automatically on your browser, save it for accessing the bastion host later**
## Security Group
A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. They can be attached to certain resources such as EC2
on AWS.

1) In **EC2** service page, navigate to **Security Group** page and click on **Create security group**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-5.PNG>
</p>

2) Set **Security Group Name:** Bastion-SecG
3) Set **Description:** Allows SSH For Bastion Host
4) Under **Inbound Rules**, Click **Add rule**
5) Select **Type:** SSH, **Source:** Custom 0.0.0.0/0 
- **Note: Best Practice, the source should be set as trusted IPs only.**
6) Click **Create security group**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-6.PNG>
</p>
## IAM Policy
A policy defines the AWS permissions that you can assign to a user, group, or role.

In this section, we will create a policy to allow our bastion host to handle ElasticIPs resource 

1) In **IAM** service page, navigate to **Policies** page and click on **Create policy**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-7.PNG>
</p>

2) Select the **JSON** tab, copy and paste the code below into the editor. 
The following JSON object denotes an **Allow** for certain actions on other resources (e.g DisassociateAddress)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2:ReleaseAddress",
                "ec2:DisassociateAddress",
                "ec2:DescribeNetworkInterfaces",
                "ec2:AssociateAddress",
                "ec2:AllocateAddress"
            ],
            "Resource": "*"
        }
    ]
}
```
3) Click **Review policy** 

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-8.PNG>
</p>

4) Set **Name:** AllowEC2AccessENI
5) Set **Description:** To Allow EC2 Instances to access ENI resources
6) Click **Create Policy**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-9.PNG>
</p>

## IAM Role
IAM roles are a secure way to grant permissions to entities that you trust. Examples of entities include the following:
- IAM user in another account
- Application code running on an EC2 instance that needs to perform actions on AWS resources
- An AWS service that needs to act on resources in your account to provide its features
- Users from a corporate directory who use identity federation with SAML

We are going to create a role for our EC2 and attach the previously created policy to it. This will allow any EC2 instances
attached with this role to have permission to handle elastic IPs resource

1) In **IAM** service page, navigate to **Roles** page and click on **Create Role**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-10.PNG>
</p>

2) Select **Type** as **AWS service**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-11.PNG>
</p>

3) Choose and Select **Use Case:** EC2 
4) Click **Next: Permissions**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-12.PNG>
</p>

5) Attach **Permissions policy:** AllowEC2AccessENI

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-13.PNG>
</p>

6) Set **Role name:** Bastion-ENI
7) Set **Role description:** Allow EC2 Instances to access ENI resources
8) Click **Create role**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-14.PNG>
</p>

## Launch Template

1) In **EC2** service page, navigate to **Launch Templates** page and click on **Create launch template**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-31.PNG>
</p>

2) Set **Launch Template Name:** Bastion-Host-Recovery
3) Select **AMI:** Amazon Linux 2 AMI
4) Select **Instance Type:** t2.micro

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-15.PNG>
</p>

5) Set **Key pair:** bastion-keys (Previously Created)
6) Set **Security Groups** Bastion-SecG (Previously Created)

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-16.PNG>
</p>

7) Under **advanced details**, select **IAM Instance profile:** Bastion-ENI (Previously Created)

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-17.PNG>
</p>

***User Data***
When you launch an instance in Amazon EC2, you have the option of passing user data to the instance that can be used to perform common 
automated configuration tasks and even run scripts after the instance has started

**We will add a script for the launch template to use during automated EC2 instance creation. The script will use AWS CLI to attach
an elastic IP to it. This ensures the bastion host that is created to always have the same IP for access**

8) Under **User data**, Add the following script. 
**Note: Change < Your ElasticIP ID > to your previously created elastic IP's resource ID. **

```
#!
INSTANCE_ID=`/usr/bin/curl -s http://169.254.169.254/latest/meta-data/instance-id`\
aws ec2 associate-address --instance-id $INSTANCE_ID --allocation-id <Your ElasticIP ID> --allow-reassociation --region ap-southeast-1
```

9) Click **Create launch template**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-18.PNG>
</p>
## Auto Scaling Group

1) In **EC2** service page, navigate to **Auto Scaling Group** page and click on **Create An Auto Scaling group**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-19.PNG>
</p>

2) Set **Auto Scaling Group name:** Bastion-Host-Recovery
3) Select **Launch Template:** Bastion-Host-Recovery
4) Click **Next**
 
<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-20.PNG>
</p>
 
5) Select **VPC:** 3-tier-vpc
6) Select **Subnets:** public-subnet-1 & public-subnet-2
7) Click **Next**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-21.PNG>
</p>

8) Leave Options Default
9) Click **Next**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-22.PNG>
</p>

10) Leave Options Default
11) Click **Next**

**Note: Setting 1/1/1 capacity will ensure there is at least one bastion host up at a time**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-23.PNG>
</p>

12) Leave Options Default
13) Click **Next**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-24.PNG>
</p>

14) **Optional** to add **Tag:** Bastion-ASG
15) Click **Next**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-25.PNG>
</p>

16) Click **Create Auto Scaling Group**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-26.PNG>
</p>

## Private Key Forwarding (Putty/Pageant)

Download Putty & Pageant:
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

To Access Bastion Host, 

1) Add **private key (ppk)** assigned to EC2 Instances in Launch Template into pageant

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-27.PNG>
</p>

2) Allow **Agent-forwarding** in Putty

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-28.PNG>
</p>

3) Specify **Hostname** as the ec2-user@{bastion host elastic IP}

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-29.PNG>
</p>

4) Click **Open**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-30.PNG>
</p>