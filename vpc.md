✅ Step 1: Create Stack using CloudFormation YAML

Go to AWS Console > CloudFormation

Click Create Stack > With new resources

Choose Upload a template file

Upload your YAML file (provided earlier like AJA_network_with_EC2.yaml)

Click Next, give a stack name like MyAppStack

Review and create the stack

Wait for CREATE_COMPLETE status

✅ Step 2: Confirm Public & Private EC2 Servers

Go to EC2 Dashboard

Verify:

One Public EC2 (with public IP)

One Private EC2 (no public IP)

Ensure both are inside the same VPC created by your stack

✅ Step 3: Open Required Ports on Public Server

Go to Security Groups > Public EC2's SG

Edit Inbound Rules:

SSH: TCP 22

MySQL/Aurora: TCP 3306

HTTP (for app): TCP 8080

✅ Step 4: Setup NAT Gateway

Create Elastic IP in AWS

Go to VPC > NAT Gateways > Create NAT Gateway:

Select public subnet

Attach Elastic IP

Go to Route Tables > Private Route Table:

Add route: 0.0.0.0/0 via NAT Gateway

✅ Step 5: Connect to Private Server via Jump Host (Public EC2)

SSH into Public EC2:

ssh -i key.pem ec2-user@<public-ec2-ip>

From Public EC2, SSH to Private EC2:

ssh -i key.pem ec2-user@<private-ec2-private-ip>

✅ Step 6: Install Required Tools on Both EC2s

Run the following on both EC2s (especially private EC2):

sudo yum update -y
sudo yum install git maven java-1.8.0-openjdk -y

✅ Step 7: Clone GitHub Java App

On the Private EC2:

git clone https://github.com/Ai-TechNov/aws-rds-java.git
cd aws-rds-java

✅ Step 8: Install & Start Tomcat Server

sudo yum install tomcat -y
sudo systemctl start tomcat
sudo systemctl enable tomcat

✅ Step 9: Setup MySQL Database (on Private EC2)

Install MySQL Server:

sudo yum install mysql-server -y
sudo systemctl start mysqld

Login to MySQL:

mysql -u root

Create DB and Table:

CREATE DATABASE jwt;
USE jwt;
CREATE TABLE USER (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50),
  email VARCHAR(50),
  password VARCHAR(100)
);

✅ Step 10: Build & Deploy App with Maven

mvn clean package
cp target/*.war /usr/share/tomcat/webapps/app.war

✅ Step 11: Access the Java App

Open browser: http://<public-ec2-ip>:8080/app

It should load your Java app (connected to the MySQL database on private EC2)

DONE! 🎉 You have successfully deployed a Java application on AWS using Git, CloudFormation, Maven, and Tomcat!

