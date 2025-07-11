🚀 AWS 2-Tier Web Application Deployment
📝 1. Project Overview
This project demonstrates how to deploy a secure and scalable 2-tier web application with vpc on AWS using infrastructure-as-code with CloudFormation.

🔧 What This Project Includes:


🛡️ A custom VPC split into public and private subnets
🖥️ A public EC2 instance that acts as both:
A web server (running Apache Tomcat)
A bastion host to access private resources
🛠️ A private EC2 instance used to test database connections securely
🗄️ An RDS MySQL database in the private subnet
💻 A Java web application hosted on Apache Tomcat, connected to RDS
🎯 Purpose:
Demonstrate network isolation using subnets
Ensure secure database access via a bastion host
Deploy a full-stack Java app using CI-like steps with Maven and Tomcat
Practice CloudFormation templating
🗺️ 2. Architecture Diagram
🧩 You can create this using draw.io, Lucidchart, or embed an image in your final report.

       [ Your Laptop ]
             |
           SSH
             ↓
  [ Public EC2 (Bastion + App) ]
             |
          SSH (Internal)
             ↓
  [ Private EC2 (DB Tester) ]
             ↓
    [ RDS MySQL in Private Subnet ]

    
It should include:

A VPC with:
🌐 Public Subnet (e.g., us-east-1a)
🔒 Private Subnet (e.g., us-east-1b)
EC2 instances:
☁️ Public EC2 = App + Bastion Host
🔐 Private EC2 = DB Test Client
📶 Internet Gateway connected to public route table
🧷 NAT (optional for outbound access if needed)
🗄️ RDS MySQL in the private subnet

🧰 3. Prerequisites
To successfully deploy this project, you need:

✅ An AWS account
🔑 A key pair (.pem) to SSH into EC2 instances
📦 Basic tools:
Git, Java (JRE), Maven
Apache Tomcat (manual or script-based setup)
🧠 Knowledge of:
Linux shell basics
MySQL operations
AWS EC2, VPC, and RDS
💻 Terminal or SSH client (e.g., PuTTY for Windows)

 4. VPC Setup Using CloudFormation
You provisioned networking infrastructure with a CloudFormation YAML template that creates:

A VPC (10.0.0.0/16)
Two subnets:
10.0.0.0/20 (public)
10.0.16.0/20 (private)
Internet Gateway and public routing
Route tables for subnet association
📄 Tip: Include your YAML file in your project folder or repository as vpc-setup.yaml.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/95f07b54-1de5-4e94-bd09-1b07701cf3b4" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/945975bc-c9db-46a0-b4e0-7f7060fdf9f0" />

<img width="1920" height="1080" alt="Screenshot 2025-07-09 21 27 21" src="https://github.com/user-attachments/assets/94fae3b9-3e6c-46fb-8d25-caac49c72e25" />

<img width="1920" height="1080" alt="Screenshot 2025-07-09 21 27 33" src="https://github.com/user-attachments/assets/711bffe1-174e-428a-930b-800b48243262" />

<img width="1920" height="1080" alt="Screenshot 2025-07-09 21 27 45" src="https://github.com/user-attachments/assets/9b1dfcca-f8c6-4575-9b02-98afef077ab6" />

<img width="1920" height="1080" alt="Screenshot 2025-07-09 21 34 40" src="https://github.com/user-attachments/assets/24720b43-3563-4689-9968-53e54452cf49" />

1. Project Overview
This project demonstrates how to deploy a secure and scalable 2-tier web application with vpc on AWS using infrastructure-as-code with CloudFormation.

🔧 What This Project Includes:
🛡️ A custom VPC split into public and private subnets
🖥️ A public EC2 instance that acts as both:
A web server (running Apache Tomcat)
A bastion host to access private resources
🛠️ A private EC2 instance used to test database connections securely
🗄️ An RDS MySQL database in the private subnet
💻 A Java web application hosted on Apache Tomcat, connected to RDS








