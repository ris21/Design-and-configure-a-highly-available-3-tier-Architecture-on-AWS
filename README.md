# Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS
How to design and configure a highly available 3-tier Architecture on AWS

![Alt text](https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/Tier3Topology.png)

## Requisite Skills
- AWS Management Console 
- Familiarity with AWS VPC, EC2, and RDS services
- SSH'ing into an instance for testing
  
## Project Description
This project demonstrates how to design and configure a highly available 3-tier Architecture on AWS with the following AWS Services, a Virtual Private Cloud (VPC) with public and private subnets, EC2 instances (Bastion Host, Web Server, App Server), an RDS database, and associated routing and security configurations on AWS. Connectivity between components was tested to ensure proper setup.

## Project Steps
### 1. Created VPC and Subnets;
   
  -A VPC (rispar vpc) was created with a CIDR block of 192.168.0.0/16 and 4 Subnets (Public and private) were configured in 2 AZs.
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/vpc.PNG)
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/subnets.PNG)
   
### 2. Configured Routing and Gateways
   
  -An Elastic IP was allocated and an Internet Gateway (risIGW) was created and attached to (rispar vpc).
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/elasticIP.PNG)
  -A NAT Gateway (risNGW) was set up in PublicSubnet1 using the Elastic IP.
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/NATGW.PNG)
  -Two route tables were created, Public route table and Private route table.
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/route%20tables%20with%20assoc..PNG)
   
### 3. Set Up Security Groups;
   
  -Four security groups were created (bastionSG, appserverSG, webserverSG and dbSG)
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/security%20groups.PNG)
   
### 4. Launched EC2 Instances for the web server, app server and bastion host;
   
  -Bastion Host; launched in Public Subnet with public IP.
  -Web Server: launched in Public Subnet with public IP, with user data to install Apache.
  
  #!/bin/bash
  sudo yum update -y
  sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
  sudo yum install -y httpd
  sudo systemctl start httpd
  sudo systemctl enable httpd
  
  -App Server: launched in PrivateSubnet1, with user data to install mariaDB.
  
  #!/bin/bash
  sudo yum install -y mariadb-server
  sudo service mariadb start
  
   ![image alt](https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/EC2%20instances%20-web%2C%20app%20and%20bastion%20host.PNG)
   
### 5. Created RDS Database (MariaDB) with;
    
  -A DB subnet group created with Private subnet
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/DBinstance.PNG)
  -Assigned to the VPC, Database subnet group, and Database security group; public access was disabled.
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/dbsubnetgroup.PNG)
