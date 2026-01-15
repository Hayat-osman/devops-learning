Networking Project: Website with Custom Domain

Project Overview

Successfully completed the networking module assignment by purchasing my own domain hayatosman.com, deploying an NGINX web server on AWS EC2, and configuring DNS to make the website accessible via my custom domain. This project demonstrates practical application of networking concepts including DNS resolution, IP addressing, routing, firewall configuration, and HTTP protocols.

Assignment Checklist Completed:

· ✅ Bought a domain: Registered hayatosman.com via Cloudflare
· ✅ Deployed EC2 instance: Launched Ubuntu server on AWS EC2
· ✅ Installed NGINX: Configured and ran NGINX web server
· ✅ Configured DNS: Created A records pointing hayatosman.com to EC2 public IP
· ✅ Allowed HTTP traffic: Security group configured to permit port 80 access
· ✅ Testing successful: Website loads NGINX default page via hayatosman.com
· ✅ Pushed to GitHub: All work documented in this repository

Step-by-Step Implementation

1. Domain Registration on Cloudflare

· Purchased hayatosman.com through Cloudflare registrar
· Configured nameservers to use Cloudflare's DNS

2. AWS EC2 Instance Setup

· Launched an EC2 instance with Ubuntu 22.04 LTS
· Selected t2.micro instance type (free tier eligible)
· Created and downloaded a new key pair for SSH access
· Used default VPC and subnet settings

3. Security Group Configuration

Created a security group with the following inbound rules:

· Port 80 (HTTP): Open to 0.0.0.0/0 for public web access

4. NGINX Installation on Ubuntu

Connected to my EC2 instance via SSH and installed NGINX:

```bash
ssh -i "my-key.pem" ubuntu@ec2-ip-address
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

5. DNS Configuration

Logged into Cloudflare dashboard and created two A records:

· Record 1: Type A | Name: @ | Content: [My EC2 Public IP] | TTL: Auto
· Record 2: Type A | Name: www | Content: [My EC2 Public IP] | TTL: Auto


### EC2 Instance
![EC2 Instance](./networking/EC2.png)
EC2 instance showing public IP, private IP, and running state
### Security Groups
![Security Groups](./networking/EC2%20SECURITY%20DETAILS.png)
Security group allowing HTTP (port 80) and SSH (port 22) access
### DNS Records
![DNS Records](./networking/DNS.png)
Cloudflare DNS with A records pointing to EC2 public IP
### NGINX Status
![NGINX Status](./networking/TERMINAL.png)
Verifying NGINX is running with systemctl status nginx
### Live Website
![Live Website](./networking/WELCOME%20TO%20NGINX.png)
NGINX default page loading at hayatosman.com



What I Learned

1. DNS (Domain Name System)

· DNS is like a phonebook for the internet
· It turns domain names (hayatosman.com) into IP addresses
· A records point directly to IP addresses

2. Ports

· Port 80 is for websites (HTTP)
· Port 22 is for server management (SSH)
· Different services use different ports

3. Cloud Servers

· EC2 is a virtual computer in the cloud
· Public IP = address everyone on internet uses
· Private IP = address only AWS uses internally

4. Security Groups

· Like a firewall for your server
· Controls who can access your server
· I opened port 80 to everyone and port 22 just for me

Challenge I Faced

Problem: My website wasn't loading after setting up DNS

What happened:

· I set up the DNS records for hayatosman.com in Cloudflare
· Waited a few minutes
· Tried to visit hayatosman.com but got an error

How I fixed it:

1. Learned about DNS propagation - it takes time to update everywhere
2. Used nslookup hayatosman.com to check if DNS was working
3. Waited about 5 more minutes
4. Refreshed my browser and it worked!

What I learned: DNS doesn't update instantly. It needs time to spread to all internet servers.

Final Result

Now when I type hayatosman.com in a browser, it shows the "Welcome to NGINX" page from my AWS server!
