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

sudo echo "<h1> ELUS MEDICAL CONSULT</h1>" >> index.html

Click "Launch Instance"

Click "View Instances"

[launch instance complete.pdf](https://github.com/ForkahEH/AWS-3-Tier-Architecture/files/12526982/launch.instance.complete.pdf)

4. Refresh until status check for the instance shows "2/2 checks passed".
<img width="721" alt="Screenshot 2023-09-05 153419" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/af62c57a-f2ca-469c-b5c2-d42af21cf4ba">

5. Select the running Web server instance.

6. Copy the public ip of theinstance and paste in a new web page.

A web page with the details specified in the user data shows on the screen.
<img width="879" alt="Screenshot 2023-09-05 154208" src="https://github.com/ForkahEH/AWS-3-Tier-Architecture/assets/127892742/e4640e6b-a5a8-4bc4-91b0-5bf8e11db7bb">

Navigate back to the EC2 console.

7. Select the running instance of the web server, Click on the actions tab, select "Images and template" from the drop down menu, select "create template from instance" from the drop down menu.

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



