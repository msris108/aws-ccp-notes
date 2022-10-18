## Table of Contents
1. [Introduction to Amazon Web Services](#Introduction%20to%20Amazon%20Web%20Services)
2. [Compute in the Cloud](#Compute%20in%20the%20Cloud)
3. [Global Infrastructure and Reliability](#Global%20Infrastructure%20and%20Reliability)
4. [Networking](#Networking)
5. [Storage and Databases](#Storage%20and%20Databases)
6. [Security](#Security)
7. [Monitoring and Analytics](#Monitoring%20and%20Analytics)
8. [Pricing and Support](#Pricing%20and%20Support)
9. [Migration and Innovation](#Migration%20and%20Innovation)

## Introduction to Amazon Web Services 
---
- Access Services on Demand
- Avoid large upfront investments 
- Provision computing resources as needed
- Pay only for what you use

#### Deployment Models: 
1. Cloud
2. On Premises
3. Cloud + On Premise (Hybrid)

- Using AWS outpost we can make AWS run on On-Perm Device

#### Benefits
1. Variable Expenses over Upfront Expenses
2. Focus on application development
3. (Auto) Scalability 
4. Pay for Usage
5. Speed and Agility

#### Core Services
![Pasted image 20221018103108.png](Pasted%20image%2020221018103108.png)


## Compute in the Cloud 
---
#EC2 #SNS #SKS #ECR #EKS #Lambda

### EC2 : Elastic Compute 2 
- Use secure, sizable compute capacity
- Boot server instances in minutes
- Pay only for what you use
- Using security groups: Requires Key-Pair for generation 

#### [EC2 Types](https://aws.amazon.com/ec2/instance-types/)
- General Purpose
	- Balances compute memory and network resources
- Compute Optimized
	- offers high performance processors
	- ideal for intensive application
	- ideal for batch optimization
- Memory Optimized
	- SQL, SAP
	- in memory caching
	- ideal for high-performance databases
- Accelerated Computing
	- graphics processing
	- intensive floating point operations 
	- machine learning
- Storage Optimized
	- max tps (transanctions per second)
	- suitable for data warehousing

#### [EC2 Pricing](https://aws.amazon.com/ec2/pricing/)
- _On demand_ - No upfront - Short term
- _Spot_ - 90% discount rates; ideal for flexible start and end time; idle ec2 machines; saves price 
- _Reserved_ - 1 year or 3 year commitment (Discounted upto 72%)
- _Compute Savings Plan_ - 66% reduction 1yr/3yr commitment (more flexible than reserved)
- _Dedicated Instance_ - above instances are shareable, but this instance is dedicated instance to a particular user. No shared tenancy. More Expensice
- _Dedicated host_ - Given a complete physical server, Paid upfront, most expensive, 3 years plan

#### Auto-Scaling - Elastic Load Balancer
- Dynamic Scaling - Policy based
- Predictive Scaling - Traffic is forecasted
- Autoscaling Group - Minimum | Desired | Maximum 
- Elastic Load Balancing - when attached with Autoscaling Group acts as a single point of contact for scaling the apps 

![Pasted image 20221018114431.png](Pasted%20image%2020221018103108.png)


### Messaging services: SNS, SQS
- Simple Notification Service: Publisher - Subscriber model, Topic based
- Simple Queue Servies: Send, Store and Receive Messages between software components 

### Severless : Lambda Function
- Scratch | Template | Container
- name > desc > runtime > arch
- Triggers > choose event driven function (S3 operations, code commits, kafka pub/sub)
- Results of lambda can we viewed in CloudWatch

### Containers : ECS | EKS
- Elastic Container Service | Elastic Kubernetes Service
- Container Orchestration: 
	- Health Checks 
	- Load Balancing 
	- Execution Sequence 
- ECS is for simple docker apps while EKS scales using K8s
- Fargate - Serverless Container Management | Pay based on resources being deployed
- Container Management can be hosted on a server like EC2 or Fargate. EKS/ECR hosted on EC2 is the resposibilty of the user to handle whereas AWS handles in the case of Fargate

## Global Infrastructure and Reliability 
---
- 27 Regions | 87 AZs | 410 Edge location (Edge Nodes)
- AZs are isolated from each other | located at a few miles way from each other | disaster recovery 
- Criteria for choosing region : 
	- Agree with Complaince of the country
	- Proximity 
	- Service availabilty in that region 
	- Pricing 
- Recommended number of AZs : 3
- CloudFront acts as GCDN (Global CDN) which caches application at the edge node nearest
- Region > AZ > Edge Locations 
- **Application is deployed in a Region _refer criteria to choose region_**
- **Each Region corresponds to multiple AZs for fault tolerance/ disaster recovery**
- **Edge Location is used for caching the application**

### Interacting with AWS
1. Console
2. CLI
3. SDK

## Networking 
---
#VPC #VPN #Subnets #NACL #SecurityGroups 
	1. VPC
	2. Public and Private subnets
	3. Virtual Private Gateway
	4. VPN 
	5. Direct Connect
	6. Hybrid Deployment
	7. Security

#### Virtual Private Cloud
- Configured with providing CIDR 
- Internet Gateway - Entry Point
- VPC consists of subnets - Public | Private
- Customer should not be able to access the DB
- To create a VPN to access corporate datacenters/ services we can use a VGW a virtual private gateway - Over the internet
- To access a public subnet we can use IGW an Internet Gateway
- Another way to access corporate services is using AWS Direct Connect - Bypasses Internet 

#### NACL and Security Groups
- Network Access Control List 
	- Firewall at Subnet 
	- Stateless - No history is maintained
	- By default allows all inbound rules and outbound rules 
	- Must write explicit rule
- Security Groups
	- Firewall at Instance level
	- Allow routes only - by default denies inboud and allows all outbound rules
	- Stateful
	- Customizable rules
- Connecting with Global Network
	- Route 53
		- Route users 
		- Manages DNS records
		- Acts a DNS market plave

## Storage and Databases
---
#EBS #S3 #EFS #RDS #DynamoDB

- Block Storage - EBS - Independant Blocks - Single AZ
- Object Storage - S3 - Metadata - Every object has a unique ID: Key
- File Storage - EFS - For concurrent access / sharing entire file sys / can span accross multiple AZs

### Elastic Block Storage
![Pasted image 20221018142414.png](Pasted%20image%2020221018142414.png)
- Files are seperated into equal size chunks 
- Usually associated with EC2 -- associated hardware for root user
- _Instance Store_: 
	- temp data, once instance is stopped data in the instance store will be deleted
	- use cases without persistence 
	- only available with few supporting instance
- EBS volume is persistant
- We can create a snapshot of an EBS volume and able to recreate the volume 

#### Simple Storage Service - S3
- Data | Metadata | Key
- Store objects 
- Set permissions to access objects 
- Choose from a range of _storage classes_ to suite use case - Cost Effective Solution
- Unlimited Storage, Only limitation is on max file size
- Stored across 3 AZs (atleast)
- Durability : 99.99999999999% 

##### **Storage Classes:** 
- S3 Standard: 
	- freq access
	- 3 AZs
- S3 Standard - IA 
	- Infrequent Access 
	- lower price 
	- 3 AZ
- S3 One Zone - 1A
	- Same as above but only one AZ
	- Cheaper 
	- ideal for backups
- S3 Intelligent - Tiering 
	- Ideal for unknown or changing patterns 
	- Requires a small monthly monitoring charge
- S3 Glacier 
	- Low-cost storage 
	- Data archiving
	- Able to retrieve objects within a few minutes to hours
- S3 Glacier Deep Archive 
	- Cheapest 
	- Within 12 hours 

`Create Bucket > Name | Name must be unique globally `

#### Elastic File Storage
- Multiple clients can access the file system
- Can mount accross multiple AZs unlike other storage options 
- Scalable file system
- Thousands of EC2s(users) can access 
- 3AZ or 1AZ

### Databases 
- RDBMS | NoSQL
- Amazon RDS is an AWS managed service 
- **Amazon Aurora** is 5x faster than MySQL and 3x faster than Postgres
	- Aurora has a serverless option as well | Replicas are spread across 3 AZs with 2 copies in each zone
- **DynamoDB** is NoSQL DB service 
	- 10 trillion reqs in a day | auto scales | key - values pair | Serverless
- **AWS Database Migration Service** : Supports both homogenous and heterogeneous migrations 
- **Other Options** 
![Pasted image 20221018150102.png](Pasted%20image%2020221018150102.png)
![Pasted%20image%2020221018150132.png](Pasted%20image%2020221018150132.png)
## Security 
---
#MFA #IAM #Compliance #WAF #Sheild #Inspector #Guard #KMS

### Shared Responsibility Model

![Pasted image 20221018151203.png](Pasted%20image%2020221018151203.png)

- Customer is responsible for the security inside cloud, while AWS is responsible for the security of the Cloud
- Customer Responsibiliteis 
	- Instance OS
	- Account Management 
	- Security Groups 
	- Application | Software Patching 
	- Host based firewall 
- AWS Responsibilities 
	- Physical security
	- Network Infrastructure
	- Hardware and Soft infrastructure 
	- Virtualization Infrastructure

### Identity and Access Management 
- IAM user
	- On account creation you are a Root User
	- Root User create -> IAM user 
- IAM policy 
	- JSON Document that provides or grants permission for user/ services 
	- Best practice is to use least access
![Pasted image 20221018152648.png](Pasted%20image%2020221018152648.png)
- IAM group 
	- Collection of IAM users 
	- Best Practice to Attach IAM policy to group not with Individual User
- IAM role 
	- IAM roles are the preferred method for interacting with AWS services. As a best practice, IAM roles are ideal for situations in which access to services or resources needs to be granted **temporarily**, rather than long term
- MFA - Multi Factor Authentication 

#### AWS Organisation
- Consolidated management of multiple accounts - Using SCP
- SCP - Service Control Policies 
- Groups can be assigned called OUs - Organisational Unit
- SCP can be applied at Member level and OU level

#### Compliance
1. AWS Artifact 
	- Documentation 
	- Not like a Compute/ Storage Service
	- Signing and management of agreements 
2. Customer Compliance Center 
	- Free tools to understand user stories and templated from other successful orgs

_WAF_ : Web Application Firewall - Prevents Malicious Attacks 
**DDOS Attack** can be prevented with _AWS Sheild_
AWS Sheild can be coupled with WAF and CloudWatch
_Amazon Inspectors_: security assessments your application and how far away from best practices, also gives recommendations
_Amazon Key Management Services_: All S3 and RDS uses KMS for encryption and decryption 
_Guard Duty_: intelligent threat detection services

## Monitoring and Analytics
---
#CloudWatch #CloudTrail

### Cloud Watch - Services
- Set alarm - when a certain metric is triggered 
- Access all metrics from a single location - Single Dashboard 
- Real time monitoring 
- Configure alerts and actions 

### Cloud Trail - Users 
- Track user activities 
- Track API requests 
- Filter out logs and troubleshooting 
- Automatically detect unusual activity

### AWS Trusted Advisor
Cost Optimization | Performance | Security | Fault Tolerance | Service Limits 
- Gives recommendations 
- Compares existing infrastructure with best practices 
- Evaluation and Implementation
- Used for *Reviewing*

## Pricing and Support
---
#Budget #CostExplorer #PricingCalculator 

#### Pricing Models 
- Pay as you go
- Pay less when you reserve 
- Pay less with volume-based discounts 

1. **Lamdba Pricing:** 
	- Pay per compute time
	- Pay for total number of requests 
	- Save money using Compute saving plans 
2. **EC2 Pricing**
	- only for the time instances run
	- on-demand oat
	- reduce cost using spot 
3. **S3 Pricing** 
	- Storage
	- Requests and retrieval operations
	- Data transfer 
	- Management and Replication 

#### Consolidated Billing 
- Reduced Billing
- Additional discounts on volume based services i.e. Consolidated Volume 

##### AWS Budgets - Cost Explorer
1. Budget - Set Threshold
2. Cost Explorer - Shows complete details of Cost/ Bill

#### [Support levels]([AWS Support Plan Comparison | Developer, Business, Enterprise, Enterprise On-Ramp | AWS Support (amazon.com)](https://aws.amazon.com/premiumsupport/plans/))
1. Free 
2. Developer 
3. Business - Full Technical Advisor Access 
4. Enterprise - TAM 

- Marketplace - Tools for AWS

## Migration and Innovation
---
### Cloud Adoption Framework
- Quick and Smooth migration
- Perspectives 
	1. Business Capabilities 
		1. Business
			IT  
		1. People 
			HR 
		2. Governance
			CIO: Chief Information Officer
	1. Technical Capabilities 
		1. Platform 
			CTO: Cheif Technical Officer
		2. Security 
			CISO: Cheif Information Security Officer 
		3. Operations
			IT operations managers and Support Managers 
			
- Six Migration Stategies: 
![Pasted image 20221018171153.png](Pasted%20image%2020221018171153.png)

#### Snow Family 
- Offline Migration Strategies 

![Pasted image 20221018171614.png](Pasted%20image%2020221018171614.png)

#### Well Architected Framework
1. Operational Excellence 
2. Security 
3. Reliability 
4. Performance Efficiency 
5. Cost Optimization
6. Sustainability