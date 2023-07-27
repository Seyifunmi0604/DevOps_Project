## CONFIGURE SONARQUBE

We cannot run SonarQube as a root user, if you run using root user it will stop automatically. The ideal approach will be to create a separate group and a user to run SonarQube

**Create a group sonar**

```
sudo groupadd sonar
```

**Now add a user with control over the /opt/sonarqube directory**

```
 sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar 
 sudo chown sonar:sonar /opt/sonarqube -R
```

**Open SonarQube configuration file using your favourite text editor (e.g., nano or vim)**

```
sudo vi /opt/sonarqube/conf/sonar.properties
```

**Find the following lines:**

```
#sonar.jdbc.username=
#sonar.jdbc.password=
```
Uncomment them and provide the values of PostgreSQL Database username and password:

![image](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ac85c23e-61ec-4acf-a10d-194dcd4589bd)

**Edit the sonar script file and set RUN_AS_USER**

```
sudo vi /opt/sonarqube/bin/linux-x86-64/sonar.sh
```
![image](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/95d510bb-b35e-4344-9e30-a6513fad781e)

**Now, to start SonarQube we need to do following:**

**Switch to sonar user**

```
sudo su sonar
```
**Move to the script directory**

```
cd /opt/sonarqube/bin/linux-x86-64/
```
**Run the script to start SonarQube**

```
./sonar.sh start
```

**Expected output shall be as:**

`
Starting SonarQube...

Started SonarQube


**Check SonarQube running status:**

```
./sonar.sh status
```

**Sample Output below:**

`
./sonar.sh status

SonarQube is running (176483).


**To check SonarQube logs, navigate to /opt/sonarqube/logs/sonar.log directory**

```
tail /opt/sonarqube/logs/sonar.log
```
**Output**

`
INFO  app[][o.s.a.ProcessLauncherImpl] Launch process[[key='ce', ipcIndex=3, logFilenamePrefix=ce]] from [/opt/sonarqube]: /usr/lib/jvm/java-11-openjdk-amd64/bin/java -Djava.awt.headless=true -Dfile.encoding=UTF-8 -Djava.io.tmpdir=/opt/sonarqube/temp --add-opens=java.base/java.util=ALL-UNNAMED -Xmx512m -Xms128m -XX:+HeapDumpOnOutOfMemoryError -Dhttp.nonProxyHosts=localhost|127.*|[::1] -cp ./lib/common/*:/opt/sonarqube/lib/jdbc/h2/h2-1.3.176.jar org.sonar.ce.app.CeServer /opt/sonarqube/temp/sq-process15059956114837198848properties

 INFO  app[][o.s.a.SchedulerImpl] Process[ce] is up

 INFO  app[][o.s.a.SchedulerImpl] SonarQube is up
`

**You can see that SonarQube is up and running**

# Configure SonarQube to run as a systemd service

**Stop the currently running SonarQube service**

```
 cd /opt/sonarqube/bin/linux-x86-64/
```

**Run the script to start SonarQube**

```
./sonar.sh stop
```

**Create a systemd service file for SonarQube to run as System Startup.**

```
 sudo vi /etc/systemd/system/sonar.service
```

**Add the configuration below for systemd to determine how to start, stop, check status, or restart the SonarQube service.**

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```

**Save the file and control the service with systemctl**

```
sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar
```

**Access SonarQube**
To access SonarQube using browser, type server’s IP address followed by **port 9000**

```
http://server_IP:9000 OR http://localhost:9000
```

**Login to SonarQube with default administrator username and password – admin**

![14 92](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/070343bb-611c-4da9-b283-ad80165b0bfe)

Now, when SonarQube is up and running, it is time to setup our Quality gate in Jenkins.
















































