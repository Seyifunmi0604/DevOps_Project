## Step 5 — Configure DB to work with WordPress
```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0c3150ef-f154-41b0-be59-a85874cf51c7)

Exit and run
```
sudo mysql_secure_installation #and set password to be” mypass”
```
Set bind address:
```
sudo vi /etc/my.cnf
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/02a84186-198d-40d9-974e-f6122b4944dd)
