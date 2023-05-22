## Step 4 — Install MySQL on your DB Server EC2
```
sudo yum update -y
sudo yum install mysql-server -y
```
Verify that the service is up and running by using **sudo systemctl status mysqld**, if it is not running, restart the service and enable it so it will be running even after reboot:
```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

