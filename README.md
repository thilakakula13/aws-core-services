# AWS Core Services: Complete Guide from Basic to Advanced

A comprehensive guide covering essential AWS services with hands-on examples, best practices, and real-world use cases. Perfect for beginners starting their cloud journey and professionals preparing for AWS certifications.

---

## 📚 Table of Contents

### Part 1: AWS Fundamentals
1. [Introduction to AWS Cloud](#1-introduction-to-aws-cloud)
2. [AWS Global Infrastructure](#2-aws-global-infrastructure)
3. [IAM - Identity and Access Management](#3-iam-identity-and-access-management)
4. [AWS CLI and SDK Setup](#4-aws-cli-and-sdk-setup)

### Part 2: Compute Services
5. [EC2 - Elastic Compute Cloud](#5-ec2-elastic-compute-cloud)
6. [Lambda - Serverless Computing](#6-lambda-serverless-computing)
7. [ECS and EKS - Container Services](#7-ecs-and-eks-container-services)
8. [Elastic Beanstalk](#8-elastic-beanstalk)

### Part 3: Storage Services
9. [S3 - Simple Storage Service](#9-s3-simple-storage-service)
10. [EBS - Elastic Block Store](#10-ebs-elastic-block-store)
11. [EFS - Elastic File System](#11-efs-elastic-file-system)
12. [Storage Gateway](#12-storage-gateway)

### Part 4: Database Services
13. [RDS - Relational Database Service](#13-rds-relational-database-service)
14. [DynamoDB - NoSQL Database](#14-dynamodb-nosql-database)
15. [ElastiCache - In-Memory Caching](#15-elasticache-in-memory-caching)
16. [Redshift - Data Warehouse](#16-redshift-data-warehouse)

### Part 5: Networking Services
17. [VPC - Virtual Private Cloud](#17-vpc-virtual-private-cloud)
18. [Route 53 - DNS Service](#18-route-53-dns-service)
19. [CloudFront - Content Delivery Network](#19-cloudfront-content-delivery-network)
20. [Elastic Load Balancing](#20-elastic-load-balancing)

### Part 6: Security & Compliance
21. [AWS Security Best Practices](#21-aws-security-best-practices)
22. [KMS - Key Management Service](#22-kms-key-management-service)
23. [Secrets Manager](#23-secrets-manager)
24. [WAF and Shield](#24-waf-and-shield)

### Part 7: Monitoring & Management
25. [CloudWatch - Monitoring Service](#25-cloudwatch-monitoring-service)
26. [CloudTrail - Audit Logging](#26-cloudtrail-audit-logging)
27. [Systems Manager](#27-systems-manager)
28. [AWS Config](#28-aws-config)

### Part 8: Additional Services
29. [SNS and SQS - Messaging Services](#29-sns-and-sqs-messaging-services)
30. [Step Functions - Workflow Orchestration](#30-step-functions-workflow-orchestration)
31. [EventBridge - Event-Driven Architecture](#31-eventbridge-event-driven-architecture)

### Part 9: Cost Optimization
32. [AWS Cost Management](#32-aws-cost-management)
33. [Pricing Models](#33-pricing-models)
34. [Cost Optimization Strategies](#34-cost-optimization-strategies)

### Part 10: Practical Projects & Interview Prep
35. [Hands-On Projects](#35-hands-on-projects)
36. [AWS Interview Questions](#36-aws-interview-questions)
37. [Certification Guide](#37-certification-guide)

---

## Part 1: AWS Fundamentals

### 1. Introduction to AWS Cloud

#### What is AWS?
Amazon Web Services (AWS) is a comprehensive cloud computing platform offering over 200 services including compute, storage, databases, networking, AI/ML, and more.

**Key Benefits:**
- **Pay-as-you-go**: Only pay for what you use
- **Scalability**: Scale up or down based on demand
- **Global Reach**: Available in 32 geographic regions
- **Reliability**: 99.99% uptime SLA
- **Security**: Built-in security and compliance

#### Cloud Computing Models:
- **IaaS**: Infrastructure as a Service (EC2, VPC)
- **PaaS**: Platform as a Service (Elastic Beanstalk, RDS)
- **SaaS**: Software as a Service (WorkDocs, WorkMail)

---

### 2. AWS Global Infrastructure

#### Components:
1. **Regions**: Geographic locations (e.g., us-east-1, eu-west-1)
2. **Availability Zones (AZs)**: Isolated data centers within a region
3. **Edge Locations**: CDN endpoints for CloudFront
4. **Local Zones**: Extensions of regions for low-latency

**Best Practices:**
- Deploy across multiple AZs for high availability
- Choose regions based on latency and compliance
- Use CloudFront edge locations for global content delivery

---

### 3. IAM - Identity and Access Management

#### Core Concepts:

**Users, Groups, and Roles:**
```json
// IAM Policy Example
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

**IAM Best Practices:**
1. Enable MFA for root account
2. Use IAM roles instead of access keys
3. Follow principle of least privilege
4. Rotate credentials regularly
5. Use IAM groups for permissions management

---

## Part 2: Compute Services

### 5. EC2 - Elastic Compute Cloud

#### Instance Types:
- **General Purpose**: t2, t3, m5 (balanced compute, memory, networking)
- **Compute Optimized**: c5, c6 (high-performance processors)
- **Memory Optimized**: r5, x1 (fast performance for large datasets)
- **Storage Optimized**: i3, d2 (high sequential read/write)
- **GPU Instances**: p3, g4 (machine learning, graphics)

#### Launch EC2 Instance (AWS CLI):
```bash
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --instance-type t2.micro \
  --key-name my-key-pair \
  --security-group-ids sg-903004f8 \
  --subnet-id subnet-6e7f829e \
  --count 1 \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=MyWebServer}]'
```

#### User Data Script:
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from $(hostname -f)</h1>" > /var/www/html/index.html
```

---

### 6. Lambda - Serverless Computing

#### What is AWS Lambda?
Run code without provisioning servers. Pay only for compute time consumed.

**Use Cases:**
- API backends
- Data processing
- Real-time file processing
- Scheduled tasks
- Event-driven applications

**Example Lambda Function (Python):**
```python
import json
import boto3

def lambda_handler(event, context):
    # Process S3 event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    print(f'Processing file: {key} from bucket: {bucket}')
    
    # Your processing logic here
    
    return {
        'statusCode': 200,
        'body': json.dumps('Successfully processed!')
    }
```

**Lambda Limits:**
- Memory: 128 MB to 10 GB
- Timeout: Max 15 minutes
- Deployment package: 50 MB (zipped), 250 MB (unzipped)
- Concurrent executions: 1000 (default)

---

## Part 3: Storage Services

### 9. S3 - Simple Storage Service

#### S3 Storage Classes:
1. **S3 Standard**: Frequently accessed data
2. **S3 Intelligent-Tiering**: Automatic cost optimization
3. **S3 Standard-IA**: Infrequently accessed
4. **S3 One Zone-IA**: Lower-cost IA storage
5. **S3 Glacier Instant Retrieval**: Archive with instant access
6. **S3 Glacier Flexible Retrieval**: Minutes to hours retrieval
7. **S3 Glacier Deep Archive**: Lowest cost, 12-hour retrieval

#### S3 Operations (AWS CLI):
```bash
# Create bucket
aws s3 mb s3://my-unique-bucket-name --region us-east-1

# Upload file
aws s3 cp myfile.txt s3://my-bucket/

# Upload with encryption
aws s3 cp myfile.txt s3://my-bucket/ --sse AES256

# Download file
aws s3 cp s3://my-bucket/myfile.txt ./

# Sync directory
aws s3 sync ./local-dir s3://my-bucket/remote-dir

# List objects
aws s3 ls s3://my-bucket/

# Delete object
aws s3 rm s3://my-bucket/myfile.txt
```

#### S3 Lifecycle Policy:
```json
{
  "Rules": [
    {
      "Id": "Archive old logs",
      "Filter": {
        "Prefix": "logs/"
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "STANDARD_IA"
        },
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

---

## Part 4: Database Services

### 13. RDS - Relational Database Service

#### Supported Engines:
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Aurora (AWS proprietary)

---

### 14. DynamoDB - NoSQL Database

**Features:**
- Fully managed NoSQL database
- Single-digit millisecond latency
- Auto-scaling
- Built-in security and backup

**Example (Python Boto3):**
```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Users')

# Put item
table.put_item(
    Item={
        'user_id': '123',
        'name': 'John Doe',
        'email': 'john@example.com'
    }
)

# Query
response = table.query(
    KeyConditionExpression=Key('user_id').eq('123')
)
```

---

## Part 5: Networking Services

### 17. VPC - Virtual Private Cloud

**Components:**
- Subnets (Public/Private)
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Network ACLs

---

## Part 10: Interview Questions & Certification

### 36. AWS Interview Questions

**Q1: What is the difference between S3 and EBS?**
- **S3**: Object storage, accessible via HTTP/HTTPS, unlimited storage
- **EBS**: Block storage, attached to EC2 instances, limited by volume size

**Q2: Explain Auto Scaling?**
Auto Scaling automatically adjusts the number of EC2 instances based on demand to maintain performance and minimize costs.

---

### 37. Certification Guide

**AWS Certifications:**
1. **Cloud Practitioner**: Entry-level, cloud fundamentals
2. **Solutions Architect Associate**: Most popular, architecture design
3. **Developer Associate**: Application development on AWS
4. **SysOps Administrator**: Operations and deployment
5. **Solutions Architect Professional**: Advanced architecture
6. **DevOps Engineer Professional**: Continuous delivery and automation

---

## Additional Resources

- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS Training](https://aws.amazon.com/training/)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)

---

## Contributing

Feel free to contribute by:
- Opening issues for corrections
- Submitting pull requests
- Sharing practical examples
- Adding more use cases

---

## License

MIT License - Free to use for learning purposes

---

## Contact

- GitHub: [@thilakakula13](https://github.com/thilakakula13)
- LinkedIn: [Thilak Akula](https://www.linkedin.com/in/thilak-akula)

---

**⭐ If this guide helped you, please star this repository!**

Happy Cloud Learning! ☁️
