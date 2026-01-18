## Overview

This project demonstrates deploying a Linux-based web server on AWS using EC2, installing and running NGINX, and exposing the application to the internet via a custom domain using Cloudflare DNS.
The project focuses on core cloud concepts such as instance provisioning, security groups, service verification, DNS configuration, and troubleshooting.

### Issues & Resolutions

- Instance launch failure: Incorrect AMI (SQL Server image) selected → Switched to Amazon Linux 2023 (x86_64).

- No access on port 80: NGINX not installed or running → Installed and started NGINX.

- SSH permission denied: Wrong username used → Connected with ec2-user.

- Cloudflare Error 522: Proxy enabled too early → Set DNS records to DNS only.

### Key Learnings

- AMI and instance type compatibility matters.

- Open security group ports do not guarantee services are running.

- SSH usernames depend on the OS.

- Always test via public IP before configuring DNS.
