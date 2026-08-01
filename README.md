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

- sudo dnf update -y
- sudo dnf install nginx -y
- sudo systemctl enable nginx
- sudo systemctl start nginx
![status](image1/status-nginx.png)
The Nginx service is active and running on the EC2 instance, confirming that the web server has been installed successfully.

## Step 4 - Verify HTTP Connectivity
Open the EC2 public IP address in a web browser to verify that the default Nginx welcome page is accessible.

![nginx](image1/nginx.png)
The default Nginx web page is successfully displayed through the EC2 public IP, confirming that HTTP connectivity between the client and the EC2 instance is functioning correctly.

# Phase 2 - Understanding TCP Connectivity with Netcat
## Objective
Install Netcat and use it to validate TCP connectivity between the client and the web server. This phase demonstrates how TCP connections behave when a service is listening on a port and when no service is available.

## Step 1 - Install Netcat
- Install Netcat on the Amazon Linux instance.
  
sudo dnf install nc -y
- Verify the installation.

nc -h
![menu](image/nc-menu.png)
The Netcat utility is successfully installed and ready to perform TCP connectivity testing.

## Step 2 - Test TCP Connectivity to Port 80
- Verify that Nginx is accepting TCP connections on port 80.

nc -vz 127.0.0.1 80

nc -vz <Private-IP> 80

nc -vz <Public-IP> 80
![nc](image/nc-127.png)
Netcat successfully establishes a TCP connection to the Nginx web server, confirming that the service is listening and accepting connections on port 80.

## Step 3 - Test an Unused TCP Port
- Attempt to connect to a port where no application is listening.

nc -vz 127.0.0.1 8080
![8080](image/8080.png)
The connection is refused because no application is listening on port 8080, demonstrating the difference between an unavailable service and a firewall restriction.

## Step 4 - Compare the Results
- Observe the difference between the two tests.

| Port | Result             | Explanation                                                          |
| ---- | ------------------ | -------------------------------------------------------------------- |
| 80   | Connected          | Nginx is actively listening on the port.                             |
| 8080 | Connection Refused | The host is reachable, but no application is listening on that port. |

A comparison between successful and failed TCP connection attempts illustrates how Netcat reports different network conditions based on service availability.

## 💡 Lesson Learned
A successful TCP connection does not necessarily mean the application is functioning correctly—it simply confirms that the target service is accepting TCP connections. Likewise, a Connection Refused response indicates that the host is reachable but no application is listening on the specified port.























