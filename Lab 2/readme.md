# Lab 2: VPC & Subnet Configuration
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
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Lab%201/blob/lab-2-pic-1.PNG>
</p>


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

#!
INSTANCE_ID=`/usr/bin/curl -s http://169.254.169.254/latest/meta-data/instance-id`
aws ec2 associate-address --instance-id $INSTANCE_ID --allocation-id eipalloc-0c42634b4d8252ded --allow-reassociation --region ap-southeast-1


## Elastic IP

## Key Pair

## Security Group

## IAM Policy

## IAM Role

## Launch Template

## Auto Scaling Group

## Private Key Forwarding (Putty/Pageant)