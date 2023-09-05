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
