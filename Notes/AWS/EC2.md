# Complete Amazon EC2 Tutorial

## Table of Contents
1. [Introduction to EC2](#introduction-to-ec2)
2. [EC2 Instance Types](#ec2-instance-types)
3. [AMIs (Amazon Machine Images)](#amis-amazon-machine-images)
4. [Security Groups](#security-groups)
5. [Key Pairs](#key-pairs)
6. [Storage Options](#storage-options)
7. [Networking](#networking)
8. [Launch Templates](#launch-templates)
9. [Auto Scaling](#auto-scaling)
10. [Load Balancing](#load-balancing)
11. [Monitoring and Logging](#monitoring-and-logging)
12. [Pricing Models](#pricing-models)
13. [Best Practices](#best-practices)
14. [Hands-on Examples](#hands-on-examples)

---

## Introduction to EC2

Amazon Elastic Compute Cloud (EC2) is a web service that provides secure, resizable compute capacity in the cloud. It's designed to make web-scale cloud computing easier for developers.

### Key Benefits:
- **Elasticity**: Scale capacity up or down within minutes
- **Complete Control**: Root access to each instance
- **Flexible**: Choice of instance types, operating systems, and software packages
- **Reliable**: 99.99% availability SLA
- **Secure**: Multiple levels of security
- **Inexpensive**: Pay only for capacity you use

### Core Concepts:
- **Instance**: A virtual server in the cloud
- **AMI**: Template for launching instances
- **Instance Type**: Hardware configuration
- **Region**: Geographic location of data centers
- **Availability Zone**: Isolated locations within regions

---

## EC2 Instance Types

EC2 offers various instance types optimized for different use cases:

### General Purpose
**T4g, T3, T3a, T2, M6i, M6a, M5, M5a, M4**
- Balanced CPU, memory, and networking
- Ideal for web servers, small databases, development environments

**Example**: `t3.micro` - 1 vCPU, 1 GB RAM, Variable network performance

### Compute Optimized
**C6i, C6a, C5, C5n, C4**
- High-performance processors
- CPU-intensive applications, scientific modeling, gaming servers

**Example**: `c5.large` - 2 vCPUs, 4 GB RAM, Up to 10 Gbps network

### Memory Optimized
**R6i, R6a, R5, R5a, R4, X1e, X1, z1d**
- High memory-to-vCPU ratios
- In-memory databases, big data analytics

**Example**: `r5.xlarge` - 4 vCPUs, 32 GB RAM, Up to 10 Gbps network

### Storage Optimized
**I4i, I3, I3en, D2, D3, D3en, H1**
- High sequential read/write access
- NoSQL databases, data warehousing, file systems

**Example**: `i3.large` - 2 vCPUs, 15.25 GB RAM, 1 x 475 GB NVMe SSD

### Accelerated Computing
**P4, P3, P2, Inf1, G4dn, G4ad, G3, F1**
- Hardware accelerators (GPUs, FPGAs)
- Machine learning, high performance computing

**Example**: `p3.2xlarge` - 8 vCPUs, 61 GB RAM, 1 x NVIDIA V100 GPU

---

## AMIs (Amazon Machine Images)

An AMI is a template containing software configuration (OS, application server, applications) needed to launch an instance.

### Types of AMIs:
1. **Amazon-provided AMIs**: Pre-configured by AWS
2. **AWS Marketplace AMIs**: Third-party AMIs
3. **Community AMIs**: Shared by AWS community
4. **Custom AMIs**: Created by you

### AMI Components:
- **Root Volume Template**: OS and applications
- **Launch Permissions**: Control who can use the AMI
- **Block Device Mapping**: Storage volumes to attach

### Creating a Custom AMI:
```bash
# 1. Launch and configure an instance
# 2. Create AMI from running instance
aws ec2 create-image \
    --instance-id i-1234567890abcdef0 \
    --name "My Custom AMI" \
    --description "AMI with custom configuration"

# 3. Launch new instance from custom AMI
aws ec2 run-instances \
    --image-id ami-12345678 \
    --instance-type t3.micro \
    --key-name MyKeyPair
```

---

## Security Groups

Security groups act as virtual firewalls controlling inbound and outbound traffic to instances.

### Key Characteristics:
- **Stateful**: Return traffic automatically allowed
- **Default Deny**: All inbound traffic blocked by default
- **Multiple Groups**: Can assign multiple security groups per instance
- **Rule Changes**: Take effect immediately

### Example Security Group Rules:

**Web Server Security Group:**
```json
{
    "GroupName": "WebServerSG",
    "Rules": [
        {
            "Type": "Inbound",
            "Protocol": "HTTP",
            "Port": 80,
            "Source": "0.0.0.0/0"
        },
        {
            "Type": "Inbound",
            "Protocol": "HTTPS", 
            "Port": 443,
            "Source": "0.0.0.0/0"
        },
        {
            "Type": "Inbound",
            "Protocol": "SSH",
            "Port": 22,
            "Source": "203.0.113.0/24"
        }
    ]
}
```

**Database Security Group:**
```json
{
    "GroupName": "DatabaseSG",
    "Rules": [
        {
            "Type": "Inbound",
            "Protocol": "MySQL/Aurora",
            "Port": 3306,
            "Source": "sg-12345678"
        }
    ]
}
```

### Creating Security Groups with AWS CLI:
```bash
# Create security group
aws ec2 create-security-group \
    --group-name WebServerSG \
    --description "Security group for web servers"

# Add inbound rules
aws ec2 authorize-security-group-ingress \
    --group-id sg-12345678 \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
    --group-id sg-12345678 \
    --protocol tcp \
    --port 443 \
    --cidr 0.0.0.0/0
```

---

## Key Pairs

Key pairs provide secure access to EC2 instances using public-key cryptography.

### Components:
- **Public Key**: AWS stores this on your instance
- **Private Key**: You store this securely (.pem file)

### Creating Key Pairs:
```bash
# Create key pair
aws ec2 create-key-pair \
    --key-name MyKeyPair \
    --query 'KeyMaterial' \
    --output text > MyKeyPair.pem

# Set proper permissions
chmod 400 MyKeyPair.pem

# Connect to instance
ssh -i MyKeyPair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
```

### Key Pair Best Practices:
- Store private keys securely
- Use different key pairs for different environments
- Regularly rotate key pairs
- Never share private keys

---

## Storage Options

EC2 provides several storage options for different use cases:

### 1. Amazon EBS (Elastic Block Store)

**Volume Types:**

**gp3 (General Purpose SSD)**
- 3,000-16,000 IOPS
- 125-1,000 MB/s throughput
- Cost-optimized

**gp2 (General Purpose SSD)**
- Baseline 3 IOPS/GB (min 100, max 16,000)
- Burstable to 3,000 IOPS

**io2/io1 (Provisioned IOPS SSD)**
- Up to 64,000 IOPS
- High-performance databases

**st1 (Throughput Optimized HDD)**
- 40-500 MB/s throughput
- Big data, data warehouses

**sc1 (Cold HDD)**
- 12-250 MB/s throughput
- Infrequent access

### Creating and Attaching EBS Volume:
```bash
# Create EBS volume
aws ec2 create-volume \
    --size 100 \
    --volume-type gp3 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=MyVolume}]'

# Attach volume to instance
aws ec2 attach-volume \
    --volume-id vol-12345678 \
    --instance-id i-1234567890abcdef0 \
    --device /dev/sdf

# Mount volume (on instance)
sudo mkfs -t xfs /dev/xvdf
sudo mkdir /data
sudo mount /dev/xvdf /data
```

### 2. Instance Store

Temporary storage physically attached to host computer:
- High I/O performance
- Data lost when instance stops
- No additional cost
- Cannot be detached/reattached

### 3. Amazon EFS (Elastic File System)

Network file system that can be mounted on multiple instances:
```bash
# Mount EFS
sudo mount -t efs fs-12345678.efs.us-east-1.amazonaws.com:/ /mnt/efs
```

---

## Networking

### VPC (Virtual Private Cloud)
Logically isolated section of AWS cloud where you launch resources.

### Subnets
Range of IP addresses in your VPC:
- **Public Subnet**: Has route to Internet Gateway
- **Private Subnet**: No direct internet access

### Example VPC Setup:
```bash
# Create VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create public subnet
aws ec2 create-subnet \
    --vpc-id vpc-12345678 \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a

# Create private subnet
aws ec2 create-subnet \
    --vpc-id vpc-12345678 \
    --cidr-block 10.0.2.0/24 \
    --availability-zone us-east-1b

# Create and attach Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
    --vpc-id vpc-12345678 \
    --internet-gateway-id igw-12345678
```

### Elastic IP Addresses
Static IPv4 addresses for dynamic cloud computing:
```bash
# Allocate Elastic IP
aws ec2 allocate-address --domain vpc

# Associate with instance
aws ec2 associate-address \
    --instance-id i-1234567890abcdef0 \
    --allocation-id eipalloc-12345678
```

### Placement Groups
Logical grouping of instances:
- **Cluster**: Low latency, high throughput
- **Partition**: Large distributed workloads
- **Spread**: Critical instances on separate hardware

---

## Launch Templates

Reusable configurations for launching instances with consistent settings.

### Creating Launch Template:
```json
{
    "LaunchTemplateName": "WebServerTemplate",
    "LaunchTemplateData": {
        "ImageId": "ami-0abcdef1234567890",
        "InstanceType": "t3.micro",
        "KeyName": "MyKeyPair",
        "SecurityGroupIds": ["sg-12345678"],
        "UserData": "IyEvYmluL2Jhc2gKZWNobyAiSGVsbG8gV29ybGQi",
        "TagSpecifications": [
            {
                "ResourceType": "instance",
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "WebServer"
                    }
                ]
            }
        ]
    }
}
```

```bash
# Create launch template
aws ec2 create-launch-template --cli-input-json file://launch-template.json

# Launch instance from template
aws ec2 run-instances \
    --launch-template LaunchTemplateName=WebServerTemplate,Version=1 \
    --min-count 1 \
    --max-count 1
```

---

## Auto Scaling

Automatically adjusts the number of instances based on demand.

### Auto Scaling Group Configuration:
```bash
# Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name WebServerASG \
    --launch-template LaunchTemplateName=WebServerTemplate,Version=1 \
    --min-size 1 \
    --max-size 5 \
    --desired-capacity 2 \
    --vpc-zone-identifier "subnet-12345678,subnet-87654321" \
    --health-check-type ELB \
    --health-check-grace-period 300
```

### Scaling Policies:
```bash
# Target tracking scaling policy
aws autoscaling put-scaling-policy \
    --policy-name cpu-scaling-policy \
    --auto-scaling-group-name WebServerASG \
    --policy-type TargetTrackingScaling \
    --target-tracking-configuration '{
        "TargetValue": 70.0,
        "PredefinedMetricSpecification": {
            "PredefinedMetricType": "ASGAverageCPUUtilization"
        }
    }'
```

---

## Load Balancing

Distributes incoming traffic across multiple instances.

### Application Load Balancer (ALB):
```bash
# Create Application Load Balancer
aws elbv2 create-load-balancer \
    --name WebServerALB \
    --subnets subnet-12345678 subnet-87654321 \
    --security-groups sg-12345678

# Create target group
aws elbv2 create-target-group \
    --name WebServerTargets \
    --protocol HTTP \
    --port 80 \
    --vpc-id vpc-12345678 \
    --health-check-path /health

# Create listener
aws elbv2 create-listener \
    --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/app/WebServerALB/1234567890123456 \
    --protocol HTTP \
    --port 80 \
    --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/WebServerTargets/1234567890123456
```

---

## Monitoring and Logging

### CloudWatch Metrics
Basic monitoring (5-minute intervals) is enabled by default:
- CPU Utilization
- Network I/O
- Disk I/O
- Status Checks

### Detailed Monitoring:
```bash
# Enable detailed monitoring (1-minute intervals)
aws ec2 monitor-instances --instance-ids i-1234567890abcdef0
```

### CloudWatch Alarms:
```bash
# Create CPU alarm
aws cloudwatch put-metric-alarm \
    --alarm-name cpu-high \
    --alarm-description "High CPU utilization" \
    --metric-name CPUUtilization \
    --namespace AWS/EC2 \
    --statistic Average \
    --period 300 \
    --threshold 80 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 2 \
    --dimensions Name=InstanceId,Value=i-1234567890abcdef0
```

### Systems Manager Session Manager
Secure shell access without SSH keys or bastion hosts:
```bash
# Start session
aws ssm start-session --target i-1234567890abcdef0
```

---

## Pricing Models

### 1. On-Demand Instances
Pay for compute capacity by hour/second with no long-term commitments.

**Use Cases:**
- Applications with unpredictable workloads
- Development and testing
- First-time applications

**Example Pricing (us-east-1):**
- t3.micro: $0.0104/hour
- m5.large: $0.096/hour
- c5.xlarge: $0.192/hour

### 2. Reserved Instances
Significant discount (up to 72%) compared to On-Demand pricing.

**Terms:**
- 1 or 3 years
- All Upfront, Partial Upfront, No Upfront payment

**Types:**
- Standard RI: Up to 72% discount
- Convertible RI: Up to 54% discount, can change instance attributes

### 3. Spot Instances
Bid for unused EC2 capacity at up to 90% discount.

```bash
# Request spot instance
aws ec2 request-spot-instances \
    --spot-price "0.05" \
    --instance-count 1 \
    --type "one-time" \
    --launch-specification '{
        "ImageId": "ami-0abcdef1234567890",
        "InstanceType": "t3.micro",
        "KeyName": "MyKeyPair",
        "SecurityGroupIds": ["sg-12345678"]
    }'
```

### 4. Dedicated Hosts
Physical EC2 server dedicated for your use.

### 5. Savings Plans
Flexible pricing model offering lower prices in exchange for usage commitment.

---

## Best Practices

### Security
1. **Use IAM roles** instead of storing credentials on instances
2. **Apply least privilege** principle to security groups
3. **Keep AMIs updated** with latest security patches
4. **Enable detailed monitoring** for production workloads
5. **Use Systems Manager** for patch management

### Performance
1. **Choose appropriate instance type** for your workload
2. **Use placement groups** for high-performance computing
3. **Optimize EBS** volume types and sizes
4. **Implement caching** strategies
5. **Use content delivery networks** (CloudFront)

### Cost Optimization
1. **Right-size instances** based on actual usage
2. **Use Reserved Instances** for predictable workloads
3. **Leverage Spot Instances** for fault-tolerant applications
4. **Implement Auto Scaling** to match capacity with demand
5. **Regular review** of unused resources

### Availability and Reliability
1. **Deploy across multiple AZs**
2. **Implement health checks** and auto-recovery
3. **Use load balancers** for high availability
4. **Regular backups** of EBS volumes
5. **Test disaster recovery** procedures

---

## Hands-on Examples

### Example 1: Launch a Web Server

```bash
#!/bin/bash

# 1. Create key pair
aws ec2 create-key-pair \
    --key-name WebServerKey \
    --query 'KeyMaterial' \
    --output text > WebServerKey.pem
chmod 400 WebServerKey.pem

# 2. Create security group
SECURITY_GROUP_ID=$(aws ec2 create-security-group \
    --group-name WebServerSG \
    --description "Security group for web server" \
    --query 'GroupId' --output text)

# 3. Add rules to security group
aws ec2 authorize-security-group-ingress \
    --group-id $SECURITY_GROUP_ID \
    --protocol tcp --port 80 --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
    --group-id $SECURITY_GROUP_ID \
    --protocol tcp --port 22 --cidr 0.0.0.0/0

# 4. Create user data script
cat > userdata.sh << 'EOF'
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from EC2!</h1>" > /var/www/html/index.html
EOF

# 5. Launch instance
INSTANCE_ID=$(aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --count 1 \
    --instance-type t3.micro \
    --key-name WebServerKey \
    --security-group-ids $SECURITY_GROUP_ID \
    --user-data file://userdata.sh \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]' \
    --query 'Instances[0].InstanceId' --output text)

echo "Instance launched: $INSTANCE_ID"
```

### Example 2: Set up Auto Scaling with Load Balancer

```bash
#!/bin/bash

# 1. Create launch template
aws ec2 create-launch-template \
    --launch-template-name WebAppTemplate \
    --launch-template-data '{
        "ImageId": "ami-0abcdef1234567890",
        "InstanceType": "t3.micro",
        "KeyName": "WebServerKey",
        "SecurityGroupIds": ["'$SECURITY_GROUP_ID'"],
        "UserData": "'$(base64 -w 0 userdata.sh)'"
    }'

# 2. Create target group
TARGET_GROUP_ARN=$(aws elbv2 create-target-group \
    --name WebAppTargets \
    --protocol HTTP \
    --port 80 \
    --vpc-id vpc-12345678 \
    --health-check-path "/" \
    --query 'TargetGroups[0].TargetGroupArn' --output text)

# 3. Create load balancer
LOAD_BALANCER_ARN=$(aws elbv2 create-load-balancer \
    --name WebAppLB \
    --subnets subnet-12345678 subnet-87654321 \
    --security-groups $SECURITY_GROUP_ID \
    --query 'LoadBalancers[0].LoadBalancerArn' --output text)

# 4. Create listener
aws elbv2 create-listener \
    --load-balancer-arn $LOAD_BALANCER_ARN \
    --protocol HTTP --port 80 \
    --default-actions Type=forward,TargetGroupArn=$TARGET_GROUP_ARN

# 5. Create Auto Scaling group
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name WebAppASG \
    --launch-template LaunchTemplateName=WebAppTemplate,Version=1 \
    --min-size 2 \
    --max-size 6 \
    --desired-capacity 2 \
    --target-group-arns $TARGET_GROUP_ARN \
    --health-check-type ELB \
    --health-check-grace-period 300 \
    --vpc-zone-identifier "subnet-12345678,subnet-87654321"
```

### Example 3: Backup and Restore Strategy

```bash
#!/bin/bash

# Create EBS snapshot
SNAPSHOT_ID=$(aws ec2 create-snapshot \
    --volume-id vol-12345678 \
    --description "Backup of web server volume" \
    --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=WebServerBackup}]' \
    --query 'SnapshotId' --output text)

# Create AMI from instance
AMI_ID=$(aws ec2 create-image \
    --instance-id i-1234567890abcdef0 \
    --name "WebServer-$(date +%Y%m%d-%H%M%S)" \
    --description "Backup AMI of web server" \
    --no-reboot \
    --query 'ImageId' --output text)

echo "Snapshot created: $SNAPSHOT_ID"
echo "AMI created: $AMI_ID"
```

---

## Conclusion

This tutorial covered the essential concepts of Amazon EC2, from basic instance management to advanced topics like Auto Scaling and Load Balancing. The key to mastering EC2 is understanding how these components work together to create scalable, reliable, and cost-effective cloud architectures.

### Next Steps:
1. Practice launching different instance types
2. Experiment with various storage options
3. Set up monitoring and alerting
4. Implement infrastructure as code using CloudFormation or Terraform
5. Explore advanced topics like container services (ECS, EKS)

Remember to always follow AWS best practices for security, performance, and cost optimization in your EC2 deployments.
