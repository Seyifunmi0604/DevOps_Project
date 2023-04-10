# PROJECT 1: LAMP STACK IMPLEMENTATION
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
Step 0 – Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
AWS is the biggest Cloud Service Provider and it offers a free tier account that we are going to leverage for our projects.
Do not focus too much on AWS itself right now, there will be a proper Cloud introduction and configuration projects later in our course.
Right now, all we need to know is that AWS can provide us with a free virtual server called EC2 (Elastic Compute Cloud) for our needs.
Spinning up a new EC2 instance (an instance of a virtual server) is only a matter of a few clicks.

•	So let’s Lunch an EC2 ubuntu instance and called it PROJECT1

![Picture1](https://user-images.githubusercontent.com/130314772/231003882-d6be59b6-ab81-4904-9439-3c996b25bd78.png)

Connect to mobaXterm using ssh: ssh -I <private-key-name.pem> ubuntu@<public-IP-Address>
ssh -i "Class29key.pem" ubuntu@ec2-107-21-86-229.compute-1.amazonaws.com


![Picture2](https://user-images.githubusercontent.com/130314772/231004048-f674cdbb-1929-42d4-8123-cc9366c366fb.png)
  
![Picture3](https://user-images.githubusercontent.com/130314772/231004191-0f7f5d8f-1097-4f53-85db-c87c9fb8cb42.png)
  
We are connected. To make our instance ID persist whenever we reboot, run: sudo hostnamectl set-hostname Project1
  
![Picture4](https://user-images.githubusercontent.com/130314772/231004515-2f8cf2a2-a7b5-4d80-add4-68e09c71f1b9.png)
  
Yaaay! Linux Server created in the Cloud and our set up looks like this now:

![Picture5](https://user-images.githubusercontent.com/130314772/231004786-67a00fb7-3701-4189-bf24-05d75b20a03c.png)
