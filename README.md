# GreenLeaf Retail — Secure AWS Web Application Infrastructure

## Overview

This project demonstrates the design and deployment of a secure, highly available web application environment in Amazon Web Services.

The environment was built for GreenLeaf Retail and includes:

- IAM least-privilege access
- Custom VPC and subnet design
- Public and private network tiers
- Bastion-host administrative access
- NAT Gateway for private outbound internet access
- Nginx and PHP web servers
- Private MySQL database
- Application Load Balancer
- Multi-Availability-Zone web tier
- EC2 recovery using an Amazon Machine Image
- Application-to-database communication over private IP addressing
- Security Groups using least-privilege rules

The deployed PHP Inventory Management System is accessed through an Application Load Balancer while its MySQL database remains isolated in a private subnet.

---

# Architecture

```text
                         Internet
                            |
                            |
                     Internet Gateway
                            |
                            |
                  Application Load Balancer
                     /                \
                    /                  \
          Public Web Subnet       Public Web Subnet 2
          ca-central-1a           ca-central-1b
                |                       |
          GreenLeaf-Web-1       GreenLeaf-Web-2-Recovery
                |                       |
                |-------- MySQL 3306 ---|
                            |
                            |
                  GreenLeaf-Database
                  Private DB Subnet
                    10.50.1.39
                            |
                            |
                       MySQL Server
                     shop_inventory


Administrative access:

Administrator
     |
     | SSH
     v
GreenLeaf-Bastion
     |
     | SSH
     v
Private Database


Private outbound internet access:

Private Database
     |
     v
Private Route Table
     |
     v
NAT Gateway
     |
     v
Internet Gateway
     |
     v
Internet