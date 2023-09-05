How to create an AWS 3 tier application architecture.

Overview

A three-tier application separates an application into three logical and physical computing tiers: a web tier, an application tier and a data tier.  
Users interact with the presentation/web tier(usually called the front-end), the business logic is processed in the application tier and the data is stored  and managed in the data tier(usually called the back-end).
A three tier architecture has a number of advantages including decreased app deployment time, increased scalability and  security since each tier has specific functions. 
In this project, a three tier application is created using AWS resources.

Step 1: Create the VPC 
In the AWS Console Home, search for and select "VPC" resource.
In the VPC Console click create VPC
<img width="902" alt="Screenshot 2023-09-05 135539" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/5e8f3e22-1ed8-4e6e-aabd-c3d374c26793">
IPv4 CIDR block: 10.0.0.0/16
Tenancy: Default
Availability zones: 2
Number of public subnets: 2
Number of private subnets: 4
NAT gateways: In 1 AZ (there is a fee for this, so delete it when you are done)
VPC endpoints: None
Enable DNS hostnames: check
Enable DNS resolutions: check
