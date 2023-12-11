## Lab - Build Your DB Server and Interact With Your DB Using an App
Build Your DB Server and Interact With Your DB Using an App
This lab is designed to reinforce the concept of leveraging an AWS-managed database instance for solving relational database needs.

Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which allows you to focus on your applications and business. Amazon RDS provides you with six familiar database engines to choose from: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.

## Objectives

After completing this lab, you can:

Launch an Amazon RDS DB instance with high availability.
Configure the DB instance to permit connections from your web server.
Open a web application and interact with your database.

Duration

This lab takes approximately 45 minutes.

Scenario

You start with the following infrastructure:

architecture-lab1.png

At the end of the lab, this is the infrastructure:

architecture-lab2.png

Start lab
To launch the lab, at the top of the page, choose Start lab.
 You must wait for the provisioned AWS services to be ready before you can continue.

To open the lab, choose Open Console.
You are automatically signed in to the AWS Management Console in a new web browser tab.

 Do not change the Region unless instructed.

COMMON SIGN-IN ERRORS
Error: You must first sign out


If you see the message, You must first log out before logging into a different AWS account:

Choose the click here link.
Close your Amazon Web Services Sign In web browser tab and return to your initial lab page.
Choose Open Console again.
Error: Choosing Start Lab has no effect
In some cases, certain pop-up or script blocker web browser extensions might prevent the Start Lab button from working as intended. If you experience an issue starting the lab:

Add the lab domain name to your pop-up or script blocker’s allow list or turn it off.
Refresh the page and try again.
Task 1: Create a Security Group for the RDS DB Instance
In this task, you will create a security group to allow your web server to access your RDS DB instance. The security group will be used when you launch the database instance.

In the AWS Management Console, select the  Services menu, and then select VPC under Networking & Content Delivery.

In the left navigation pane, click Security Groups.

Click Create security group and then configure:

Security group name: 

DB Security Group
Description: 

Permit access from Web Security Group
VPC: Lab VPC
You will now add a rule to the security group to permit inbound database requests. The security group currently has no rules. You will add a rule to permit access from the Web Security Group.

In the Inbound rules section, click Add rule, then configure:
Type: MySQL/Aurora (3306)
CIDR, IP, Security Group or Prefix List: Type 

sg
 and then select Web Security Group.
This configures the Database security group to permit inbound traffic on port 3306 from any EC2 instance that is associated with the Web Security Group.

Scroll to the bottom of the screen, then click Create security group
You will use this security group when launching the Amazon RDS database.

Task 2: Create a DB Subnet Group
In this task, you will create a DB subnet group that is used to tell RDS which subnets can be used for the database. Each DB subnet group requires subnets in at least two Availability Zones.

In the AWS Management Console, select the  Services menu, and then select RDS under Database.

In the left navigation pane, click Subnet groups.

 If the navigation pane is not visible, click the  menu icon in the top-left corner.

Click Create DB Subnet Group then configure:
Name: 

DB Subnet Group
Description: 

DB Subnet Group
VPC ID: Lab VPC
In the Add subnets section for Availability zones, click the , then:
Select  the first Availability zone
Select  the second Availability zone
For Subnets, click the , then:
For the first Availability zone, select  10.0.1.0/24
For the second Availability zone, select  10.0.3.0/24
Click Create
This adds Private Subnet 1 (10.0.1.0/24) and Private Subnet 2 (10.0.3.0/24). You will use this DB subnet group when creating the database in the next task.

Task 3: Create an Amazon RDS DB Instance
In this task, you will configure and launch a Multi-AZ Amazon RDS for MySQL database instance.

Amazon RDS Multi-AZ deployments provide enhanced availability and durability for Database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB instance, Amazon RDS automatically creates a primary DB instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ).

In the left navigation pane, click Databases.

Click Create database

 If you see Switch to the new database creation flow at the top of the screen, please click it.

Select  MySQL.

Under Templates, select  Free tier.

Under Settings, configure:

DB instance identifier: 

lab-db
Master username: 

main
Master password: 

lab-password
Confirm password: 

lab-password
Under DB instance size, configure:
Select  Burstable classes (includes t classes).
Select db.t3.micro. Note: If the db.t3.micro instance is not available in the Availability Zone, choose db.t2.micro instead.
Under Storage, configure:
Select General Purpose (SSD) under Storage type.
Under Connectivity, configure:
Virtual Private Cloud (VPC): Lab VPC
Under VPC security group select Choose existing

Under Existing VPC security groups

Use X to Remove default
Select DB Security Group to highlight it in blue
Expand  Additional configuration, then configure:
Initial database name: 

lab
Uncheck  Enable automated backups.
Uncheck  Enable Enhanced monitoring.
 This will turn off backups, which is not normally recommended, but will make the database deploy faster for this lab.

Scroll to the bottom of the screen, then click Create database
Your database will now be launched.

Click lab-db (click the link itself).
You will now need to wait approximately 4 minutes for the database to be available. The deployment process is deploying a database in two different Availability zones.

 While you are waiting, you might want to review the Amazon RDS FAQs or grab a cup of coffee.

Wait until Info changes to Modifying or Available.

Scroll down to the Connectivity & Security section and copy the Endpoint field.

It will look similar to: lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com

Paste the Endpoint value into a text editor. You will use it later in the lab.
Task 4: Interact with Your Database
In this task, you will open a web application running on your web server and configure it to use the database.

In the left section of these instructions you are currently reading, copy the WebServer IP address.

Open a new web browser tab, paste the WebServer IP address and press Enter.

The web application will be displayed, showing information about the EC2 instance.

At the top of the web application page, click the RDS link.
A picture displaying the web application interface

Figure: A picture displaying the web application interface.

You will now configure the application to connect to your database.

Configure the following settings:
Endpoint: Paste the Endpoint you copied to a text editor earlier
Database: 

lab
Username: 

main
Password: 

lab-password
Click Submit
A message will appear explaining that the application is running a command to copy information to the database. After a few seconds the application will display an Address Book.

The Address Book application is using the RDS database to store information.

Test the web application by adding, editing and removing contacts.
The data is being persisted to the database and is automatically replicating to the second Availability Zone.

End lab

https://awsrestart.labs.awsevents.com/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FCUR-TF-100-RSDBAS-3%2F160-lab-DF-database-server%3A3.0.0-c154acee/en-US/46a424b2-440d-424c-b01f-97df9bc7474a::iF7X3pisW2U9TkKoMwPgmX