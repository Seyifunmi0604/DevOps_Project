# STEP1 INSTALLING APACHE AND UPDATING THE FIREWALL
What exactly is Apache?
Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. It runs on 67% of all webservers in the world. It is fast, reliable, and secure. 
Welcome! - The Apache HTTP Server Project.

It can be highly customized to meet the needs of many different environments by using extensions and modules. Most WordPress hosting providers use Apache as their web server software. However, websites and other applications can run on other web server software as well. Such as Nginx, Microsoft’s IIS, etc.

The Apache web server is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website.

Install Apache using Ubuntu’s package manager ‘apt’:

#update a list of packages in package manager

sudo apt update

#run apache2 package installation

sudo apt install apache2

To verify that apache2 is running as a Service in our OS, use following command:

sudo systemctl status apache2

![Picture1 1](https://user-images.githubusercontent.com/130314772/231008361-7d59c720-4173-466f-a802-9348be610c96.png)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:

![Picture1 2](https://user-images.githubusercontent.com/130314772/231008520-af7c615a-4586-4125-babe-62dd07bbc7e7.png)

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).
First, let us try to check how we can access it locally in our Ubuntu shell, run they both literally do the same thing:

 curl http://localhost:80
or
 curl http://127.0.0.1:80

![Picture1 3](https://user-images.githubusercontent.com/130314772/231008768-943ec14a-7e8f-4461-bc1d-40ec9bac3562.png)

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet.

Open a web browser of your choice and try to access following url

http://<Public-IP-Address>:80 : In my case http://107.21.86.229:80
  
Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:
  
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
or
curl ifconfig.me
  
![Picture1 4](https://user-images.githubusercontent.com/130314772/231009376-3f34952f-7829-4832-8138-417ab34bf4c0.png)
  
http://107.21.86.229:80
  
![Picture1 5](https://user-images.githubusercontent.com/130314772/231009661-fa847016-8ecd-4699-8506-e18b7e27f2e3.png)

In fact, it is the same content that you previously got by ‘curl’ command, but represented in nice HTML formatting by your web browser.

