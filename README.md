# Project Blog Page Application (Django) deployed on AWS Application Load Balancer with Auto Scaling, S3, Relational Database Service(RDS), VPC's Components, Lambda, DynamoDB and Cloudfront with Route 53

## Description

The Blog Page Application aims to deploy a blog application as a web application written by Django Framework on AWS Cloud Infrastructure. This infrastructure has an Application Load Balancer with Auto Scaling Group of Elastic Compute Cloud (EC2) Instances and Relational Database Service (RDS) on defined VPC. Also, The Cloudfront and Route 53 services are located in front of the architecture and manage the traffic securely. The user is able to upload pictures and videos on their own blog page and these are kept on S3 Bucket. Cloud engineers will create this architecture.

## Problem Statement

![Project_004](capstone.jpg)

- Your company has recently ended up with a project that aims to serve as a Blog web application in an isolated VPC environment. You and your colleagues have started to work on the project. Your Developer team has developed the application and you are going to deploy the app in a production environment.

- The application is coded by the Fullstack development team and given to you as the DevOps team. The app allows users to write their own blog page to whom user registration data should be kept in a separate MySQL database in AWS RDS service and pictures or videos should be kept in the S3 bucket. The object list of the S3 Bucket containing movies and videos is recorded on the DynamoDB table. 

- The web application will be deployed using the Django framework.

- The Web Application should be accessible via web browser from anywhere in secure.

- Please push your program to the project repository on GitHub. You are going to pull it into the web servers in the production environment on AWS Cloud. 

In the architecture, you can configure your infrastructure using the following,

  - The application stack should be created with new AWS resources.

  - Specifications of VPC:

    - VPC has two AZs and every AZ has 1 public and 1 private subnets.

    - VPC has an Internet Gateway

    - One of the public subnets has a NAT Instance.

    - You might create a new instance as a Bastion host on the Public subnet or you can use a NAT instance as a Bastion host.

    - There should be managed private and public route tables.

    - Route tables should be arranged regarding routing policies and subnet associations based on public and private subnets.

  - You should create an Application Load Balancer with Auto Scaling Group of Ubuntu 18.04 EC2 Instances within the created VPC.

  - You should create an RDS instance within one of the private subnets on the created VPC and configure it on the application.

  - The Auto Scaling Group should use a Launch Template in order to launch instances needed and should be configured to;

    - use all Availability Zones on created VPC.

    - set desired capacity of instances to  ` 2`

    - set minimum size of instances to  ` 2`

    - set maximum size of instances to  ` 4`

    - set health check grace period to  ` 90 seconds

    - set health check type to  ` ELB`

    - Scaling Policy --> Target Tracking Policy

      - Average CPU utilization (set Target Value ` %70`)

      - seconds warm up before including in metric ---> `200`

      - Set notification to your email address for launch, terminate, fail to launch, fail to terminate instance situations

  - ALB configuration;
    
    - Application Load Balancer should be placed within a security group that allows HTTP (80) and HTTPS (443) connections from anywhere. 
    
    - Certification should be created for secure connection (HTTPS) 
      - To create a certificate, AWS Certificate Manager can be utilized.

    - ALB redirects to traffic from HTTP to HTTPS

    - Target Group
      - Health Check Protocol is going to be HTTP

  - The Launch Template should be configured to;

    - Prepare Django environment on EC2 instance based on Developer Notes,

    - Download the "fatihyildiz_aws_capstone" folder from the GitHub repository,

    - Install the requirements using requirements.txt in 'frank_aws_webserver' folder

    - Deploy the Django application on port 80.

    - Launch Template only allows HTTP (80) and HTTPS (443) ports coming from ALB Security Group and SSH (22) connections from anywhere.

    - EC2 Instances type can be configured as `t2.micro`.

    - The instance launched should be tagged `frankyildiz AWS Capstone Project`

    - Since Django App needs to talk with S3, S3 full access role must be attached EC2s. 

  - For RDS Database Instance;
  
    - Instance type can be configured as `db.t2.micro`

    - Database engine can be `MySQL` with version of `8.0.20`.

    - The RDS endpoint should be addressed within settings file of blog application which is explained in the developer notes.

    - Please read carefully "Developer notes" carefully to manage RDS sub settings.

  - CloudFront should be set as a cache server that points to Application Load Balance with the following configurations;

    - The CloudFront distribution should communicate with ALB securely.

    - Origin Protocol policy can be selected as `HTTPS only`.

    - Viewer Protocol Policy can be selected as `Redirect HTTP to HTTPS`

  - As cache behavior;

    - GET, HEAD, OPTIONS, PUT, POST, PATCH, and DELETE methods should be allowed.

    - Forward Cookies must be selected All.

    - Newly created ACM Certificate should be used for securing connections. (You can use the same certificate with ALB)

  - Route 53 

    - Connection must be secure (HTTPS). 

    - Your hostname can be used to publish the website.

    - Failover routing policy should be set while publishing the application
      
      - Primary connection is going to be Cloudformation

      - Secondary connection is going to be a static website placed in another S3 bucket. This S3 bucket has just basic static website that has a picture that says "the page is under construction" given files within S3_static_Website folder

      - Healthcheck should check If Cloudfront is healthy or not. 

  - As S3 Bucket

    - First S3 Bucket

      - It should be created within the Region that you created VPC

      - Since development team doesn't prefer to expose traffic between S3 and EC2s on internet, Endpoint should be set on created VPC. 

      - S3 Bucket name should be addressed within configuration file of blog application that is explained developer notes.
    
    - Second S3 Bucket 
      
      - This Bucket is going to be used for failover scenario. It has just a basic static website that has a picture said "The page is under construction"

  - To write the objects of S3 on DynamoDB table
    
    - Lambda Function 

      - Lambda function is going to be Python 3.8

      - Python Function can be found in GitHub repo

      - S3 event is set as trigger

      - Since Lambda needs to talk S3 and DynamoDB and to run on created VPC, S3, DynamoDB full access policies and NetworkAdministrator policy must be attached to it

      - `S3 Event` must be created first S3 Bucket to trigger Lambda function 

    - DynamoDB Table

      - Create a DynamoDB table which has primary key that is `id`

      - Created DynamoDB table's name should be placed on Lambda function.


## Project Skeleton 

```text
fatihyildiz_blog_proj (folder)
|
|----Readme.md               # Given to the DevOps team (Definition of the project)
|----src (folder)            # Given to the DevOps team (Django Application)
|----requirements.txt        # Given to the DevOps team (txt file)
|----lambda_function.py      # Given to the DevOps team (python file)
|----developer_notes.txt     # Given to the DevOps team (txt file)
```

### At the end of the project, the following topics are to be covered;

- Bash scripting

- AWS EC2 Launch Template Configuration

- AWS VPC Configuration
  - VPC
  - Private and Public Subnets
  - Private and Public Route Tables
  - Managing routes
  - Subnet Associations
  - Internet Gateway
  - NAT Gateway
  - Bastion Host
  - Endpoint

- AWS EC2 Application Load Balancer Configuration

- AWS EC2 ALB Target Group Configuration

- AWS EC2 ALB Listener Configuration

- AWS EC2 Auto Scaling Group Configuration

- AWS Relational Database Service Configuration

- AWS EC2, RDS, ALB Security Groups Configuration

- IAM Roles configuration

- S3 configuration

- Static website configuration on S3

- DynamoDB Table configuration

- Lambda Function configuration

- Get a Certificate with AWS Certification Manager Configuration

- AWS Cloudfront Configuration

- Route 53 Configuration

- Git & GitHub for Version Control System

### At the end of the project, students will be able to;

- Construct a VPC environment with whole components like public and private subnets, route tables and managing their routes, Internet Gateway, and NAT Instance. 

- Apply web programming skills, importing packages within the Python Django Framework

- Configure connection to the `MySQL` database.

- Demonstrate bash scripting skills using the `user data` section within launch template to install and set up the Blog web application on EC2 Instance.

- Create a Lambda function using S3, Lambda and DynamoDB table.

- Demonstrate their configuration skills of AWS VPC, EC2 Launch Templates, Application Load Balancer, ALB Target Group, ALB Listener, Auto Scaling Group, S3, RDS, CloudFront, Route 53.

- Apply git commands (push, pull, commit, add, etc.) and GitHub as a Version Control System.

## Steps to Solution
  
- Step 1: Create dedicated VPC and whole components

- Step 2: Create Security Groups (ALB ---> EC2 ---> RDS)

- Step 3: Create RDS

- Step 4: Create two S3 Buckets and set one of these as a static website.

- Step 5: Download or clone the project definition from `frankyildiz` repo on Github 

- Step 6: Prepare your Github repository 

- Step 7: Prepare user data to be utilized in the Launch Template

- Step 8: Write RDS, S3 in the settings file given by Fullstack Developer team  

- Step 9: Create a NAT Instance in a Public Subnet

- Step 10: Create a Launch Template and IAM role for it

- Step 11: Create certification for a secure connection

- Step 12: Create ALB and Target Group

- Step 13: Create an Autoscaling Group with a Launch Template

- Step 14: Create Cloudfront in front of ALB

- Step 15: Create Route 53 with Failover settings

- Step 16: Create DynamoDB Table

- Step 17-18: Create Lambda function 

- Step 17-18: Create an S3 Event and set it as a trigger for Lambda Function

## Notes

- RDS database should be located in a private subnet. just EC2 machines that have an ALB security group can talk with RDS.

- RDS is located in private groups and only EC2s can talk with it on port 3306

- ALB is located public subnet and it redirects traffic from http to https

- EC2s are located in private subnets and only ALB can talk with them


## Resources

- [Python Django Framework](https://www.djangoproject.com/)

- [Python Django Example](https://realpython.com/get-started-with-django-1/)

- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)
