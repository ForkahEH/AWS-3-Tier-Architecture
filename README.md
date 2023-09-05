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

1. In the AWS Console Home, search for and select "EC2" resource.

2. In the EC2 Console, select "Launch Instances"

<img width="913" alt="Screenshot 2023-09-05 150924" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/4c74f007-b520-48e8-a5f4-d76f22b88949">

3. In the "Launch Instance" page, enter the name of the webserver: Web server

Key pair: 3 tier

Select Edit in Network Settings

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

sudo echo /"<h1> ELUS MEDICAL CONSULT</h1>"/ >> index.html
Click "Launch Instance"

Click "View Instances"

![Picture1](https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/357f95ba-9459-4839-b9d9-94b5a324f7e3)
<img width="647" alt="Screenshot 2023-09-05 183324" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/9b349d55-fb82-4d76-95a5-c31000aaf7cc">
<img width="614" alt="Screenshot 2023-09-05 183402" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/59131c04-8853-455d-9dd7-37856a8c8ad8">
<img width="619" alt="Screenshot 2023-09-05 183435" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/6562de58-1d07-4f56-ab1e-6740e994817a">
<img width="610" alt="Screenshot 2023-09-05 183455" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/c8fd32fd-cad0-4f7a-bbb7-b1c02f27d131">
<img width="608" alt="Screenshot 2023-09-05 183529" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/c8b0e5de-bdd6-451a-8346-562b6746e650">
<img width="619" alt="Screenshot 2023-09-05 183612" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/eb4d9279-028d-4d1c-be48-d044585abbbb">


4. Refresh until status check for the instance shows "2/2 checks passed".
<img width="721" alt="Screenshot 2023-09-05 153419" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/af62c57a-f2ca-469c-b5c2-d42af21cf4ba">

5. Select the running Web server instance.

6. Copy the public ip of theinstance and paste in a new web page.

A web page with the details specified in the user data shows on the screen.
<img width="879" alt="Screenshot 2023-09-05 154208" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/e4640e6b-a5a8-4bc4-91b0-5bf8e11db7bb">

Navigate back to the EC2 console.

7. Create a Launch Template and Auto Scaling Group.
Select the running instance of the web server, Click on the actions tab, select "Images and template" from the drop down menu, select "create template from instance" from the drop down menu.

<img width="724" alt="Screenshot 2023-09-05 154815" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/32300c19-cdfc-40a3-833c-80380d1f1b53">

8. In the "Create launch template" window, enter the name of the launch template: Web server template. Select "Auto Scaling guidance" since EC2 autoscaling will be used.

<img width="758" alt="Screenshot 2023-09-05 161034" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/46b98a43-2ac9-4cd1-9a06-9882ab44f8a2">


In the Network settings section, select "Don't include in launch template" for the subnet section.

Select the existing web server security group.

<img width="656" alt="Screenshot 2023-09-05 160805" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/8ba67708-d52d-41f4-b991-6aa6c075556d">

Confirm the rest of the settings and click "Create launch template".

9. Navigate back to the EC2 Console. Select "Auto Scaling group".

10. Click "Create Auto Scaling group"

11. In the Create Auto Scaling group page, enter the name of the Auto Scalig group: WebServerASG

<img width="857" alt="Screenshot 2023-09-05 161640" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/c4630e49-97e8-4459-973d-3d22cab7f784">

Select  "WebServerTemplate" in the "Launch Template" section. Leave the defaults and click Next.

<img width="593" alt="Screenshot 2023-09-05 161948" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/5ed4e59b-3698-499f-9cbd-994be62b7dd9">

Select "3 tier project-vpc" and the web tier subnets, click Next.

<img width="823" alt="Screenshot 2023-09-05 162227" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/7b7e5a80-bc77-4a16-b42a-3d95117633c5">

Select "attach to a new load balancer". For the Load Balancer type, select "Application Load Balancer" and select "Internet-facing"

<img width="851" alt="Screenshot 2023-09-05 162851" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/104fd1d7-6052-4845-a22f-d16256842bbb">

<img width="612" alt="Screenshot 2023-09-05 163027" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/8cb15ed5-d50c-4092-a0c4-b6e2cadfc7ca">

Under "Listeners and routing", create a new target group "WebServerASG-ALB"

<img width="595" alt="Screenshot 2023-09-05 163316" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/a51b6b48-3fb3-46d5-9a74-ec8028caa064">

Under "Health Checks", enable Elastic Load Balancing health checks.

<img width="611" alt="Screenshot 2023-09-05 163701" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/2acd1730-d73c-40e2-9cb8-3be6c6a5d380">

Under Additional settings, select Enable group metrics collection within CloudWatch and Enable default instance warmup. Click Next.

<img width="637" alt="Screenshot 2023-09-05 163858" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d1ec5830-c394-4800-a602-ce457e2113f9">

Under Group size, set minimum capacity to 1, desired capacity to to and maximum capacity to 3.

<img width="631" alt="Screenshot 2023-09-05 164120" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/0e001893-7eaa-4b19-9e3f-d2511737cf79">

Under Scaling policies, select "Target tracking scaling policy". Click Next.

<img width="697" alt="Screenshot 2023-09-05 164438" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d61aea2f-4dd3-4044-b759-9666230f0b47">

Notifications can be set to send SNS topics whenever Amazon EC2 Auto Scaling launches or terminates the EC2 instances in your Auto Scaling group. Its optional.

Click Next.

Review and Click "Create Auto Scaling Group".

12. Confirm the route table for the public subnets are internet-facing and attached to the web subnets. To do this:

Navigate to the VPC Console.

Select the "3 tier project-vpc" and select "Route tables" in the VPC dashboard.

In the Route tables page, select the public route table and check that it is associated with the web subnets( the public subnets).

<img width="902" alt="Screenshot 2023-09-05 182555" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/3d8540c2-b44e-417f-912c-ac41c0449b44">


Step 3: Create the application tier

1. In the AWS Console Home, search for and select "EC2" resource.

2. In the EC2 Console, select "Launch Instances"

3. In the "Launch Instance" page, enter the name of the application server: App server

Key pair: 3 tier

Select Edit in Network Settings
For the application tier, access is limited for security purposes. SSH, HTTP, and ICMP traffic is only allowed from our web tier security group. The ICMP rule will allow us to ping the application tier from the web tier.

Select "3 tier project-vpc" and one of the web subnets.

<img width="658" alt="Screenshot 2023-09-05 184739" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/cf3103ed-2d8f-4f36-b555-7423e4cb6e05">

Select create security group

Security group name: AppSG
Select “Custom” source type, and source "WebSG".
<img width="603" alt="Screenshot 2023-09-05 184812" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/114b9970-d277-4cbb-9af6-af0ffd1ed227">
<img width="598" alt="Screenshot 2023-09-05 184906" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/b2a24579-fcd6-4187-a513-8e3a3153c3c2">

Click "Launch Instance"

Click "View  all Instances"

4. Create a Launch Template

Select the running instance of the app server, Click on the actions tab, select "Images and template" from the drop down menu, select "create template from instance" from the drop down menu.

<img width="734" alt="Screenshot 2023-09-05 185800" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/00da8fac-0bad-406c-b6bf-61d9227364aa">

In the "Create launch template" window, enter the name of the launch template: Web server template. Select "Auto Scaling guidance" since EC2 autoscaling will be used.

<img width="642" alt="Screenshot 2023-09-05 190158" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/1750cae8-b3c4-4765-ac86-6d564d4d2082">

In the Network settings section, select "Don't include in launch template" for the subnet section.

Select the existing app server security group.

<img width="621" alt="Screenshot 2023-09-05 190316" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/a4194496-3a95-4c4e-a9f7-0cb31e5cb4a8">

Scroll to the bottom of the page and click "Create launch template"

<img width="612" alt="Screenshot 2023-09-05 190421" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/e8ef6577-12cb-4270-97e7-9954bd0aa5a4">

5. Create Auto Scaling Group
Navigate back to the EC2 Console. Select "Auto Scaling group".

Click "Create Auto Scaling group"

In the Create Auto Scaling group page, enter the name of the Auto Scalig group: AppServerASG

Select  "AppServerTemplate" in the "Launch Template" section. Leave the defaults and click Next.

<img width="618" alt="Screenshot 2023-09-05 190657" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/50bb26f7-6902-4de0-8357-6efefb2f170f">

Select "3 tier project-vpc" and the app tier subnets, click Next.

<img width="580" alt="Screenshot 2023-09-05 190758" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/57d932bd-456c-4a2e-be0e-5e8ded73ea6c">

Select "attach to a new load balancer". For the Load Balancer type, select "Application Load Balancer" and select "Internal"

<img width="578" alt="Screenshot 2023-09-05 191322" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/09e96f7b-c30f-4977-8d20-2ee99ae955da">

Under "Listeners and routing", create a new target group "APPServerASG-ALB"

<img width="583" alt="Screenshot 2023-09-05 191500" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/55532049-4fce-47a5-b356-1c6e24223259">

Under "Health Checks", enable Elastic Load Balancing health checks.

<img width="611" alt="Screenshot 2023-09-05 163701" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/2acd1730-d73c-40e2-9cb8-3be6c6a5d380">

Under Additional settings, select Enable group metrics collection within CloudWatch and Enable default instance warmup. Click Next.

<img width="637" alt="Screenshot 2023-09-05 163858" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d1ec5830-c394-4800-a602-ce457e2113f9">

Under Group size, set minimum capacity to 1, desired capacity to to and maximum capacity to 3.

<img width="631" alt="Screenshot 2023-09-05 164120" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/0e001893-7eaa-4b19-9e3f-d2511737cf79">

Under Scaling policies, select "Target tracking scaling policy". Click Next.

<img width="697" alt="Screenshot 2023-09-05 164438" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d61aea2f-4dd3-4044-b759-9666230f0b47">

Notifications can be set to send SNS topics whenever Amazon EC2 Auto Scaling launches or terminates the EC2 instances in your Auto Scaling group. Its optional.

Click Next.

Review and Click "Create Auto Scaling Group".

<img width="612" alt="Screenshot 2023-09-05 191640" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/02d2f7f7-631d-411d-9b6e-23d40bc7d334">


6. Confirm the route tables for the private subnets are not internet-facing and attached to the app subnets. To do this:

Navigate to the VPC Console.

Select the "3 tier project-vpc" and select "Route tables" in the VPC dashboard.

In the Route tables page, select private route tables 1 and 2 and check that they are associated with the app subnets. Also check the routes and confirm the targets as the nat gateway and local.
<img width="712" alt="Screenshot 2023-09-05 194442" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/287dac57-8054-410e-aebf-fa0b492fd412">
<img width="705" alt="Screenshot 2023-09-05 194117" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/b45b5177-b773-44c8-be18-d4d56093b8f3">
<img width="716" alt="Screenshot 2023-09-05 194047" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d7bab139-8c5a-46da-ad47-ff366aada414">

Step 4: Create the database tier

1. In the AWS Console Home, search for and select "RDS" resource.

2. In the RDS Console, select "Subnet groups" and click "Create DB subnet group".

<img width="913" alt="Screenshot 2023-09-05 195610" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/848cc8a2-875f-4aa7-86de-39eecc9496ae">

3. In the Create DB subnet group page, enter the name of the subnet group: DatabaseSubnetGroup.

<img width="620" alt="Screenshot 2023-09-05 200912" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/d5d349f7-a62e-4843-ad39-f141616b81ca">

4. Select the 3 tier project-vpc, the Availability Zones( us-east-1a, us-east-1b), and the data subnets. Click "Create".

<img width="583" alt="Screenshot 2023-09-05 200512" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/bf941d39-8d5a-4530-9316-a4ecf557a2cb">

5. Click on “Databases” on the RDS console, and then “Create database”. Select “Standard create” and “MySQL”.

<img width="904" alt="Screenshot 2023-09-05 201056" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/095f85ed-d9e8-4697-b4c1-d8955355e4a7">

<img width="603" alt="Screenshot 2023-09-05 201202" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/e0fce439-4e38-4fcc-9ac0-9f9ebb1ae2ed">

6. Select the free tier template.

<img width="654" alt="Screenshot 2023-09-05 201345" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/23d3a176-586f-4bbf-8106-340f86c67835">

7. Under Settings, enter the name of the datebase: threetiervpcdatabse

<img width="584" alt="Screenshot 2023-09-05 201729" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/5e2d077f-bf6f-4be5-9671-c577530f8f74">

8. Under Credential settings, enter the master username and master password.

<img width="609" alt="Screenshot 2023-09-05 201840" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/01a42574-e37e-49a6-b997-f8f948edeb86">

9. Under Connectivity, select "Don’t connect to an EC2 compute resource" and select the "3 tier project-vpc"

<img width="587" alt="Screenshot 2023-09-05 202114" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/afdd169c-7b57-4371-b20a-432c4c0fba99">

10. Under VPC security group, select "Create new". Enter the New VPC security group name: dataSG and select the us-east-1a availability zone.

<img width="614" alt="Screenshot 2023-09-05 202701" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/41c7f7d8-a1f8-4733-9710-8ffa3324c8fd">

11. Click "Create database"

12. Update the Data tier Security Group by navigating to the VPC console. Select Security groups and click on "dataSG". By default, the database security group has an inbound rule to allow MySQL/Aurora traffic on port 3306 the host IP address. For security reasons, only inbound traffic for MySQL from the application tier security group will be allowed.

13. Click "Edit inbound rules". Delete the default rule and click "Add rule". Select “MySQL/Aurora” for Type, choose custom Source, and then select the application tier security group and “Save rules.”

<img width="877" alt="Screenshot 2023-09-05 203842" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/bcf45bef-71a3-4bc7-a6bf-21a3fd8ad671">

14. Confirm the route tables for the private subnets are not internet-facing and attached to the app subnets. To do this:

Navigate to the VPC Console.

Select the "3 tier project-vpc" and select "Route tables" in the VPC dashboard.

In the Route tables page, select private route table 3 and check that it associated with the data subnets. Also check the routes and confirm the targets as the nat gateway and local.

<img width="700" alt="Screenshot 2023-09-05 204321" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/ca26deeb-1c17-4523-a399-9a7e18ee0c7b">

Step 5: Test the Connectivity of the app tier.

1. Connect to web server instance using SSH.
<img width="565" alt="Screenshot 2023-09-05 205312" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/cbbd80eb-a0bc-41e6-b788-82ea6261798a">

2. Ping the app tier from the web tier using the app tier private IPv4 address.
<img width="388" alt="Screenshot 2023-09-05 205557" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/0ebd7ae6-2497-46f6-829c-8d3d5285b0de">

