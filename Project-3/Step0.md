## PROJECT3 MERN STACK IMPLEMENTATION

# SIMPLE TO-DO APPLICATION ON MERN WEB STACK

In this project, we are tasked to implement a web solution based on MERN stack in AWS Cloud.

The below are the steps for MERN stack implementation; Simple to-do application on mern web stack

•	Step 1 – backend configuration

•	Install expressjs

•	Models

•	Mongodb database

•	Step 2 – frontend creation

•	Frontend creation (continued)

# MERN Web stack consists of following components:

1.	[MongoDB](https://www.mongodb.com/): A document-based, No-SQL database used to store application data in a form of documents.

2.	[ExpressJS](https://expressjs.com/): A server side Web Application framework for Node.js.
	
3.	[ReactJS](https://react.dev/): A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.

4.	[Node.js](https://nodejs.org/en): A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

![Picture3 1](https://user-images.githubusercontent.com/130314772/232909324-44cbb7a3-eb3d-4e49-8ad1-382960935ed8.png)

Side Self Study

1.	Make a research what types of Database Management Systems (DBMS) exist and what each type is more suitable for. Be able to explain the difference between Relational DBMS and NoSQL (of a different kind).

2.	Get yourself familiar with a concept of [Web Application Frameworks](https://en.wikipedia.org/wiki/Web_framework). Get to know what server-side (backend) and client-side (forntend) frameworks exist and what they are used for.

3.	[Practice basic JavaScript syntax just for fun](https://www.w3schools.com/js/js_intro.asp).
	
4.	Explore what [RESTful API](https://restfulapi.net/) is and what it is used for in Web development.
	
5.	Read what [Cascading Style Sheets (CSS)](https://en.wikipedia.org/wiki/CSS) is used for and browse basic [syntax and properties](https://www.w3schools.com/css/css_intro.asp).

# Step 0 – Preparing prerequisites

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account and create a new EC2 Instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP/TERMINATE** the ones you are not working with at the moment to save available free hours.

Hint #1: When you create your EC2 Instances, you can add Tag “Name” to it with a value that corresponds to a current project you are working on – it will be reflected in the name of the EC2 Instance. 

![Picture3 2](https://user-images.githubusercontent.com/130314772/232910851-5da5659e-0bb3-4f3c-b60e-01dc5083b054.png)

Hint #2 (for Windows users only): In previous projects we used Putty and Git Bash to connect to our EC2 Instances.

In this project and going forward, we are going to explore the usage [windows terminal](https://learn.microsoft.com/en-us/windows/terminal/install).

Watch this videos to learn how to set up windows terminal on your pc;

•	[windows installatatiosn part 1](https://www.youtube.com/watch?v=R-qcpehB5HY)

•	[windows installatatiosn part 2](https://www.youtube.com/watch?v=jsNIlK5s6pI)
