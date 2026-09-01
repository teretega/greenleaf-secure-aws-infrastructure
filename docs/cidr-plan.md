# GreenLeaf Retail — CIDR Addressing Plan

## VPC Address Space

The GreenLeaf Retail environment uses the following VPC:

- **VPC CIDR:** `10.50.0.0/20`
- **Total addresses:** 4096

The subnet design was created to meet the required capacity while leaving at least 50% of the VPC address space available for future growth.

AWS reserves five IP addresses in every subnet, so usable capacity was calculated as:

**AWS usable addresses = Total subnet addresses - 5**

---

## Subnet Requirements

| Tier | Required Usable IPs | Selected CIDR | Total IPs | AWS Usable IPs |
|---|---:|---:|---:|---:|
| Public Web | 200 | `/24` | 256 | 251 |
| Bastion | 10 | `/28` | 16 | 11 |
| Private Database | 25 | `/27` | 32 | 27 |

### Public Web Subnet

A `/24` provides:

256 total addresses - 5 AWS reserved addresses = **251 usable addresses**

A `/25` would provide only 123 AWS-usable addresses, so it would not satisfy the requirement for 200 usable addresses.

Selected:

`10.50.0.0/24`

---

### Bastion Subnet

A `/28` provides:

16 total addresses - 5 AWS reserved addresses = **11 usable addresses**

This satisfies the requirement for up to 10 usable addresses.

Selected:

`10.50.1.0/28`

---

### Private Database Subnet

A `/27` provides:

32 total addresses - 5 AWS reserved addresses = **27 usable addresses**

This satisfies the requirement for up to 25 usable addresses.

Selected:

`10.50.1.32/27`

---

## Additional Public Subnet for the Application Load Balancer

During implementation, a second public subnet was created in another Availability Zone so that the Application Load Balancer could span at least two Availability Zones.

Selected:

`10.50.2.0/24`

This subnet was associated with the public route table and used for the second web server and ALB availability-zone mapping.

---

## Address Space Utilization

Initial required subnets:

- Public Web: 256 addresses
- Bastion: 16 addresses
- Private Database: 32 addresses

Total initially allocated:

**304 addresses**

The `/20` VPC contains 4096 addresses, leaving:

**4096 - 304 = 3792 addresses**

This means approximately **92.6% of the original VPC address space remained unallocated during the initial subnet design**, significantly exceeding the requirement to reserve at least 50% for future growth.

The additional `/24` created later for multi-AZ load balancing consumes another 256 addresses while still leaving well over 50% of the VPC available for future expansion.

---

## Final Implemented Subnets

| Subnet | CIDR | Purpose |
|---|---|---|
| GreenLeaf-Public-Web-Subnet | `10.50.0.0/24` | Public web tier / ALB |
| GreenLeaf-Bastion-Subnet | `10.50.1.0/28` | Secure administrative access |
| GreenLeaf-Private-DB-Subnet | `10.50.1.32/27` | Private MySQL database |
| GreenLeaf-Public-Web-Subnet-2 | `10.50.2.0/24` | Second AZ for ALB and web tier |