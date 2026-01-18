# Step 1: EC2 Instance Setup

An EC2 instance was launched with the following configuration:

	• Operating System: Amazon Linux 2023 (x86_64, kernel 6.1)
	• Instance Type: t3.micro
	• Region: eu-west-2 (London)
	• Public IPv4 Address: Enabled

Image1
Image2
	
## Key Pair Configuration
An RSA key pair (webkeys.pem) was created for secure SSH access.
File permissions were restricted so that only the owner could read the key:

chmod 400 webkeys.pem

Image3

# Step 2: Security Group Configuration
A security group was configured to allow required inbound traffic while keeping the instance secure.
Inbound Rules
Type	Protocol	Port	Source
SSH	TCP	22	My IP
HTTP	TCP	80	0.0.0.0/0
			
Security groups in AWS are stateful, meaning outbound traffic is allowed automatically once inbound traffic is permitted.

Image4

# Step 3: Instance Health Check
After launching the instance, AWS status checks were monitored.
Once the instance passed 2/2 status checks, it confirmed that both the AWS infrastructure and the operating system were healthy and reachable.
Only after these checks passed was the instance accessed via SSH and HTTP.

Image5

# Step 4: SSH Access and NGINX Setup
The instance was accessed securely using SSH:

ssh -i ~/.ssh/webkeys.pem ec2-user@<EC2_PUBLIC_IPV4>
An initial check showed that NGINX was not installed:

sudo systemctl status nginx

## Manual NGINX Installation (Amazon Linux 2023)
Since User Data did not install NGINX successfully, it was installed manually using dnf:

sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx

## Verification
NGINX was verified to be running and listening on port 80:

sudo systemctl status nginx
sudo ss -tulnp | grep :80
At this stage, the NGINX default welcome page was accessible via the EC2 public IP.

# Step 5: DNS Configuration (Cloudflare)
To expose the application using a custom domain, DNS records were configured in Cloudflare.

The Cloudflare proxy was set to DNS only (grey cloud).

Enabling the proxy initially caused a 522 Connection Timed Out error, as the EC2 instance was not configured to accept proxied traffic.

Image6

# Step 6: Final Verification
After DNS propagation completed, the deployment was verified:
	• The NGINX welcome page loaded successfully via the EC2 public IP
	• The same page loaded successfully via the custom domain (dyemo.co.uk)

Image6

Image7
