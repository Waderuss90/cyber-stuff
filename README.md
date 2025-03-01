
Reading
-------
- AWS Exam Guide
- Sample Questions
- Practice Exam ($20)
- FAQs EC2
- FAQs S3 !!!
- FAQs ELB (even better: FAQs for all LBs)
- Architecting for the Cloud: AWS Best Practices
- AWS Well-Architected Framework
- AWS Storage Services Overview
- FAQs EBS
- FAQs VPC
- FAQs Route 53
- FAQs RDS
- FAQs SQS
- AWS Security Best Practice white paper
- Overview of Security Processes
- Check cost management in cloud practitioner

Check
- S3 Transfer Acceleration Tool


Amazon EC2 || Amazon S3 || Amazon  || Amazon

Interesting AWS Certs
---------------------
Certified Solutions Architect – Associate
Certified Developer – Associate
Certified SysOps Admin – Associate
Certified Advanced Networking – Specialty
Certified DevOps – Professional
Certified Solutions Architect – Professional
Certified Security – Specialty
Certified Big Data – Specialty
Certified Machine Learning – Specialty


Abbreviations
-------------
ACM     AWS Certificate Manager
ALB     Application Load Balancer
AMI     Amazon Machine Image
ARN     Amazon Resource Name
AZ     	Availability Zone
CORS	Cross Origin Resource Sharing
CRR     Cross-region replication
DAX     DynamoDB Accelerator
DC     	dense compute, Redshift
DS     	dense storage , Redshift
EBS     Elastic Block Store
EC2     Elastic Compute Cloud
ECS     EC2 Container Service
ECU     EC2 compute units
EFS     Elastic File System
EKS     Elastic Container Service for Kubernetes
EMR     Elastic MapReduce
HVM     Hardware Virtual Machine
IAM     AWS Identity and Access Management
JWT     JSON Web Token
KMS     Key Management Service
OLAP	Online Analytics Processing
OLTP	Online Transaction Processing
PV     	Paravirtual
RDS     Relational Database Service
S3      Simple Storage Service
SES     Simple Email Service
SNS     Simple Notification Service
SQS     Simple Queue Service
SSE     Server Side Encryption (SSE-S3, SSE-KMS, SSE-C)
STS     Security Token Service
SWF     Simple WordFlow Service
VTL     gateway-virtual tape library
VTS     virtual tape shelf
WAF     Well-Architected Framework
WAF     Web Application Firewall


Regions
-------
Jan 2020:
22 geographic regions
69 Availability Zones


EC2 instance
------------

Pricing:
- on demand
- reserved (one or three years)
- spot (price depends on supply & demand)
- dedicated


S3
--
https://docs.aws.amazon.com/s3/index.html

Unlimited storage
Object based storage.
Key-value store; incl. version ID and metadata (content-type, last-modified).
	Key = object name; value = data itself
Objects stored in buckets (folders).
S3 doesn't not support running an OS or a DB
Universal namespace (globally unique) !
Availability: 99.95 - 99.99
Durability:	  11 9's
>= 3 AZs

Tiered storage
Lifecycle management
Versioning

Encryption
Bucket policies on bucket level, not object level
Bucket ACL (tied to bucket or object)
S3 access logs (not enabled by default)
AWS Policy Generator also for bucket policies.

S3 secure by default:
- no public acces
- access by ownwer only


Encryption:
Encryption in transit, HTTPS
Encryption at rest - Client side encryption
Encryption at rest - Server side encryption
	SSE-S3		S3 managed keys, AES-256
	SSE-KMS		aws:kms
	SSE-C		Customer provided key

Enforcing Server side encryption:
- At creation time, or
- Via bucket policy (deny PUT request without x-amz-server-side-encryption)

x-amz-server-side-encryption request header, e.g. in HTTP PUT
- aes-256
- aws:kms


Object lock (implicitly enables versioning)
WORM	Write-Once-Read-Many

Depending on your Region, your Amazon S3 website endpoint follows one of these two formats.
s3-website dash (-) Region	http://bucket-name.s3-website-Region.amazonaws.com
s3-website dot (.) Region	http://bucket-name.s3-website.Region.amazonaws.com


S3 url:	https://bucket-name.s3.region-code.amazonaws.com/key-name
		https://tisipi.s3.eu-central-1.amazonaws.com/MY_FILE


0-5 TB objects
durability      11 nines (99.999999999%)
Availability    99.99%
SLA             99.9%
default buckets 20

S3 Standard             99.99%; SLA 99,9%   Frequent access; default; highest cost, no access fee
S3 Intelligent Tiering  99.9%; SLA 99%      Frequent and Infrequent access; unkown access patterns
S3 Standard-IA;         99.9%; SLA 99%      S3-IA, Infrequent Access; not dayly access; lower fee, but retrieval fee
S3 One Zone-IA;         99.5%; SLA 99%      One Zone Infrequent Access; 20% less than S3-IA; non-critical data
S3 Glacier              99.99%; SLA 99,9%   Archiving, very infrequent access (2-3 times/year), very cheap, but pay for access; 1 min to 12 h Retrieval time
S3 Glacier Deep Archive 99.99%              Lowest cost, long-term, retrieval time within 12 h; alternative to magnetic tape libraries

Glacier 99.99% or 99.9% ??

S3 Outposts

S3 RRS (reduced redundancy storage) will be phased out. Similar to S3 One Zone


Create a Static Website Using Amazon S3		https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html


SOP and CORS
------------
Same Origin Policy (SOP)
- Two URLs have the same origin if the protocol, port (if specified) and host are the same for both.
- A security mechanism that restricts how a document or script loaded by one origin can interact with a resource from another origin.

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, scheme, or port) than its own from which a browser should permit loading of resources. CORS also relies on a mechanism by which browsers make a “preflight” request to the server hosting the cross-origin resource, in order to check that the server will permit the actual request. In that preflight, the browser sends headers that indicate the HTTP method and headers that will be used in the actual request.

The CORS policy defines specific HTTP headers that need to be included in the request/response interaction;
allowing the server to communicate which origins it will allow requests from.
The browser then enforces this by allowing or preventing scripts from accessing the response.

Unlike “simple requests”, for "preflighted" requests the browser first sends an HTTP request using the OPTIONS method to the resource on the other origin, in order to determine if the actual request is safe to send. Cross-site requests are preflighted like this since they may have implications to user data.


How does Amazon S3 evaluate the CORS configuration on a bucket?
When Amazon S3 receives a preflight request from a browser, it evaluates the CORS configuration for the bucket and uses the first CORSRule rule that matches the incoming browser request to enable a cross-origin request. For a rule to match, the following conditions must be met:
- The request's Origin header must match an AllowedOrigin element.
- The request method (for example, GET or PUT) or the Access-Control-Request-Method header in case of a preflight OPTIONS request must be one of the AllowedMethod elements.
- Every header listed in the request's Access-Control-Request-Headers header on the preflight request must match an AllowedHeader element.

	<CORSConfiguration>
		<CORSRule>
			<AllowedOrigin>*</AllowedOrigin>
			<AllowedMethod>GET</AllowedMethod>
			<MaxAgeSeconds>3000</MaxAgeSeconds>
			<AllowedHeader>Authorization</AllowedHeader>
		</CORSRule>
	</CORSConfiguration>



EBS volume types
----------------
- gp2                   General Purpose SSD             boot disks & general applications, < 16000 IOPS/volume, 99.9% durability
- io1                   Provisioned IOPS SSD            OLTP, latency sensitive applications, < 64000 IOPS/volume, 50 IOPS/GB, 99.9% durability
- io2                   Provisioned IOPS SSD            OLTP, latency sensitive applications, < 64000 IOPS/volume, 500 IOPS/GB, 99.999% durability
                                                        pricing io1 and io2 is the same; io2 is later generation

- st1                   Throughput Optimized HDD        big data, warehouse, ETL; 500 MB/s/volume; NO boot volume, 99.9% durability
- sc1                   Cold HDD                        less frequently accessed data; lowest cost, 50 MB/s/volume; NO boot volume; 99.9% durability
- magnetic (standard)   Magnetic HDD                    obsolete?

Root volume only on SSD or Magnetic


Databases, etc.
---------------
RDS for OLTP
Datawarehouse like Redshift for OLAP

RDS backup retention period: 1-35 days (default retention period: 7 days)

RDS Databases:
- SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- MariaDB
- Aurora (Amazon)


Multi-AZ (for DR):
- SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- MariaDB
(Aurora 18 copies)?


Read Replicas (for performance):
- Oracle
- MySQL Server
- PostgreSQL
- MariaDB
- Aurora
(SQL Server not supported)

Five read replicas          (Backup must be enable to create them)
	15 aurora read replicas
Multi-AZ Read replicas      (cross-AZ)
Read replica in 2nd region  (cross-region)


RDS Backup:
- Automated backup  (enabled by default; implemented by snapshots and transaction logs)
- Database snapshot (manually, no retention period; backup to known state)
- Note: A restored RDS instance has a NEW RDS endpoint!

RDS encryption
- At creation time.
- AWS KMS service using AES-256 encryption.
- Or convert via snapshot: unencrypted DB --> unencrypted snapshot --> encrypted snapshot --> restore as encrypted DB.


Aurora / MySQL port number	3306


DynamoDB (no SQL)


ElastiCache:
- In-memory cache 
- Two options:
	1. Memcached    (simple, object caching, key/value; no persistence/failover, no multi-AZ)
	2. Redis        (key/value, sorting/ranking; 'complexer' datatypes; persistence/failover, multi-AZ)

ElastiCache help DB under stress when:
- read-heavy
- no frequent changes

ElastiCache does not help DB under stress when:
- Heavy write loads
- Complex, long-running OLAP queries


Boot strap script EC2 with MySQL client:
	#!/bin/bash
	yum update -y
	# Install MySQL client to connect to DB
	yum install mysql -y


# Check installation
mysql --version

# Connect to DB using endpoint 
# -p will prompt for password
mysql -u <user mame> -p -h <RDS_ENDPOINT> <db name>

	# Check the status of DB:
	status
	show databases;

	# Quit the database connection:
	exit



Route 53
--------
DNS service. Maps domain name to:
- EC2 instance
- Elastic load balancer
- S3

Hosted zone
A record
Alias point to the zone apex (the root of the domain)

50 domain names (per account). Can be lifted.
500 hosted zones (per account)


Serverless
----------
Lambda
API Gateway
DynamoDB
Aurora Serverless
Athena
S3...

Languages:
- Node.js
- Java
- Python
- C#
- Go
- PowerShell
- Ruby?

Lambda triggers:
- API gateway
- AWS IoT
- ALB
- CloudWatch events and logs
- CodeCommit
- Cognito Sync Trigger
- DynamoDB
- Kinesis
- S3
- SNS
- SQS
- Alexa Skills Kit

AWS X-ray for debugging


Billing notification
--------------------
# Billing notification on e.g. 10 USD:
CloudWatch > Alarms > Billing >  Create Alarm


SSH Keypairs
------------
# Download generated Keypair, e.g. MyKP.pem from AWS

# Login into EC2 instance (Linux, Mac)
chmod 400 MyKP.pem
ssh ec2-user@<ipAddress> -i MyKP.pem
sudo su

# Login into EC2 instance (Windows)
Install and run SSH Chrome Extension
wincmd> ssh-keygen -y -f MyKP.pem > MyKP.pub
wincmd> ren MyKP.pem MyKP
Import MyKP.pub and MyKP into SSH Chrome app


Move instance to another AZ
---------------------------
Create snapshot.
Turn snapshot into AMI image.
Launch your image in another AZ.

Note: You can create AMI from a volume (i.o. snapshot)


Move instance to another Region
-------------------------------
Copy AMI to another region.
Launch


Create Encrypted root device volume
-----------------------------------
Option 1:
Select root device encryption during creation of EC2 instance.

Option 2:
Take snapshot of unencrypted root device volume.
Copy snapshot to an encrypted snapshot (select encrypt option).
Create an AMI from this encrypted snapshot.
Launch EC2 instance with encrypted root device volume.


AWS CLI Access to EC2 instance
------------------------------
# Enter AWS Access Key ID
# Enter AWS Secret Access Key
#
aws configure

aws s3 ls
aws s3 ls --region us-east-1

aws s3 mb s3://tisipi-test

# NOTE: SECURITY RISK! CREDENTIALS ARE STORED IN HOME DIRECTORY:
cd
cd .aws
ls
# This directory has a config and credentials file
cat credentials

DON'T DO THIS: ATTACH A ROLE TO THE EC2 INSTANCE INSTEAD!!


VPC
---
Create VPC with CIDR block
Create subnets in AZs
	Auto-assign public IPv4 address, if needed, for public subnets
Create IGW
Attached IGW to VPC
Create Public route table
	Add default route to IGW
Associate (public) subnets to (public) route table(s)
Launch EC2 instance
	VPC and subnet
	Security group


Boot strap script
-----------------
Instance creation
	-> Advanced Details
	-> User data, as text: your bash script

#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1>Meh, meh, wat inne schoene bootstrapped surver :-)</h1></html>" > index.html
aws s3 mb s3://tisipi-com
aws s3 cp index.html s3://tisipi-com


Apache Web Server
-----------------
#!/bin/bash
# Check for any updates for AWS Linux image
yum update -y
# Install Apache
yum install httpd -y
# Make HTML page
cd /var/www/html
echo "<html><h1>Bootstrapped web surver 01 :-)</h1></html>" > index.html
#vi index.html
#<html><h1>Hello, world </h1></html>
# Start Apache and save config
service httpd start
chkconfig httpd on


Wordpress Server
----------------
#!/bin/bash
yum update -y
yum install httpd php php-mysql -y
cd /var/www/html
wget https://wordpress.org/wordpress-5.1.1.tar.gz
tar -xzf wordpress-5.1.1.tar.gz
cp -r wordpress/* /var/www/html/
rm -rf wordpress
rm -rf wordpress-5.1.1.tar.gz
chmod -R 755 wp-content
chown -R apache:apache wp-content
service httpd start
chkconfig httpd on


EFS/NFS Mount
-------------
# Install the EFS mount helper on Amazon Linux
yum install -y amazon-efs-utils

# NFS client on RHEL or Suse
sudo yum install -y nfs-utils
# NFS client on Ubuntu
sudo apt-get install -y nfs-common


# Example:
#sudo mount -t efs -o tls fs-4ab216cb:/ /mnt/efs
mount -t efs -o tls fs-4ab216cb:/ /var/www/html


EC2 Meta Data
-------------
# Check bootstrap data of EC2 instance
curl http://169.254.169.254/latest/user-data/

curl http://169.254.169.254/latest/meta-data/
curl http://169.254.169.254/latest/meta-data/local-ipv4
curl http://169.254.169.254/latest/meta-data/public-ipv4



++++++

aws ec2 attach-internet-gateway --vpc-id "vpc-091a873767d93516c" --internet-gateway-id "igw-05dc6145b24b450f6" --region us-east-1




AWS Certified Developer - Associate 2020
=======================================
https://www.freecodecamp.org/news/how-i-passed-the-aws-certified-developer-associate-exam/
freeCodeCamp 	https://www.youtube.com/watch?v=RrKRN9zRBWs	12h
https://tutorialsdojo.com/aws-certified-developer-associate/

AM Policy Simulator Console     https://policysim.aws.amazon.com/
Pricing calculator              https://calculator.aws/#/


Github ACloudGuru	https://github.com/ACloudGuru-Resources/course-aws-certified-developer-associate


IAM
---
Root account
IAM user

User/group/role
Principal/resources

Console Access:
	IAM user name (plus account ID or alias)
	Password
	opt. MFA code

Programmatic access:
	Access key ID
	Secret access key (view or download only once)
	Up to two active access keys 

Other credentials for IAM Users:
- X.509 certificates for SOAP APIs
- GIT credentials (SSH keys or passwords) to interact with AWS CodeCommit

Roles are the preferred option from a security perspective.
Avoid hard coding credentials: ATTACH A ROLE TO THE EC2 INSTANCE!


Principal that assumes a role, gets short-term credentials through AWS Security Token Service (AWS STS):
- AccessKeyId
- SecretAccessKey
- SessionToken
- Expiration

Trust policy specifies which principals can assume a role. For example allow Amazon EC2 to request short-term credentials associated with an IAM role:

	{
	"Version": "2020-11-11",
	"Statement": [
		{
		"Effect": "Allow",
		"Principal": {
			"Service": "ec2.amazonaws.com"
		},
		"Action": "sts:AssumeRole"
		}
	]
	}


IAM User or Role?

	Use roles to make AWS API calls from untrusted machines (short term credentials)

	Code Running on                                         Suggestion
	---------------                                         ----------
	Local development laptop or on-premises server          IAM user
	Deploying Code to AWS                                   IAM role
	EC2 instance                                            IAM role
	An IAM user mobile device                               IAM role
	Enterprise environment with external identity provider  IAM role
	Client-side code uploading data to S3                   IAM role
	Client-side code interacting with DynamoDB              IAM role


ARN formats:
	arn:partition:service:region:account-id:resource
	arn:partition:service:region:account-id:resourcetype/resource
	arn:partition:service:region:account-id:resourcetype:resource

ARN examples:
	arn:aws:iam::0123456789:user/mike
	arn:aws:s3:::my-bucket/my-object.png
	arn:aws:polly:us-west-1:0123456789:lexicon/awsLexicon



Systems Manager Parameter Store
-------------------------------
Management & Governance > Systems Manager > Parameter Store

- Storage of secrets and configuration data like passwords, database strings, license codes.
- Separate your secrets from your code.
- Parameters can be tagged and organized into hierarchies. For example: "dev/db-string” and “prod/db-string".
- Plain text or encrypted.
	- Integrated with AWS KMS to encrypt the data.
- Control user and resource access to parameters using IAM.
- Reference by name, eg. in bootstrap script
- Integrated with AWS services like EC2, CloudFormation, Lambda, CodeBuild, CodePipeline, CodeDeploy


AWS CLI
-------
CLI command reference   https://docs.aws.amazon.com/cli/latest/index.html
                        https://awscli.amazonaws.com/v2/documentation/api/latest/index.html

Supported on Linux, Windows and MacOS
Also available on AMI Linux EC2 instances.


# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

# Configure AWS CLI and credentials
aws configure
	AWS Access Key ID [None]: ********************
	AWS Secret Access Key [None]: *******************
	Default region name [None]: eu-central-1
	Default output format [None]: json]

	Credentials file: 	~/.aws/credentials
		[default]
		aws_access_key_id = xxxxx
		aws_secret_access_key = yyyy

	Config file: 		~/.aws/config
		[default]
		output = json
		region = eu-central-1


Alternatively, set environment variables:
	export AWS_ACCESS_KEY_ID=********************
	export AWS_SECRET_ACCESS_KEY=*******************/K7MDENG/bPxRfiCYEXAMPLEKEY
	export AWS_DEFAULT_REGION=eu-central-1


aws configure list
aws configure list-profiles
aws configure get region


To send only the stderr diagnostic information to debug file:
	aws servicename commandname options --debug 2> debug.txt

To send both the output and stderr diagnostic information:
	aws servicename commandname options --debug &> debug.txt

# AWS CLI help
aws help
aws ec2 help

# List Buckets
aws s3 ls
aws s3 ls --region us-east-1

# List contents of bucket
aws s3 ls s3://www.tisipi.nl

# Make bucket
aws s3 mb s3://tisipi-xxx

# Remove bucket
aws s3 rb s3://tisipi-xxx

# Copy file to bucket
aws s3 cp index.html s3://tisipi-com


# Delete a single s3 object:
aws s3 rm s3://mybucket/test2.txt

# Recursively delete all objects under a specified bucket
aws s3 rm s3://mybucket --recursive

# Recursively delete all objects under a specified bucket, but exclude some objects
aws s3 rm s3://mybucket/ --recursive --exclude "*.jpg"

# Recursively delete all objects under a specified bucket, but exclude objects under prefix
aws s3 rm s3://mybucket/ --recursive --exclude "another/*"


# AWS CLI pagination
# Default page size 1000
# --page-size 1000

aws s3api list-objects --bucket tisipi-com --page-size 100
aws s3api list-objects --bucket tisipi-com --max-items 100


Python virtual environment
--------------------------
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Deactivate virtual environment
deactivate


Boto3
-----
http://boto3.readthedocs.io/

AWS SDK for Python (Boto3)

pip install boto3

import boto3

print("List all S3 buckets in your AWS account using resources:")
s3 = boto3.resource("s3")
for bucket in s3.buckets.all():
    print(bucket.name)

print("List all S3 buckets in your AWS account using client:")
s3_client = boto3.client("s3")
bucket_data = s3_client.list_buckets()
for bucket in bucket_data["Buckets"]:
    print(bucket["Name"])


print("List all objects in bucket")
s3 = boto3.resource("s3")
bucket = s3.Bucket(MY_BUCKET)
for obj in bucket.objects.all():
    print(obj.key)


print("Upload a file to your S3 bucket")
with open(MY_FILE, "rb") as file_data:
    s3 = boto3.resource("s3")
    try:
        s3.Bucket(MY_BUCKET).put_object(Key=MY_FILE, Body=file_data)
    except:
        print("Error. Bucket does not exist")


print("Retrieve the website configuration of S3 bucket")
s3 = boto3.client('s3')
result = s3.get_bucket_website(Bucket='MY_BUCKET')


print("Convert text to speech")
polly = boto3.client("polly")
voice_output = polly.synthesize_speech(
    Text="Hi Mister Tisipi!", OutputFormat="mp3", VoiceId="Brian"
)
audio = voice_output["AudioStream"].read()
with open("audio.mp3", "wb") as file:
    file.write(audio)



polly.eu-central-1.amazonaws.com/v1/speech:

	POST /v1/speech HTTP/1.1 host: polly.eu-central-1.amazonaws.com
	content-Type: application/json
	x-amz-date: 20210516T051402Z
	authorization: AWS4-HMAC-SHA256 Credential=AKIAIO5FODNN7EXAMPLE/20210516/eu-central-1/polly/aws4_request, SignedHeaders=content-length;content-type;host;x-amz-date, Signature=d968197e88a6a8de69d1a7bcab414669eecd5f841e13dc90e4a7852c2c428038

	{
	       "OutputFormat": "mp3",
	       "Text" : "Hi Mister Tisipi!",
	       "VoiceId": "Brian"
	}



Expand an acronym in the generated audio file by an XML lexicon with a grapheme:

aws-lexicon.xml:

	<?xml version="1.0" encoding="UTF-8"?>
	<lexicon version="1.0"
		...
		alphabet="ipa"
		xml:lang="en-US">
	<lexeme>
		<grapheme>AWS</grapheme>
		<alias>Amazon Web Services</alias>
	</lexeme>
	</lexicon>

# Synthesizing speech with this custom lexicon
# Note: regional in scope
#
aws polly put-lexicon --name awsLexicon --content file://aws-lexicon.xml --region us-west-2
aws polly synthesize-speech --text 'Hello AWS!' --voice-id Brian --output-format mp3 hello.mp3 --lexicon-names="awsLexicon" --region us-west-2


List of AWS services and their regional API endpoints:
	https://docs.aws.amazon.com/general/latest/gr/rande.html.

The general syntax of a Regional endpoint is as follows:
	protocol://service-code.region-code.amazonaws.com
	for example
	https://dynamodb.us-west-2.amazonaws.com


Region Name                 Region Code
US East (Ohio)              us-east-2
US East (N. Virginia)       us-east-1
US West (N. California)     us-west-1
US West (Oregon)            us-west-2
Africa (Cape Town)          af-south-1
Asia Pacific (Hong Kong)    ap-east-1
Asia Pacific (Mumbai)       ap-south-1
Asia Pacific (Osaka)        ap-northeast-3
Asia Pacific (Seoul)        ap-northeast-2
Asia Pacific (Singapore)    ap-southeast-1
Asia Pacific (Sydney)       ap-southeast-2
Asia Pacific (Tokyo)        ap-northeast-1
Canada (Central)            ca-central-1
China (Beijing)             cn-north-1
China (Ningxia)             cn-northwest-1
Europe (Frankfurt)          eu-central-1
Europe (Ireland)            eu-west-1
Europe (London)             eu-west-2
Europe (Milan)              eu-south-1
Europe (Paris)              eu-west-3
Europe (Stockholm)          eu-north-1
Middle East (Bahrain)       me-south-1
South America (São Paulo)   sa-east-1



---

#!/bin/bash
yum update -y
yum install httpd -y
cd /var/www/html
echo "<html><body><h1>Bootstrapped web surverke :-)</h1></body></html>" > index.html
systemctl start httpd
systemctl enable httpd
systemctl status httpd

+++
versus: ?
service httpd start
chkconfig httpd on
++++

Load balancer:
- ALB Application load balancer  HTTP/HTTPS, request type
- Network load balancer			TCP, low latency; most expensive
- Classic load balancer			HTTP/HTTPS and TCP; legacy
- Gateway load balancer			3rd party virtual appliances

X-Forwarded-For HTTP header 	IPv4 address end user; ALB and Classic


HTTP response codes:
1xx		Informational
2xx		Success
3xx		Redirect
4xx		Client error
5xx		Server Error

200 OK

504 Gateway Timeout





AWS Certified Advanced Networking - Specialty
=============================================
Rick Crisci         https://www.linkedin.com/in/rickcrisci/
AWS landing page    https://aws.amazon.com/certification/certified-advanced-networking-specialty/

#TODO:
- IPSpace
- Check bookmarks

https://docs.aws.amazon.com/vpc/latest/userguide/
https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html


Primary IPv4 CIDR range cannot be resized (it can be deleted)
4 secondary IPv4 CIDR blocks can be added though
1 Primary IPv6 block can be attached to VPC
NO secondary IPv6 CIDR blocks can be added though.
BUT one IPv6 CIDR blocks can be added to a subnet!



VPC Limits
----------
AWS VPC Limits 		https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html

Per account.
Most are soft limits and can be adjusted.

VPCs per Region                         5       (100s)       
Subnets per VPC                         200                  
IPv4 CIDR blocks per VPC            	5       (50)         
IPv6 CIDR blocks per VPC                1                    

Elastic IP addresses per Region         5                    

Network ACLs per VPC                    200                  
Rules per network ACL                   20      (40)         

Route tables per VPC                    200
Routes per route table                  50      (1,000)      (non-propagated routes)
BGP advertised routes per route table   100     (No)         (propagated routes)


VPC security groups per Region          2,500                
Inbound rules per security group        60                   
Outbound rules per security group       60                   
Security groups per network interface   5       (16)         

Enforced separately for IPv4 rules and IPv6 rules.
#(security groups per network interface) * #(rules per security group) <= 1,000. 


FAQs
----
https://aws.amazon.com/vpc/faqs
https://aws.amazon.com/vpc/faqs/#IP_Addressing


ENI		Elastic Network Interface



Five reserved IP addresses in subnet (incl. network and broadcast):
- base:		reserved
- 1st:		gateway
- 2nd:		dns
- 3rd:		future use
- last:		reserved


VPC with public and private subnets		https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html
