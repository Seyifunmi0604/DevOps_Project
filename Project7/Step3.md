# Step 3 — Prepare the Web Servers

We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and MySQL database.
You already know that one DB can be accessed for **reads and writes** by multiple clients. For storing shared files that our Web Servers will use – we will utilize NFS and mount previously created Logical Volume **lv-apps** to the folder where Apache stores files to be served to the users **(/var/www).**

This approach will make our Web Servers **stateless**, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.
During the next steps we will do following:

•	Configure NFS client (this step must be done on all three servers)
•	Deploy a Tooling application to our Web Servers into a shared NFS folder
•	Configure the Web Servers to work with a single MySQL database

1.	Launch a new EC2 instance with RHEL 8 Operating System
2.	Install NFS client
```
sudo yum install nfs-utils nfs4-acl-tools -y
```
**3.	Mount /var/www/ and target the NFS server’s export for apps**

```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```

**4.	Verify that NFS was mounted successfully by running df -h. **

![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/804f5cef-dd36-4b9b-9519-55bfbee2f30a)

NOTE: what ever we create in /var/www on Webserver1, will be found in **/mnt/apps** on NFS server. For example let’s **sudo touch /var/www/text.md** from webserver1 and navigate to **ls /mnt/apps** in NFS server.

```
sudo touch /var/www/text.md
```

![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f4d17cc0-2bb9-47b9-a5ac-1b7578091b92)

```
ls /mnt/apps
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/aec30bd9-d834-4c8b-89a7-b9abea0546f1)

**5.	Make sure that the changes will persist on Web Server after reboot:**
```
sudo vi /etc/fstab
```

add following line

```
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
```

**6.	Install Remi’s repository, Apache and PHP**

```
sudo yum install httpd -y

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module reset php

sudo dnf module enable php:remi-7.4

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

sudo setsebool -P httpd_execmem 1
```
# Repeat steps 1-6 for another 2 Web Servers.

7.	Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files – it means NFS is mounted correctly. You can try to create a new file touch test.txt from one server and check if the same file is accessible from other Web Servers.

![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f7d36c9a-e638-4300-89aa-8dd0ea8c20fa)

![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/bb6f0576-a339-47ae-9e34-e00ee3ba0277)

![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/224a5f90-706f-4ed6-a3bd-ce1f11b96471)

8.	Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №5 to make sure the mount point will persist after reboot.

![Picture33](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0b0a54de-bb74-498f-9fad-9c756fbe8349)

```
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd
```
# Make sure that the changes will persist on Web Server after reboot:

```
sudo vi /etc/fstab
# add the below
<NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd nfs defaults 0 0
```
9.	Fork the tooling source code from [Darey.io Github Account to your Github account](https://github.com/darey-io/tooling). Learn how to fork a repo [here](https://www.youtube.com/watch?v=f5grYMXbAV0)

10.	Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to **/var/www/html**

11.	Install git:
  
```
sudo yum install git -y
git init
```
```
git clone https://github.com/Seyifunmi0604/tooling.git
```
  
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/395f89ec-a9a3-49c8-aa8d-1e06a6d0a0ee)

To ensure . Ensure that the html folder from the repository is deployed to **/var/www/html
```
cd tooling
sudo cp -R html/. /var/www/html
```

![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5a20d642-eda4-409a-af76-dfa13de589fe)

** Note 1: Do not forget to open TCP port 80 on the Web Server.**

**Note 2: If you encounter 403 Error – check permissions to your /var/www/html folder and also disable ```SELinux sudo setenforce 0```**

To make this change permanent – open following config file `sudo vi /etc/sysconfig/selinux` and set **SELINUX=disabled** then restart httpd.
```
sudo vi /etc/sysconfig/selinux 
```
![Picture1 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0414c702-d7db-442f-b97d-005e6a491508)

12.	Update the website’s configuration to connect to the database (in /var/www/html/functions.php file).

```
sudo vi /var/www/html/functions.php
```
![Picture1 10](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6c1680a3-439b-4bb4-9f25-980672438cfa)

Apply **tooling-db.sql** script to your database using the below command on webserver
```
#mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql 
mysql -h 10.0.16.32 -u webaccess -p tooling < tooling-db.sql
```
![Picture1 11](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7a8c4aea-0c35-45b7-baea-d0491d041578)

NOTE: open port 3306 on DBServer and allow webserver private IP and install mysql on the webserver

![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4d4008f8-29f8-46d0-b234-b121b517d221)
```
sudo yum install mysql -y
```

In the database server update the bind address to 0.0.0.0: 
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5a414c95-d27a-4912-8998-3c0ea66c1dcd)

![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/26a7e915-78a7-4077-a41c-f7a0659b39db)

```
sudo systemctl status mysql
```
13.	Create in MySQL a new admin user with username: `myuser and password: password:`
```
INSERT INTO ‘users’ (‘id’, ‘username’, ‘password’, ’email’, ‘user_type’, ‘status’) VALUES
-> (1, ‘myuser’, ‘5f4dcc3b5aa765d61d8327deb882cf99’, ‘user@mail.com’, ‘admin’, ‘1’);
```
14.	Open the website in your browser `http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php` and make sure you can login into the website with myuser user.

![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/8ad5a496-3c3f-4e08-9ce1-4a4260915659)

![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f86445a5-8173-48af-abc6-af0803c1958c)

