AWS 2-Tier Web Application Deployment — Full Documentation
1. Project Objective
Deploy a secure and scalable 2-tier Java-based web application on AWS, using CloudFormation (IaC) to manage infrastructure. It includes:

Custom VPC

Public/Private EC2 instances

Bastion host architecture

RDS MySQL backend

Apache Tomcat + Maven-built Java web app

2. Architecture Diagram
Use a tool like draw.io or Lucidchart to draw this:

csharp
Copy
Edit
   [Your Laptop]
         |
        SSH
         ↓
 [Public EC2 (Web + Bastion)]
         |
        SSH
         ↓
 [Private EC2 (DB Tester)]
         ↓
 [RDS MySQL in Private Subnet]
3. Prerequisites
Before deployment:

✅ AWS Account
✅ AWS Key Pair (.pem)
✅ Tools: Git, Java (JRE), Maven, Apache Tomcat
✅ Basic Knowledge: EC2, VPC, SSH, MySQL
✅ Terminal or PuTTY (for SSH from Windows)

4. VPC Setup with CloudFormation
🛠 Use the following components in a YAML file (vpc-setup.yaml):

a. Create VPC
CIDR: 10.0.0.0/16

b. Create Subnets
Public Subnet: 10.0.0.0/20 (AZ: us-east-1a)

Private Subnet: 10.0.16.0/20 (AZ: us-east-1b)

c. Create:
Internet Gateway

Public Route Table with IGW association

Route Table Associations

Optional: NAT Gateway (for outbound internet from private subnet)

✅ Steps:
Go to AWS → CloudFormation

Click Create Stack → With new resources

Upload vpc-setup.yaml

Click Next, provide stack name → My2TierApp

Click Create Stack

Wait for status: CREATE_COMPLETE

5. EC2 Instance Setup
🟢 Public EC2: Web Server + Bastion Host
AMI: Ubuntu 20.04 or Amazon Linux

Subnet: Public

Security Group Inbound Rules:

SSH (22) → Your IP

HTTP (8080) → Anywhere (0.0.0.0/0)

Install: Git, Java, Maven, Tomcat

Deploy app + allow SSH to private instance

🔒 Private EC2: DB Tester
AMI: Ubuntu/Amazon Linux

Subnet: Private

Security Group Inbound:

SSH (22) → Allow only from Public EC2 Private IP

6. SSH Access Using Bastion Host
From Local:

bash
Copy
Edit
ssh -A -i my-key.pem ec2-user@<public-ec2-public-ip>
From Public EC2 to Private EC2:

bash
Copy
Edit
ssh ec2-user@<private-ec2-private-ip>
✅ Tips:

Use SSH Agent Forwarding (-A)

Ensure .pem is not copied to public EC2

Security group allows internal traffic on port 22

7. RDS MySQL Database Setup
Go to RDS → Create Database:

Engine: MySQL

Subnet group: Private Subnet

Public Access: No

Credentials: admin / password

VPC: Select your custom VPC

✅ Once created:

Note the endpoint (e.g., database-1.c7k4u266e1cr.us-east-1.rds.amazonaws.com)

From Private EC2, test connection:

bash
Copy
Edit
mysql -h <RDS-endpoint> -u admin -p
8. Deploy Java Web Application on Apache Tomcat
On Public EC2 (App Server):

bash
Copy
Edit
# Update & install dependencies
sudo apt update
sudo apt install git unzip -y
sudo apt install openjdk-17-jre-headless maven -y

# Clone and build the app
git clone https://github.com/shiva7919/aws-rds-java.git
cd aws-rds-java
mvn clean package

# Download Tomcat and deploy WAR
wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.107/bin/apache-tomcat-9.0.107.zip
unzip apache-tomcat-9.0.107.zip
cp target/*.war apache-tomcat-9.0.107/webapps/

# Start Tomcat
cd apache-tomcat-9.0.107/bin
chmod 755 *.sh
sh startup.sh
9. Test the Application
🔗 Open in browser:

pgsql
Copy
Edit
http://<public-ec2-public-ip>:8080/aws-rds-java
Test Pages:

/login.jsp

/userRegistration.jsp

✅ These pages connect to the RDS DB using JDBC — credentials are in app config.

10. Security Group Summary
Resource	Port	Source	Purpose
Public EC2	22	Your IP	SSH from your system
Public EC2	8080	0.0.0.0/0	Web Access (Tomcat)
Private EC2	22	Public EC2 Private IP	SSH via Bastion
RDS MySQL	3306	Private EC2 Private IP	DB Access from App Server

🔒 Follow least privilege and do not open RDS to public.

11. Final Directory Tree
bash
Copy
Edit
aws-rds-java/
├── pom.xml
├── target/
│   └── aws-rds-java.war
└── apache-tomcat-9.0.107/
    └── webapps/
        └── aws-rds-java.war
12. Conclusion
🎯 You successfully:

🏗️ Deployed VPC using CloudFormation

💻 Launched EC2 instances in Public & Private Subnets

🔐 Configured a Bastion Host for secure access

🗄️ Deployed a Java web app using Maven & Tomcat

🔗 Connected app securely to RDS MySQL database

🔍 Verified functionality via browser and DB tests










