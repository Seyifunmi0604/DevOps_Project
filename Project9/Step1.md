## INSTALL AND CONFIGURE JENKINS SERVER

# Step 1 – Install Jenkins server

1.	Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
2.	Install [JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) (since Jenkins is a Java-based application)
   
```
sudo apt update
sudo apt install default-jdk-headless
java --version
```
![9 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/2a0f4d31-72cb-49d5-8a76-066104e47330)

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
 /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt update
sudo apt-get install jenkins
```
**Make sure Jenkins is up and running**
```
sudo systemctl status jenkins
```
![9 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5df78db3-a8fb-42cc-a48f-309e9934cde8)

4.	By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

![9 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/cf6d8f91-3285-4738-979e-2d5902b64732)

5.	Perform initial Jenkins setup.
From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

![9 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7c8bfcc1-ccd8-49c8-9180-eb391d9c4605)

You will be prompted to provide a default admin password

Retrieve it from your server:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![9 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e8598feb-3fe4-4643-b3d3-0287834b5a9c)

Then you will be asked which plugins to install – choose suggested plugins.

![9 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b1d6ae4b-41de-4c8a-9558-ae69f74cdffe)

![9 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/1964f134-85a4-4b2c-950c-4d7f3b0a6f21)

![9 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/405bfba3-f5bd-4f20-a3dc-b2ad9bc90213)


































