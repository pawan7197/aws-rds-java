✅ Step 1: Create CloudFormation Stack

Objective: Provision VPC, public/private subnets, and EC2 instances using a CloudFormation YAML template.

Log in to the AWS Management Console.

Navigate to CloudFormation service.

Click on Create Stack > With new resources (standard).

Under "Specify template", select Upload a template file.

Upload the YAML file (e.g., AJA_network_with_EC2.yaml).

Click Next.

Enter a suitable Stack Name, such as MyAppStack.

Click through the configuration pages and review all settings.

Finally, click Create Stack.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/8a0363b6-9600-4534-9616-6e794b2a13eb" />

Wait until the status shows CREATE_COMPLETE.

✅ Step 2: Validate EC2 Instances

Objective: Confirm both EC2 instances are created and belong to the same VPC.

Navigate to the EC2 Dashboard.

Under Instances, confirm the following:

A Public EC2 Instance with a public IP.

A Private EC2 Instance without a public IP.

Check the VPC ID of both instances to ensure they are in the same VPC created by the stack.

✅ Step 3: Configure Security Group (Public Server)

Objective: Open necessary ports for SSH, database access, and application traffic.

Go to Security Groups in the EC2 console.

Select the Security Group attached to the Public EC2.

Edit Inbound Rules and add the following:

SSH: TCP Port 22 from 0.0.0.0/0

MySQL/Aurora: TCP Port 3306 from 0.0.0.0/0

HTTP (App): TCP Port 8080 from 0.0.0.0/0

✅ Step 4: Set Up NAT Gateway

Objective: Allow private EC2 to access the internet for downloading packages.

Go to VPC Dashboard > Elastic IPs, and allocate a new Elastic IP.

Navigate to NAT Gateways > Create NAT Gateway:

Select the Public Subnet.

Attach the newly created Elastic IP.

Go to Route Tables > Private Route Table.

Edit routes and add the following:

Destination: 0.0.0.0/0

Target: Your NAT Gateway ID

✅ Step 5: Connect to Private EC2 via Jump Host

Objective: Use the Public EC2 instance as a jump host to access the Private EC2.

SSH into the Public EC2 instance:

ssh -i key.pem ec2-user@<public-ec2-public-ip>

From the Public EC2 terminal, SSH into the Private EC2:

ssh -i key.pem ec2-user@<private-ec2-private-ip>

✅ Step 6: Install Required Packages on EC2s

Objective: Prepare environment with required tools on both EC2 instances.

Run the following commands on both instances:

sudo yum update -y
sudo yum install git maven java-1.8.0-openjdk -y

✅ Step 7: Clone Java Application Repository

Objective: Download the Java application code from GitHub.

On the Private EC2, run:

git clone https://github.com/Ai-TechNov/aws-rds-java.git
cd aws-rds-java

✅ Step 8: Install and Start Apache Tomcat

Objective: Deploy Java WAR file to Tomcat application server.

Run the following:

sudo yum install tomcat -y
sudo systemctl start tomcat
sudo systemctl enable tomcat

✅ Step 9: Set Up MySQL Database

Objective: Create a database and table to support the Java app.

Install MySQL server on the Private EC2:

sudo yum install mysql-server -y
sudo systemctl start mysqld

Open MySQL shell:

mysql -u root

Execute the following SQL commands:

CREATE DATABASE jwt;
USE jwt;
CREATE TABLE USER (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50),
  email VARCHAR(50),
  password VARCHAR(100)
);

✅ Step 10: Build and Deploy the Java Application

Objective: Package the application using Maven and deploy it to Tomcat.

Run the following in the app directory:

mvn clean package
cp target/*.war /usr/share/tomcat/webapps/app.war

✅ Step 11: Access the Deployed Application

Objective: Verify successful deployment from the browser.

Open your web browser.

Visit:

http://<public-ec2-public-ip>:8080/app

You should see the Java application UI, backed by the database running on Private EC2.

✅ Final Result

You have successfully deployed a Java-based web application on AWS using:

AWS CloudFormation for infrastructure

EC2 (public & private)

NAT Gateway & Jump Hosting

Git + Maven + Tomcat for application setup

MySQL for database

This setup is scalable, secure, and cloud-native for production-like deployments.



