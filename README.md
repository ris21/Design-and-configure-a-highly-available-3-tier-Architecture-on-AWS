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
   
  -A VPC (risvpc) was created with a CIDR block of 192.168.0.0/16 and 4 Subnets (Public and private) were configured in 2 AZs.
  ![image alt]( https://github.com/ris21/Design-and-configure-a-highly-available-3-tier-Architecture-on-AWS/blob/main/vpc.PNG)
   
### 2. Configured Routing and Gateways
   
  -An Elastic IP was allocated and an Internet Gateway (risIGW) was created and attached to (risvpc).
  -A NAT Gateway (risNGW) was set up in PublicSubnet1 using the Elastic IP.
  -Two route tables were created, Public route table and Private route table.
   
### 3. Set Up Security Groups;
   
  -Four security groups were created (bastionSG, appserverSG, webserverSG and dbSG)
   
### 4. Launched EC2 Instances for the web server, app server and bastion host);
   
  -Bastion Host; launched in Public Subnet with public IP.
  -Web Server: launched in Public Subnet with public IP, with user data to install Apache.
  -App Server: launched in PrivateSubnet1, with user data to install mariaDB.
   
### 5. Created RDS Database (MariaDB) with;
    
  -A DB subnet group created with Private subnet
  -Assigned to the vPC, Database subnet group, and Database security group; public access was disabled.
