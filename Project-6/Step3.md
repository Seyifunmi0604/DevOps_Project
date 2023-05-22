## Step 3 — Install WordPress on your Web Server EC2

1.	Update the repository
```
sudo yum -y update
```
2.	Install wget, Apache and it’s dependencies
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
3.	Start Apache
```
sudo systemctl enable httpd
sudo systemctl start httpd
```
4.	To install PHP and it’s depemdencies
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```
5.	Restart Apache
```
sudo systemctl restart httpd
```
6.	Download wordpress and copy wordpress to **/var/www/html**
```
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/
```
```
ls -l
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/54426479-dd2d-4105-a206-02c1e2798563)
```
cd wordpress/ and run ls -l
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/74b22932-2719-4ca1-b0e7-302d9b8dbd97)

7.	Configure **SELinux** Policies
```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```







