# Stage 1: VPC & Subnet Configuration
- 
## Introduction
Configure a VPC network hosting public and private subnet on different availability zones.


## Blueprint
<p align="center">
  <img src="https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/Stage-1-vpc-subnet-arch.PNG">
</p>

## VPC Configuration

1) In VPC services page, navigate to "Your VPCs" page and click on "Create VPC"
2) Tag your VPC resource with a name
3) set the IPv4 CIDR block IP to be "10.0.0.0/16"
4) Extra Options:
IPv6 CIDR block: For assignment of IPv6 Network Address
Tenancy: Dedicated tenancy ensures all EC2 instances that are launched in a VPC run on hardware that's dedicated to a single customer 
5) Click "Create VPC" 


<p align="center">
  <img src="https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-1.PNG">
</p>

### DNS Hostnames assignment
By enabling "DNS Hostnames", EC2 Instances with public IP created in this VPC will automatically be assigned a hostname.

6) Right click the created VPC, Click on "Edit DNS Hostnames"
<p align="center">
  <img src="https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-2.PNG">
</p>

7) Click on "Enable" Checkbox and then save the changes.
<p align="center">
  <img src="https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-3.PNG">
</p>

## Subnet Configuration

## 