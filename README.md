# Understanding-TCP-IP-and-Firewall-Behavior-in-AWS-EC2
## Project Overview
This project demonstrates how TCP/IP communication works in AWS by deploying an Nginx web server on an Amazon EC2 instance and validating network connectivity using Netcat and Nmap. It also explores how AWS Security Groups (stateful firewall) and Network ACLs (stateless firewall) affect network traffic through a series of hands-on experiments.

Throughout the project, multiple validation methods were used to distinguish between application-level, operating system-level, and network-level connectivity. This provides a practical understanding of how AWS networking components interact to secure EC2 instances.

## Prerequisites

Before starting this project, ensure you have:
- An active AWS account.
- Basic understanding of TCP/IP and common network ports.
- A VPC with Internet Gateway connectivity.
- An EC2 Key Pair for SSH access.
- A local SSH client (PowerShell or Terminal).
- Internet access for downloading packages on EC2.

# Phase 1 - Environment Setup
## Step 1 - Launch an Amazon EC2 Instance
- Create an Amazon EC2 instance using Amazon Linux 2023 and t3.micro.
- Place the instance in a public subnet, assign a public IPv4 address,
- configure a Security Group that allows SSH (TCP 22) from your current public IP
- and HTTP (TCP 80) from the internet.
![ec2](image1/ec2-net.png)

The EC2 instance is successfully deployed in a public subnet with a public IPv4 address. The associated Security Group allows SSH access for administration and HTTP access for web traffic.

## Step 2 - Connect to the EC2 Instance via SSH
Connect to the EC2 instance using your local terminal (PowerShell or Terminal) and verify that remote access is working correctly.

ssh -i your-key.pem ec2-user@your public IP
![login](image1/login.png)

The EC2 instance is successfully accessed through SSH, confirming that the network configuration and Security Group allow secure remote administration.

## Step 3 - Install and Configure Nginx
Update the package repository, install Nginx, enable the service at boot, and start the web server.

sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
![status](image1/status-nginx.png)

The Nginx service is active and running on the EC2 instance, confirming that the web server has been installed successfully.

## Step 4 - Verify HTTP Connectivity
Open the EC2 public IP address in a web browser to verify that the default Nginx welcome page is accessible.

![nginx](image1/nginx.png)

The default Nginx web page is successfully displayed through the EC2 public IP, confirming that HTTP connectivity between the client and the EC2 instance is functioning correctly.

# Phase 2

