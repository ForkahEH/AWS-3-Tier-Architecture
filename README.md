AWS 3 tier application architecture.

Overview

A three-tier application separates an application into three logical and physical computing tiers: a web tier, an application tier and a data tier.  
Users interact with the presentation/web tier(usually called the front-end), the business logic is processed in the application tier and the data is stored  and managed in the data tier(usually called the back-end).
A three tier architecture has a number of advantages including decreased app deployment time, increased scalability and  security since each tier has specific functions. 
In this project, a three tier application is created using AWS resources.

Step 1: Create the VPC 

1. In the AWS Console Home, search for and select "VPC" resource.

2. In the VPC Console click create VPC
<img width="902" alt="Screenshot 2023-09-05 135539" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/5e8f3e22-1ed8-4e6e-aabd-c3d374c26793">

3. Select "VPC and more"

4. I use tag auto-generation, which will tag all of the resources in this VPC for organization and easier access.

Name tag auto-generation: 3 tier project

IPv4 CIDR block: 10.0.0.0/16

Tenancy: Default

Availability zones: 2

Number of public subnets: 2

Number of private subnets: 4

NAT gateways: In 1 AZ 

VPC endpoints: None

Enable DNS hostnames

Enable DNS resolutions

5. Click “Create VPC”
![Picture2](https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/ae0f226c-d0f3-4c6c-a253-74e9d58ff4b6)

6. Click “View VPC” after the VPC and all its resources are created.
7. Click "Subnets" from the VPC dashboard.
8. In the Subnets console, the tags of the various subnets are edited to reflect the various tiers. This allows easy identification of subnets.
<img width="701" alt="Screenshot 2023-09-05 145201" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/090210c1-fb50-4e6f-953b-5857b7fbc88e">

<img width="684" alt="Screenshot 2023-09-05 145232" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/16f0fa58-952a-48b0-b097-e4d1db2ff296">

<img width="704" alt="Screenshot 2023-09-05 145252" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/c535257e-f2d7-492e-9ac5-930c5a8cee19">

9. Enable auto-assign public IPv4 address for the public subnets.
<img width="719" alt="Screenshot 2023-09-05 150322" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/48bfeef1-90fe-498f-a848-3d768d1e4556">

<img width="823" alt="Screenshot 2023-09-05 150222" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/923cd235-f781-42b6-a2f5-36e169e4d167">

Step 2: Create the web tier

In the AWS Console Home, search for and select "EC2" resource.

In the EC2 Console, select "Launch Instances"
<img width="913" alt="Screenshot 2023-09-05 150924" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/4c74f007-b520-48e8-a5f4-d76f22b88949">
Name: Web server
Key pair: 3 tier
Edit Network Settings
Select "3 tier project-vpc" and one of the web subnets.
Select Enable "auto assign public ip"
Select create security group
Security group name: WebSG
Rules: SSH, port 22 and HTTP, port 80
HTTP source: Anywhere

User Data
#!/bin/bash
yum update -y 
yum install -y httpd
systemctl start httpd
systemctl enable httpd
cd var/www/html
sudo echo "<h1> ELUS MEDICAL CONSULT</h1>" >> index.html

Click "Launch Instance"

Click "View Instances"

Refresh until status check for the instance shows "2/2 checks passed".
<img width="721" alt="Screenshot 2023-09-05 153419" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/af62c57a-f2ca-469c-b5c2-d42af21cf4ba">

Select the running Web server instance.

Copy the public ip of theinstance and paste in a new web page.

A web page with the details specified in the user data shows on the screen.

<img width="932" alt="Screenshot 2023-09-05 153617" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/70de4cd2-7a4e-4a12-bf32-12d7e6c40fb2">
