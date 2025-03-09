

# Hosting a Static Website on AWS

This repository contains the necessary configurations, scripts, and diagrams for deploying a static HTML web application on AWS. The project demonstrates the use of several AWS services to ensure high availability, security, and scalability for hosting a static website on EC2 instances. 

## Architecture Overview

The architecture leverages multiple AWS services for building a secure and scalable environment for hosting the static website. Below is a high-level overview of the setup:

### Key Components:
1. **Virtual Private Cloud (VPC)**: Configured with both public and private subnets across two availability zones (AZs).
2. **Internet Gateway (IGW)**: Facilitates connectivity between the VPC instances and the Internet.
3. **Security Groups**: Used as a network firewall to control inbound and outbound traffic.
4. **Application Load Balancer (ALB)**: Distributes incoming traffic to EC2 instances across multiple availability zones.
5. **Auto Scaling Group (ASG)**: Automatically adjusts the number of EC2 instances based on traffic and scaling policies.
6. **NAT Gateway**: Provides internet access to EC2 instances within private subnets.
7. **EC2 Instances**: The web servers that host the static website.
8. **AWS Certificate Manager**: Manages SSL/TLS certificates to secure the application with HTTPS.
9. **Simple Notification Service (SNS)**: Sends notifications about scaling events and other activities within the Auto Scaling Group.
10. **Route 53**: Used to register a domain and configure DNS records for routing traffic to the website.

## Prerequisites

- AWS account with permissions to create EC2 instances, VPC, Route 53, and other AWS resources.
- Domain registered (if needed for DNS configuration).
- Basic understanding of AWS services such as EC2, VPC, ALB, and Auto Scaling.
  
## Technologies and AWS Services Used

- **Amazon EC2**: To host the static HTML files on the server.
- **Amazon VPC**: For network setup and segmentation of public and private subnets.
- **Amazon Route 53**: For DNS configuration and domain registration.
- **Amazon ALB (Application Load Balancer)**: To distribute web traffic across EC2 instances.
- **Amazon Auto Scaling**: To ensure the availability and scalability of the web application.
- **Amazon SNS**: For monitoring and notifications about scaling events.
- **AWS Certificate Manager**: For securing the application with SSL/TLS certificates.

## Project Setup

### 1. **Clone the Repository**

Clone this repository to access the deployment scripts, configuration files, and reference diagrams.

```bash
git clone https://github.com/lynkolds/AWS-Static-Website-Deployment.git
cd AWS-Static-Website-Deployment
```

### 2. **Launch EC2 Instance**

To deploy the static website, launch an EC2 instance in a public subnet with an Amazon Linux AMI, allowing HTTP and SSH access.

1. Log into the AWS Management Console and navigate to EC2.
2. Launch a new EC2 instance using the Amazon Linux 2 AMI.
3. Select a suitable instance type (e.g., t2.micro for testing).
4. Configure the instance in a public subnet.
5. Assign a public IP to the instance.
6. Open necessary ports (HTTP 80, SSH 22) in the security group.

### 3. **Execute the Deployment Script**

Once the EC2 instance is running, SSH into the instance and execute the provided script for setting up the web server.

```bash
# SSH into the EC2 instance
ssh -i your-key.pem ec2-user@your-ec2-public-ip

# Switch to root user
sudo su

# Update all installed packages to the latest version
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change to the Apache web root directory
cd /var/www/html

# Install Git to clone the repository
yum install -y git

# Clone the project GitHub repository
git clone https://github.com/lynkolds/AWS-Static-Website-Deployment.git

# Copy the web files to the Apache web root
cp -R AWS-Static-Website-Deployment/. /var/www/html/

# Clean up by removing the cloned repository
rm -rf AWS-Static-Website-Deployment

# Enable Apache to start at boot
systemctl enable httpd

# Start the Apache HTTP Server
systemctl start httpd
```

### 4. **Verify Website Availability**

Once the EC2 instance is configured and Apache is running, you can access the website by navigating to the public IP address of your EC2 instance in a browser. If you are using Route 53 for DNS configuration, use the domain name instead of the IP.

Example URL:
```
http://your-ec2-public-ip
```
Or, if configured with Route 53 DNS:
```
http://yourdomain.com
```

### 5. **Set Up Load Balancer and Auto Scaling**

To scale the website for high availability, create an **Application Load Balancer (ALB)** and an **Auto Scaling Group (ASG)** that automatically scales your EC2 instances based on traffic. The ALB will distribute traffic evenly across the instances in the ASG.

1. Create an **ALB** in a public subnet.
2. Set up **Target Groups** for the EC2 instances.
3. Create an **Auto Scaling Group** to ensure that instances scale based on demand.
4. Configure **SNS notifications** for auto-scaling activities.

### 6. **Domain Name and DNS Configuration**

To register your domain and set up DNS records:

1. Go to **Route 53** and register your domain name (if not done already).
2. Set up DNS records to point to the **Application Load Balancer**.

### 7. **Secure the Website with SSL/TLS**

To secure the communications between the client and server, configure **AWS Certificate Manager (ACM)** to issue an SSL certificate for your domain and bind it to your Application Load Balancer.

## Monitoring and Alerts

- **SNS Notifications**: Configure **SNS** to alert you when the **Auto Scaling Group** adds or removes EC2 instances.
- **CloudWatch Metrics**: Use **CloudWatch** to monitor the health and performance of your EC2 instances, ALB, and other resources.

## Troubleshooting

- **Website Not Accessible**: Ensure that the EC2 instance has a public IP, the security group allows HTTP traffic, and the ALB is configured correctly.
- **Scaling Issues**: If the Auto Scaling Group is not scaling properly, check the scaling policies, CloudWatch metrics, and EC2 instance health.

## Conclusion

This project demonstrates the use of several AWS services to host a static website on EC2 instances with high availability, scalability, and security. By using a combination of VPC, ALB, Auto Scaling, Route 53, and ACM, you can build a robust, production-ready environment for hosting web applications.

Feel free to use this setup for deploying your own static websites on AWS and extend it as needed. 

For more detailed information, refer to the diagrams and script files in this repository.

---

If you encounter any issues or have questions, please feel free to raise an issue on this repository.
