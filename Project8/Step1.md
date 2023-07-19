## Configure Apache As A Load Balancer

1.	Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:

![8 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/2c34ebbb-45d5-45ac-b24d-0be46023ba17)

2.	Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in the Security Group.
   
3.	Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

```
#Install apache2
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev

#Enable following modules:
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic
#Restart apache2 service
sudo systemctl restart apache2

```
![8 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c23f3fa4-cceb-4707-939a-e743178115e2)

Make sure apache2 is up and running

```
sudo systemctl status apache2
```
![8 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/33afae68-4faa-41f1-aac9-344f3bc51955)

Configure load balancing

```
sudo vi /etc/apache2/sites-available/000-default.conf

#Add this configuration into this section <VirtualHost *:80>  </VirtualHost>

<Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
```
![8 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d863c1a6-74a9-4ea5-b7a4-d7362b929f09)

![8 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7bb349ad-2782-44b3-8b2d-157d6e91d916)

```
#Restart apache server

sudo systemctl restart apache2

sudo systemctl status apache2

```
![8 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c7b89425-6f66-40bd-a623-c0df50ad8ebc)

**bytraffic** balancing method will distribute incoming load between your Web Servers according to current traffic load. We can control in which proportion the traffic must be distributed by **loadfactor** parameter.
You can also study and try other methods, like: **bybusyness, byrequests, heartbeat**

4.	Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:
```
http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php
```
![8 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/06fb9bb1-1519-4446-a174-7689c3d277d8)

**Note:** If in the Project-7 you mounted /var/log/httpd/ from your Web Servers to the NFS server – unmount them and make sure that each Web Server has its own log directory.
sudo umount -f /var/log/httpd/ 

Open two ssh/Putty consoles for both Web Servers and run the following command:

```
sudo tail -f /var/log/httpd/access_log
```
![8 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ddade9a8-1d09-4d14-9bfa-d2a2e0002756)

Try to refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers receive HTTP GET requests from your LB – new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.

![8 10](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/80460c47-f965-4d77-a945-cdc3a2498022)

If you have configured everything correctly – your users will not even notice that their requests are served by more than one server.

# Side Self Study:
Read more about different configuration aspects of [Apache mod_proxy_balancer](https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html) module. Understand what sticky session means and when it is used.

## Optional Step – Configure Local DNS Names Resolution
Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management.
What we can do, is to configure local domain name resolution. The easiest way is to use **/etc/hosts** file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.

```
#Open this file on your LB server

sudo vi /etc/hosts

#Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers

<WebServer1-Private-IP-Address> Web1
<WebServer2-Private-IP-Address> Web2
```
![8 11](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/73c3d19c-2d23-415b-ba3b-7a59a7b2fdb9)

Now you can update your LB config file with those names instead of IP addresses.

```
sudo vi /etc/apache2/sites-available/000-default.conf
```
```
BalancerMember http://Web1:80 loadfactor=5 timeout=1
BalancerMember http://Web2:80 loadfactor=5 timeout=1
```
![8 12](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7b3e1506-0857-4d5a-84d3-767a5cadf8ab)

You can try to curl your Web Servers from LB locally **curl http://Web1 or curl http://Web2** – it shall work.
Remember, this is only internal configuration and it is also local to your LB server, these names will neither be ‘resolvable’ from other servers internally nor from the Internet.
You can try to curl your Web Servers from LB locally **curl http://Web1 or curl http://Web2** to see the HTML formated version of your website.

![8 13](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/1942528a-b938-4914-9515-ea847efc7898)

# Targed Architecture
Now your setup looks like this:

![8 14](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/67c08eb9-5cc8-46cd-b35a-5cbe6408a373)






