# Three-Tier Infrastructure Setup
Learn to setup a highly redundant three-tier web application cloud infrastructure.


## Architecture 
<p align="center">
  <img src="https://github.com/ravensp93/aws-three-tier-web/blob/master/blob/aws-poc-1-arch.PNG">
</p>

Phases | Description
------------ | -------------
[Stage 1](https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/readme.md) | Bastion Host Setup with ASG 
[Stage 2](https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%202/readme.md)  | Front-End Tier Setup with ALB & ASG
[Stage 3](https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%203/readme.md)  | 

Highly Redundant/scalable Bastion/3 tier Web Architecture (Backend/Storage Process not included)

Standard Web Tier

Resources created
1x ALB 
1x Target Group
2x Webservers deployed in different AZ
	- gp2
	- t2.micro
1x Bastion host (EC2)

1x Security group
	- default security group
1x priv webserver security group
	- 
2x ASG Template
1x key-pair for asym key login.

EC2 creation
to toggle Auto assign public IP , go to subnet  Modify auto-assign IP Settings (Sholdnt be on for private subnets)

during bastion host setup
- User Agent to attach elastic IP
1) download puttygen to convert pem to ppks
2) upload ppks into pageant 
3) enable agent forwarding in putty

useful URL regarding bastion: https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/
Always have more than one bastion. You should have a bastion in each availability zone (AZ) where your instances are. 
If your deployment takes advantage of a VPC VPN, also have a bastion on premises.

ASG setup:
1) using cloudwatch metric to do step

user data

#!/bin/bash

sudo su

yum update -y
yum install httpd.x86_64
yum start httpd.service
yum enable httpd.service
echo "$(hostname -f)"  > /var/www/html/index.html
