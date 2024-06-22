# Project :Deployment of Blog Page Application (Django) on AWS: Utilizing ALB, Auto Scaling, S3, RDS, VPC, Lambda, DynamoDB, CloudFront, and Route 53

## Description

The Blog Page Application aims to deploy a blog application written using the Django Framework on AWS Cloud Infrastructure. This infrastructure includes an Application Load Balancer with an Auto Scaling Group of Elastic Compute Cloud (EC2) Instances and a Relational Database Service (RDS) on a defined VPC. RDS credentials are retrieved via SSM parameters. Additionally, CloudFront and Route 53 services are used to manage traffic securely. Users can upload pictures and videos to their blog pages, which are stored in an S3 Bucket. The object list of the S3 Bucket containing media files is recorded on a DynamoDB table. The DevOps team is responsible for deploying this application in a production environment.

## Problem Statement

- Your company has completed a project to serve as a Blog web application in an isolated VPC environment. You and your colleagues have started working on deploying this application. The Developer team has developed the application, and you are tasked with deploying it in the production environment.

- The application allows users to write their own blog pages, with user registration data stored in a MySQL database on AWS RDS, and pictures or videos stored in an S3 bucket. The object list of the S3 bucket is recorded in a DynamoDB table. 

- You are requested to:

1. Push your program to the project repository on GitHub and pull it into the web servers in the production environment on AWS Cloud.

2. Configure the infrastructure using the following specifications.

### Specifications

#### VPC Configuration

- VPC has two Availability Zones (AZs), each with 1 public and 1 private subnet.

- VPC includes an Internet Gateway.

- One of the public subnets hosts a NAT Instance.

- A Bastion host can be created on a Public subnet, or the NAT instance can be used as a Bastion host.

- Managed private and public route tables with appropriate routing policies and subnet associations.

#### Application Load Balancer (ALB)

- ALB with an Auto Scaling Group of Ubuntu 22.04 EC2 Instances.

- Auto Scaling Group configuration:

  - Uses all Availability Zones.

  - Desired capacity: 1

  - Minimum size: 1

  - Maximum size: 3

  - Health check grace period: 90 seconds

  - Health check type: ELB

  - Scaling Policy: Target Tracking Policy

    - Average CPU utilization: 70%

    - Warm-up time: 200 seconds

    - Email notifications for launch, terminate, failure events

- ALB Security Group allows HTTP (80) and HTTPS (443) connections from anywhere.

- Certificate created using AWS Certificate Manager for secure HTTPS connections.

- ALB redirects HTTP traffic to HTTPS.

- Target Group health check protocol is HTTP.

#### Launch Template for EC2 Instances

- Configured to prepare Django environment on EC2 instance based on developer notes.

- Downloads the project from the GitHub repository.

- Installs requirements using requirements.txt.

- Deploys the Django application on port 80.

- Security Group allows HTTP (80) and HTTPS (443) from ALB Security Group, and SSH (22) connections from anywhere

- Instance type: t2.micro.`

- Tagged as AWS Capstone Project.

- Attached with S3 full access role.

#### RDS Configuration

- Instance type: db.t2.micro.

- Database engine: MySQL 8.0.33.

- Endpoint configured in the application's settings file.

- Database credentials retrieved from SSM Parameter Store.

#### SSM Parameters

- Database master password: /yourname/capstone/password (SecureString)

- Database username: /yourname/capstone/username (SecureString)

- GitHub Token: /yourname/capstone/token (SecureString)

#### CloudFront Configuration

- Points to ALB with HTTPS-only origin protocol policy.

- Viewer Protocol Policy: Redirect HTTP to HTTPS.

- Allows all HTTP methods.

- Forwards all cookies.

- Uses the same ACM certificate as ALB.

- Cache settings include specific headers.

#### Route 53 Configuration

- Secure connection (HTTPS).

- Uses failover routing policy:

    - Primary connection: CloudFront

    - Secondary connection: static website in another S3 bucket.

    - Healthcheck verifies CloudFront's health.

#### S3 Buckets

- First S3 Bucket:

  - Created in the same region as the VPC.

  - Configured with VPC Endpoint for internal traffic.

  - Bucket name configured in the application settings file.

- Second S3 Bucket:

  - Used for failover with a static website indicating "the page is under construction".

#### DynamoDB and Lambda Configuration

- DynamoDB table with primary key id.

- Lambda function:

  - Python 3.8

  - Triggered by S3 events.

  - Full access to S3 and DynamoDB.

  - NetworkAdministrator policy attached.

  - 30-second timeout.

  - S3 event created in the first S3 bucket to trigger the Lambda function.

#### Deployment Steps

1. Create the VPC and its components.

2. Create Security Groups.

3. Create the RDS instance.

4. Create the S3 Buckets.

5. Clone the project repository.

6. Prepare the GitHub repository.

7. Prepare a userdata script for the Launch Template.

8. Update the settings file for RDS and S3 configurations.

9. Create the NAT Instance.

10. Create the Launch Template and IAM role.

11. Create the SSL certificate.

12. Create the ALB and Target Group.

13. Create the Auto Scaling Group.

14. Set up CloudFront.

15. Configure Route 53.

16. Create the DynamoDB Table.

17. Create the Lambda function and S3 event trigger.


## Project Skeleton 

```text
Deploying-Django-Blog-on-AWS-ALB-Auto-Scaling-S3-RDS-VPC-Lambda-DynamoDB-CloudFront-Route53 (folder)
|
|----Readme.md               
|----src (folder)
|----S3_Static_Website (folder)          
|----userdata.sh             
|----requirements.txt        
|----lambda_function.py      
|----developer_notes.txt
|----capstone.jpg   
```

## Expected Outcome

### Key Topics Covered;

- Bash scripting

- AWS EC2 Launch Template Configuration

- AWS VPC Configuration

- AWS EC2 ALB and Auto Scaling Group Configuration

- AWS RDS Configuration

- IAM Roles Configuration

- S3 Configuration

- DynamoDB and Lambda Configuration

- AWS CloudFront and Route 53 Configuration

- Git & GitHub for Version Control


## Resources

- [Python Django Framework](https://www.djangoproject.com/)

- [Python Django Example](https://realpython.com/get-started-with-django-1/)

- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)
