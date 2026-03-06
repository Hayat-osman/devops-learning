## 1. Architecture Overview

This project implements a highly available, two-tier web application architecture on AWS. The design uses an Application Load Balancer (ALB) to distribute traffic across two EC2 instances deployed in private subnets across multiple Availability Zones. This ensures the backend instances are isolated from direct internet traffic, enhancing security and resilience.

![Architecture Diagram](/AWS/Assignment2/Architecture-diagram-2.png)

---

## 2. Infrastructure Configuration

### 2.1 VPC and Network Routing

A VPC was configured with public and private subnets to create a secure network boundary.

**Subnets:** 2 Public and 2 Private subnets were created across  ‘eu-west-2a` and `eu-west-2b`.

![VPC Resource Map](/AWS/Assignment2/Screenshot%20/Resourcemap.png)

**Public Route Table:** Routes `0.0.0.0/0` traffic to an **Internet Gateway (IGW)** This table is associated with the public subnets, allowing the ALB to be internet-facing.

![Public Route Table](/AWS/Assignment2/Screenshot%20/Publicroutetable2.png)

**Private Route Table:** Routes `0.0.0.0/0` traffic to a **NAT Gateway**. This table is associated with the private subnets, allowing the EC2 instances outbound internet access for updates without being publicly exposed.

![Private Route Table]
(/AWS/Assignment2/Screenshot%20/Privateroutetable-2.png)

### 2.2 Security Groups

Security Groups act as stateful firewalls to control traffic to the ALB and EC2 instances.

**ALB Security Group:** Allows inbound HTTP traffic on port 80 from anywhere (`0.0.0.0/0`) so users can access the website.

![ALB Security Group Rules](/AWS/Assignment2/Screenshot%20/ALB-SG-NEW.png)

**EC2 Security Group:** Allows inbound HTTP traffic on port 80 **only** from the ALB's security group. This critical rule prevents any traffic from reaching the instances directly.

![EC2 Security Group Rules](/AWS/Assignment2/Screenshot%20/EC2SG-2.png)

### 2.3 EC2 Instances & Automation

Two EC2 instances were launched in the private subnets across two AZs to serve as the backend web servers.

**Automation:** A user-data script was used to automatically install and start the Apache web server on launch. The script also creates a unique `index.html` file on each instance to verify load balancing.

```bash
#!/bin/bash
dnf update -y
dnf install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Hello from instance $(hostname -f)" > /var/www/html/index.html
```

### 2.4 Application Load Balancer (ALB)

An internet-facing ALB was deployed into the public subnets to manage and distribute incoming traffic.

**Listener:** Configured to listen for HTTP traffic on port 80.
**Target Group:** A target group was created to manage the EC2 instances.
**Protocol:** HTTP, Port 80
**Health Checks:** Configured on the root path The ALB uses this to ensure it only sends traffic to healthy instances.
 **Targets:** Both instances were registered as targets.

---

## 3. Validation

The deployment was verified by accessing the ALB's public DNS name.

**Load Balancing:** Refreshing the browser successfully returned content from alternating instances, confirming the ALB was distributing traffic correctly.

![Response from PrivateEC2-1](/AWS/Assignment2/Screenshot%20/PrivateEC2-1.png)

![Response from PrivateEC2-2](/AWS/Assignment2/Screenshot%20/PrivateEC2-2.png)

**Health Status:** The target group status showed both registered instances as **Healthy**, confirming they were responding correctly to the ALB's health checks.

![Healthy Target Group](/AWS/Assignment2/Screenshot%20/Targetgroup.png)