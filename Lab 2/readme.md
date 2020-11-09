# Lab 2: VPC & Subnet Configuration
- <a href=#private-key-forwarding>Private Key Forwarding</a>
- <a href=#blueprint>Blueprint</a>
- <a href=#>VPC Configuration</a>
- <a href=#subnet-configuration>Subnet Configuration</a>
- <a href=#internet-gateway>Internet Gateway</a>
- <a href=#nat-gateway>NAT Gateway</a>
- <a href=#route-table-configuration>Route Table Configuration</a>

## Introduction

In this lab, we will configure a bastion host that is singly redundant by leveraging on auto-scaling group.

A bastion host is a server whose purpose is to provide access to a private network from an external network, such as the Internet. 
Because of its exposure to potential attack, a bastion host must minimize the chances of penetration.



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
