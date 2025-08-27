🚀 AWS Project: Migrate Physical servers to AWS Cloud, Multi-Tier Application Deployment with High Availability & Monitoring 

Region Selection - Selected North Virginia (us-east-1) as deployment region (widely used and cost-effective).

1. VPC & Networking Setup

Created a VPC with IPv4 CIDR block 10.0.0.0/16.

Subnets:

Public Subnet A: 10.0.1.0/24 in us-east-1a

Public Subnet B: 10.0.2.0/24 in us-east-1b

Private Subnet C: 10.0.3.0/24 in us-east-1c

Enabled Auto-assign Public IPv4 for the 2 public subnets.

Created an Internet Gateway and attached it to the VPC.

Created a Route Table:

Added route 0.0.0.0/0 → Internet Gateway.

Associated it with public subnets.

Created NAT Gateway with an Elastic IP in public subnet, attached it to private subnet for outbound internet access.

Enabled DNS hostnames in VPC settings.

2. Security & Access

Created Security Groups:

WebServer-SG: Allow HTTP/HTTPS from internet, SSH only from Bastion.

DB-SG: Allow MySQL traffic only from private subnets or Bastion.

Bastion-SG: Allow SSH from my IP.

Created IAM Role with S3 full access for VPC Flow Logs.

3. Storage & Monitoring

Created S3 bucket to store VPC Flow Logs.

Created EFS file system and mounted it with EC2 instances.

Enabled VPC Flow Logs and sent logs to S3.

Created CloudWatch Dashboard:

Monitoring Auto Scaling Group CPU Utilization.

Set up alarms for scale-in and scale-out events.

4. Compute & Scaling

Created a Launch Template for WebServer instances (Amazon Linux 2, Apache/Nginx installed, EFS mounted).

Created an Auto Scaling Group across multiple Availability Zones for high availability.

Created an Application Load Balancer:

Listener: HTTP 80

Target Group: WebServers in Auto Scaling Group.

5. Bastion & Database Setup

Created 2 Bastion Servers (us-east-1a & us-east-1b) in public subnet for secure SSH access.

Created Database Server (RDS/MySQL) in private subnet.

Configured connection: Bastion → Database.

Verified internet connectivity from Bastion by pinging google.com.

6. Notifications & DNS

Created SNS Topic for notifications and subscribed via email.

Configured Route 53 DNS with Failover Routing Policy for Load Balancer endpoints.

7. Testing & Validation

Connected to Bastion-A via SSH, then connected to DB server.

Verified Bastion-B connectivity as backup.

Accessed WebServers via Load Balancer DNS, confirmed scaling works.

Verified logs in S3, monitoring in CloudWatch, and notifications in SNS.

🏆 Key Points to Explain in Interview

Architecture Goal: Designed a highly available, secure, and scalable 3-tier infrastructure.

Best Practices Used:

Multi-AZ deployment

Public & private subnet separation

Bastion host for secure SSH

NAT Gateway for private internet access

Auto Scaling with Load Balancer

Monitoring via CloudWatch

Centralized logging with VPC Flow Logs in S3

Notifications with SNS

Business Value: Ensures scalability, security, high availability, and monitoring for production workloads.
