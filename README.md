    # Networking Assignment: Domain + EC2 + DNS set up 

## Assignment Overview

Successfully completed the networking module assignment by purchasing my own domain hayatosman.com, deploying an NGINX web server on AWS EC2, and configuring DNS to make the website accessible via my custom domain. This project demonstrates practical application of networking concepts including DNS resolution, IP addressing, routing, firewall configuration, and HTTP protocols.



### 1. Domain Registration on Cloudflare

· Purchased hayatosman.com through Cloudflare registrar

### 2. AWS EC2 Instance Setup

· Launched an EC2 instance with Ubuntu 
· Selected t2.micro instance type
· Created and downloaded a new key pair for SSH access

### 3. Security Group Configuration

Created a security group with the following inbound rules:

· Port 80 (HTTP): Open to 0.0.0.0/0 for public web access

### 4. NGINX Installation on Ubuntu

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
![EC2 Instance](./networking/INSTANCE.png)
EC2 instance showing public IP, private IP, and running state
### Security Groups
![Security Groups](./networking/SECURITY.png)
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



## What I Learned

This assignment helped me understand how the internet connects different pieces:

1. **How DNS works**: I learned that DNS acts like a phonebook. When someone types `hayatosman.com`, DNS servers are responsible for finding and returning the correct IP address for my server.
2. **Servers and IP Addresses**: I now know that a cloud server (like AWS EC2) has two IPs:
- A **Public IP** that anyone on the internet can use to find it.
- A **Private IP** that only other services inside AWS use to talk to it privately.
3. **Security with Firewalls**: I configured a "Security Group" on AWS, which is like a firewall. I set it to allow website traffic on port 80 and restrict server management to my IP on port 22.
4. **The Full Journey**: I saw the complete path a website request takes: `Browser -> DNS -> My EC2 Server -> NGINX -> Website Page`.

## Challenge I Faced

The main challenge was that my website at `hayatosman.com` didn't load right after I set up the DNS. I figured out it was because of DNS propagation—the time it takes for the updated address to spread online. I checked that the server itself was working using its IP address, then waited a few minutes. After that, my custom domain worked perfectly.

