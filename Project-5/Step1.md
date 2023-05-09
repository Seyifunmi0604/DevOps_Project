## TASK – Implement a Client Server Architecture using MySQL Database Management System (DBMS).

To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

1.	Create and configure two Linux-based virtual servers (EC2 instances in AWS).
```
Server A name - `mysql server`

sudo apt update -y
sudo apt upgrade -y
```
```
Server B name - `mysql client`

sudo apt update -y
sudo apt upgrade -y
```
2.	On mysql server Linux Server install MySQL Server software.
```
sudo apt install mysql-server -y

sudo systemctl status mysql

sudo systemctl enable mysql
```
```
sudo mysql_secure_installation
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/bbc6a4e0-ac22-4cf4-bb85-079e95cafabf)


Interesting fact: [MySQL](https://www.mysql.com/) is an open-source relational database management system. Its name is a combination of "My", the name of co-founder Michael Widenius’s daughter, and "SQL", the abbreviation for Structured Query Language.

3.	On mysql client Linux Server install MySQL Client software.
```
sudo apt install mysql-client -y
```
4.	By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

5.	You might need to configure MySQL server to allow connections from remote hosts.
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
**Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:**

![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d99715e3-1c4b-4189-b0f5-a5d23cd96ece)

6.	From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.
```
sudo mysql -u remote_user -h 10.0.27.237 -p #the IP is <DBprivateIP>
```
7.	Check that you have successfully connected to a remote MySQL server and can perform SQL queries:

Run the below on MySQLServer

In a situation where **sudo mysql_secure_installation** failed to work, run sudo **mysql** and then run the below instead
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password'; 
```
```
CREATE USER ‘remote_user’@’%’IDENTIFIED WITH mysql_native_password BY ‘PassWord.1’; 

CREATE DATABASE test_db;

GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```
```
Show databases;
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a40e0e3f-2a07-4ec3-9bad-51363d364b59)



