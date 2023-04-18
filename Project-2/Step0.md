# PROJECT 2

## WEB STACK IMPLEMENTATION (LEMP STACK)

Below are the steps involved:

Web stack implementation (lemp stack)

•	Step 1 – installing the nginx web server

•	Step 2 — installing mysql

•	Step 3 – installing php

•	Step 4 — configuring nginx to use php processor

•	Step 5 – testing php with nginx

•	Step 6 – retrieving data from mysql database with php (continued)

Project 2 covers similar concepts as Project 1 and helps to cement your skills of deploying Web solutions using LA(E)MP stacks.

You have done a great job with successful completion of Project 1.

In this project you will implement a similar stack, but with an alternative Web Server – [NGINX](https://nginx.org/en/), which is also very popular and widely used by many websites in the Internet.

Side Self Study

1.	Make yourself familiar with basic [SQL syntax and most commonly used commands](https://www.w3schools.com/sql/sql_syntax.asp)

2.	Be comfortable using not only VIM, but also [Nano editor](https://www.nano-editor.org/) as well, get to know [basic Nano commands](https://www.linuxandubuntu.com/home/nano-cli-text-editor-for-everyone-basic-tutorials)

Step 0 – Preparing Prerequisites

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account and create a new EC2 Instance of t2.micro family with Ubuntu Server

22.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you STOP the ones you are not working with at the moment to save available free hours.

Download and install Git Bash like it is shown in this [video](https://www.youtube.com/watch?v=qdwWe9COT9k&feature=youtu.be)

EC2 Instance launched let’s connect via gitbash:

ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>

![Pic2 0](https://user-images.githubusercontent.com/130314772/232650766-d2b4d683-e33e-4e48-8865-20aeab4080b0.png)
  
We are connected. To make our instance ID persist whenever we reboot, run: **sudo hostnamectl set-hostname Project2**
