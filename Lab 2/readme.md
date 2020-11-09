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

```
#!
INSTANCE_ID=`/usr/bin/curl -s http://169.254.169.254/latest/meta-data/instance-id`\
aws ec2 associate-address --instance-id $INSTANCE_ID --allocation-id eipalloc-0c42634b4d8252ded --allow-reassociation --region ap-southeast-1
```

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

**Note: Save the IP Address for accessing the instance and resource ID for cli attachment of elastic IP onto the bastion instance later**

## Key Pair
A key pair, consisting of a private key and a public key, is a set of security credentials that you use to prove your identity when connecting to an instance.

1) In **EC2** service page, navigate to **Key Pairs** page and click on **Create Key Pair**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-4.PNG>
</p>

2) Tag VPC resource with a **name** bastion-keys
3) Select **ppk** file format 
4) Click **Create Key Pair**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%202/blob/lab-2-pic-32.PNG>
</p>

**Note: Key pair will be downloaded automatically on your browser, save it for accessing The instances later**
## Security Group

## IAM Policy

## IAM Role

## Launch Template

## Auto Scaling Group

## Private Key Forwarding (Putty/Pageant)