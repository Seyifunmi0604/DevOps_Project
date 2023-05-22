## Step 6 — Configure WordPress to connect to remote database.

Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32

![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b48acf6b-51a1-4d37-b088-6b218af568be)

1.	Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client
```
sudo yum install mysql
```
Modify DB credentials as below:
```
cd /var/www/html/
```
```
sudo vi wp-config.php
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5f179536-5b9e-4467-8321-52c391e45431)

Disable apache default page:
```
sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup
```
Ref [How to disable the default apache “Welcome Page” in CentOS/RHEL – The Geek Diary](https://www.thegeekdiary.com/how-to-disable-the-default-apache-welcome-page-in-centos-rhel-7/)

Let’s test our connection from Webserver to Database Server
```
sudo mysql -h <privateIP of Database Server> -u <Database User> -p
```
2.	Verify if you can successfully execute **SHOW DATABASES;** command and see a list of existing databases.

![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/836d0b0e-7421-4216-85e2-f9317575128d)

```
exit
```
## 3.	Change Permission and configuration so Apache could use WordPress:
```
sudo chown -R apache:apache /var/www/html/
```
```
ls -l wordpress/
```
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/33574ad9-79a9-4a40-8d09-f6efdb86ad13)

4.	Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)

5.	Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/

![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/eae951fa-b380-40ca-9d2f-ec6d08704eea)

![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/648ddcdf-b171-4121-983e-89e7f098cb8a)

![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/fda4f84c-3bbe-448b-81fe-5223086b43b5)




