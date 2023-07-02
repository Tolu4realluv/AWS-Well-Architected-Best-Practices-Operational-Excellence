# AWS-Well-Architected-Best-Practices-Operational-Excellence
---
Operational excellence helps teams to focus more of their time on building new features that benefit customers, and less time on maintenance and firefighting. To build correctly, we look to best practices that result in well-running systems, a balanced workload for you and your team, and most importantly, a great customer experience.

## Objectives of the Project
In this project, we create AWS Resource Group consisting of two EC2 instances and use the "RUN Command" capability of AWS Systems Manager to install Amazon CloudWatch agent for collecting logs and getting some additional metrics.

By running this project, we will be able to:

* Add custom tags to EC2 instances
* Create an AWS Resource Group for specific Amazon EC2 instances
* Use AWS Systems Manager for configuration of Amazon EC2 instances
* Install and start Amazon CloudWatch agent with AWS Systems Manager
* Validate customs metric and log groups for Amazon EC2 instances in Amazon CloudWatch

## Evaluating the existing Architecture.

![](https://github.com/Tolu4realluv/AWS-Well-Architected-Best-Practices-Operational-Excellence/blob/main/starting%20image.png)

The company is a retail business and one of the main application of the company is a product catalog a. The Company's  Architecture contains a single Amazon VPC, with an internet gateway and a 2 subnets. The subnets are labelled public subnet and private subnet. Each subnets contains a single EC2 instance, one labelled web app instance and one labelled database instance.
After running the AWS Well achitected framework review, it recommends the following:

* Automation : Most of the operational task are performed manually and the company wants to Automate some of the operational tasks and provide visibility into most performance metric like memory and disk usage etc. and additionally a centralized log monitoring for database  and Application is needed.
* Availability : A highly available architecture is required for the product catalog App
* Security : Security is a top proirity for the company
* Rightsizing Amazon EC2 instances: The company is not sure about their current EC2 instance size and are also expecting an increase in traffic in the nearest future.
* Cost : Some applications are not using the approved instance types in accordance with the company's architecture standards and these increases cost due to overprovision of resources.

As a solution Architect for the company taksed with the responsibility of having an Architecture with AWS best practices applied, We want the Architecture to meet new performance requirement, mitigate risks and save money and also take Automation into consideration.

## Proposed Architecture

![](https://github.com/Tolu4realluv/AWS-Well-Architected-Best-Practices-Operational-Excellence/blob/main/Proposed.png)

The Architecture is an Amazon VPC containing 2 avail;ability Zones. There are 6 total subnets in the environment, with each availabi;lity zone having 3 of the subnets. the subnets are labelled and divided by availability zone such as: 1 Public subnet, 1 Private subnet, and 1 db Private subnet. Each of the private subnets is part of an Autoscaling Group and contain 1 web server each. Each of the db private subnets contain 1 Amazon RDS instance each. Internet traffic flows into an internet gateway, to an Application load balancer residing in the public subnets, Then to the Autoscaling group in the private subnet and finally to the Amazon RDS primary instance in the db private subnet. Supporting management, automation & monitoring services for the environment are; Amazon CloudWatch, Amazon Cloudtrail, AWS CloudFormation,  AWS config & AWS system manager. Supporting security services for the evvironment are AWS Identity and Access Management and AWS Secrets Manager. 

___The Operational Excellence pillar includes the ability to support development and run workloads effectively, gain insight into their operations, and continuosly improve supporting processes and procedures to deliver business value.___ 

 Recall tha some of the discovery after running the AWS well-Architected Framework review was the need to automate some of the operational tasks, lack of visibility into important metrics such as memory and disk usage for the EC2 instances. The want an automated process to get that information. Additionally, the need a centralized log monitoring for the database and the application instances.

Now We Begin to Implement The new Architecture after successfully check the existing Architecture and adding new entries into the product catalog application.

![](https://github.com/Tolu4realluv/AWS-Well-Architected-Best-Practices-Operational-Excellence/blob/main/product.JPG)

## Task 1: Tag the Amazon EC2 Instances.

With AWS, customers can assign metadata to their AWS resources in the form of Tags. Each Tags is a simple label consisting of customer-defined key and optional value. Tags makes it easier to manage, search for, and filter resources. Tags are a great way to organise AWS resources, establish governance and enforce permissions. and they are critical to cost attribution for cost optimization. 

* On the AWS console, we search for EC2 and clicked on Instances,
* we select the wa-web-server instance only and chose Tags on the lower half of the screen.
* chose Manage Tags and add new tag
* Create Tag 1 for key we put __Environment__ and for Value-optional we put __Production__
* Create Tag 2 for key we put __App__ and for Value-optional we put __Product-Catalog__
* we chose save and return to the instances page
* We now select the wa-db-server instance only and repeat the previous three steps by creating Tags.
  
After succesfully creating the Tags, we get the following message:
![](https://github.com/Tolu4realluv/AWS-Well-Architected-Best-Practices-Operational-Excellence/blob/main/Tags.JPG)

## Task 1: Create A Resource Group.
A resouce group is a collection of AWS resources in the same region that match a tag-based criteria provided in a search query. Resource group can be used to perform bulk actions such as Applying updates or security patches, upgrading an application version, installing new softwares, etc.
* At the top of AWS management console, we searched for and selected __Resource Groups & Tag Editor__
* we clicked create a resource group, the brower displayed the __create query-based-group page__
* on the page, in __Group type__ section, we select __Tag Based__
* on the page, in the __Grouping criteria__ section, for Resources, we select __AWS::EC2::Instance__
* For Tags, For __Tag-Key__ we typed __App__ and for __Optional tag value__, we input __Product-Catalog__ and we chose save.
* For Tags, For __Tag-Key__ we typed __Environment__ and for __Optional tag value__, we input __Production__ and we chose save.
* Then we chose preview group names.
In the __Group resources__ section, both of our previously tagged Amazon EC2 are listed.
* on the __create query-based-group page__ in the __Group details__ section, for __Group name__ we input __rg-wa__ and for __Group Decription__ we input __Resource Group for Well-Architected Labs.__ and then we chose __create groups__.

The browser displays the rg-wa Group details page.
![](https://github.com/Tolu4realluv/AWS-Well-Architected-Best-Practices-Operational-Excellence/blob/main/Tags.JPG)








