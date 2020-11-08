# Stage 1: VPC & Subnet Configuration
- <a href=#introduction>Introduction</a>
- <a href=#blueprint>Blueprint</a>
- <a href=#vpc-configuration>VPC Configuration</a>
- <a href=#subnet-configuration>Subnet Configuration</a>

## Introduction
Configure a VPC network hosting public and private subnet on different availability zones.


## Blueprint
<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/Stage-1-vpc-subnet-arch.PNG>
</p>

## VPC Configuration

1) In **VPC** services page, navigate to **Your VPCs** page and click on **Create VPC**
2) Tag **VPC resource** with a **name** <3-tier-vpc>
3) set **IPv4 CIDR block IP** to **10.0.0.0/16**
4) Extra Options:
**IPv6 CIDR block:** For assignment of IPv6 Network Address
**Tenancy:** Dedicated tenancy ensures all EC2 instances that are launched in a VPC run on hardware that's dedicated to a single customer 
5) Click **Create VPC** 

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-1.PNG>
</p>

### DNS Hostname assignment
By enabling **DNS Hostnames**, EC2 Instances created with public IP within this VPC will automatically be assigned a hostname.

6) Right click the created VPC, Click on **Edit DNS Hostnames**
<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-2.PNG>
</p>

7) Click **Enable** Checkbox and save the changes.
<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-3.PNG>
</p>

## Subnet Configuration

**Subnets to be configured:**
**Name Tag:** private-subnet-1 **IPv4 CIDR:** 10.0.0.0/24 **Availability Zone:** ap-southeast-1a
**Name Tag:** private-subnet-2 **IPv4 CIDR:** 10.0.1.0/24 **Availability Zone:** ap-southeast-1b
**Name Tag:** public-subnet-1 **IPv4 CIDR:** 10.0.2.0/24 **Availability Zone:** ap-southeast-1a
**Name Tag:** public-subnet-2 **IPv4 CIDR:** 10.0.3.0/24 **Availability Zone:** ap-southeast-1b

1) In **VPC** services page, navigate to **Subnets** page and click on **Create subnet**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-4.PNG>
</p>

2) Tag **Subnet** with a **name < private-subnet-1 >**
3) Select **VPC** <3-tier-vpc>
4) Select **Availability Zone** <ap-southeast-1a>
5) set **IPv4 CIDR block IP** to **10.0.0.0/24**
6) Click **Create**

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-5.PNG>
</p>

7) Repeat for the other 3 subnets

<p align=center>
  <img src=https://github.com/ravensp93/aws-three-tier-web/blob/master/Stage%201/blob/stage-1-pic-6.PNG>
</p>
## 