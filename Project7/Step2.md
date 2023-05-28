# STEP 2 — CONFIGURE THE DATABASE SERVER

By now you should know how to install and configure a MySQL DBMS to work with remote Web Server

**1.	Install MySQL server**
```
sudo apt update -y
sudo apt install mysql-server -y
```
**2.	Create a database and name it tooling**
```
sudo mysql
create database tooling;
```
**3.	Create a database user and name it webaccess**
```
Create user ‘webaccess’@’webserverCIDR’identified by ‘password’;
```
```
create user 'webaccess'@'10.0.16.0/20' identified by 'password';
```
**4.	Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr**
```
grant all privileges on tooling.* to 'webaccess'@'10.0.16.0/20';

flush privileges;

show databases;
```
![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b5916c92-35a3-45d6-b1b0-b637b607db07)







