Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which allows you to focus on your applications and business. Amazon RDS provides you with six familiar database engines to choose from: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.

Objectives


Launch an Amazon RDS DB instance with high availability.

Configure the DB instance to permit connections from your web server.

Open a web application and interact with your database.

Task 1: Create a Security Group for the RDS DB Instance
In this task, you will create a security group to allow your web server to access your RDS DB instance. The security group will be used when you launch the database instance.

In the AWS Management Console, select the  Services menu, and then select VPC under Networking & Content Delivery.

In the left navigation pane, click Security Groups.

Click Create security group and then configure:

Security group name: DB Security Group

Description: Permit access from Web Security Group

VPC: Lab VPC

You will now add a rule to the security group to permit inbound database requests. The security group currently has no rules. You will add a rule to permit access from the Web Security Group.

In the Inbound rules section, click Add rule, then configure:

Type: MySQL/Aurora (3306)

CIDR, IP, Security Group or Prefix List: Type sg and then select Web Security Group.

This configures the Database security group to permit inbound traffic on port 3306 from any EC2 instance that is associated with the Web Security Group.

Scroll to the bottom of the screen, then click Create security group

You will use this security group when launching the Amazon RDS database.

 

Task 2: Create a DB Subnet Group
In this task, you will create a DB subnet group that is used to tell RDS which subnets can be used for the database. Each DB subnet group requires subnets in at least two Availability Zones.

In the AWS Management Console, select the  Services menu, and then select RDS under Database.

In the left navigation pane, click Subnet groups.

 If the navigation pane is not visible, click the  menu icon in the top-left corner.

Click Create DB Subnet Group then configure:

Name: DB Subnet Group

Description: DB Subnet Group

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

Choose Create database

Under Engine options, select  MySQL.

The options include several use cases, ranging from enterprise-class databases to Dev/Test systems. In the options, you might notice Amazon Aurora. Aurora is a MySQL-compatible system that was re-architected for the cloud. If your company uses large-scale MySQL or PostgreSQL databases, Aurora can provide enhanced performance.

In the Templates section, select  Dev/Test.

You can now select a database configuration, including software version, instance class, storage, and login settings. The Multi-AZ deployment option automatically creates a replica of the database in a second Availability Zone for high availability. 

In Availability and durability section, choose  Multi-AZ DB instance.

In the Settings section, configure the following options:

DB instance identifier: lab-db

Master username: admin

Master password: lab-password

Confirm password: lab-password

In the DB instance class section, configure the following options:

Select  Burstable classes (includes t classes).

Select db.t3.micro.

In the Storage section, configure the following options:

Storage type: Select General Purpose SSD (gp2)

Allocated storage: Enter 20 GiB

In the Connectivity section, configure the following option: 

Virtual private cloud (VPC): Lab VPC

In the VPC security group (firewall) section, for Existing VPC security groups, choose the X on default to remove this security group. Then choose the dropdown list, and select DB Security Group to add it.

In the Monitoring section, clear (deselect) the Enable Enhanced monitoring option.

In the Additional configuration section, choose  to expand it. 

For Initial database name, enter lab

At the bottom of the screen, choose Create database.

Your database will now be launched.

Choose the link displayed the top lab-db .

You will now need to wait approximately 8 minutes for the Multi-AZ database to be available. The deployment process is deploying a database in two different Availability zones.

Wait until Status changes to Modifying or Available.

Scroll down to the Connectivity & Security section and copy the Endpoint field.

It will look similar to: lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com

Paste the Endpoint value into a text editor. You will use it later in the lab.

 

Task 4: Interact with Your Database
In this task, you will open a web application running on your web server and configure it to use the database.

Copy the WebServer IP address by selecting i AWS Details above these instructions you are currently reading.

Open a new web browser tab, paste the WebServer IP address and press Enter.

The web application will be displayed, showing information about the EC2 instance.

At the top of the web application page, click the RDS link.

A picture displaying the web application interface

Figure: A picture displaying the web application interface.

 

You will now configure the application to connect to your database.

Configure the following settings:

Endpoint: Paste the Endpoint you copied to a text editor earlier

Database: lab-db

Username: admin

Password: lab-password

Click Submit

A message will appear explaining that the application is running a command to copy information to the database. After a few seconds the application will display an Address Book.

The Address Book application is using the RDS database to store information.

Test the web application by adding, editing and removing contacts.

The data is being persisted to the database and is automatically replicating to the second Availability Zone.
