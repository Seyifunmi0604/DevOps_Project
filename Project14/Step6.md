# SONARQUBE INSTALLATION

Before we start getting hands on with SonarQube configuration, it is incredibly important to understand a few concepts:

•	[Software Quality](https://en.wikipedia.org/wiki/Software_quality) – The degree to which a software component, system or process meets specified requirements based on user needs and expectations.

•	[Software Quality Gates](https://docs.sonarsource.com/sonarqube/latest/user-guide/quality-gates/) – Quality gates are basically acceptance criteria which are usually presented as a set of predefined quality criteria that a software development project must meet in order to proceed from one stage of its lifecycle to the next one.

SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as [The Sonar Way](https://docs.sonarsource.com/sonarqube/latest/instance-administration/quality-profiles/)). Software testers and developers would normally work with project leads and architects to create custom quality gates.

## Install SonarQube on Ubuntu 20.04 With PostgreSQL as Backend Database

**Update playbooks/site.yml**

![14 80](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6b54df0d-0560-4170-b73e-961a5c567dd7)

**Update inventory/ci**

![14 81](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/91d41a97-02df-4fc0-a246-e0921efbbba6)

**Commit and push the changes**

**Run or build with parameter “ci” from Jenkins UI**

![14 82](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/89c872ce-fe7d-453c-a4e8-875664ef6a26)

![14 83](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5076ec94-20fe-40b8-a969-d84a3bd2407f)

![14 84](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5ae74199-d3e0-4612-aa1c-a69a8e278e51)

![14 85](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4d0e646e-4f80-4823-90fe-acadfb802be8)

**Update ansible.cfg with:**

```
roles_path = /home/ec2-user/proj14-ansible-config/roles
```
![14 86](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d30b5316-3008-41bf-9972-2b8efc64768a)

**Ansible.cfg file**

```
[defaults]
roles_path = /home/ec2-user/proj14-ansible-config/roles
timeout = 160
callback_whitelist = profile_tasks
log_path=~/ansible.log
host_key_checking = False
gathering = smart
ansible_python_interpreter=/usr/bin/python3
allow_world_readable_tmpfiles=true
```

Run: 

```
export ANSIBLE_CONFIG=/home/ec2-user/proj14-ansible-config/deploy/ansible.cfg
```

Run: 

```
ansible-galaxy collection install community.postgresql
```

![14 87](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/98b7fdda-eba3-45b4-8647-5b523ff37e80)

```
ansible-playbook -i inventory/ci playbooks/site.yml
```

![14 88](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/41f50ba3-4cf3-4e91-aefa-e5917921bd15)

![14 89](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7e564676-0c0e-4b7d-be3e-077ad14a6188)

Open port 9000 for sonaqube to be accessible on the browser

**publicIP:9000**

![14 90](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0b97e043-7433-4a9f-a665-d6478b0406cf)

![14 91](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e4b70167-daa5-47ac-bca6-e5ff91e9704e)

**Now, when SonarQube is up and running, it is time to setup our Quality gate in Jenkins.**

# Here is a manual approach to installation. Ensure that you can to automate the same with Ansible.

Below is a step by step guide how to install **SonarQube 7.9.3** version. It has a strong prerequisite to have Java installed since the tool is Java-based. MySQL support for SonarQube is deprecated, therefore we will be using PostgreSQL.
We will make some Linux Kernel configuration changes to ensure optimal performance of the tool – we will increase **vm.max_map_count**, **,file discriptor**, and **,ulimit**

**Tune Linux Kernel**

This can be achieved by making session changes which does not persist beyond the current session terminal.

```
sudo sysctl -w vm.max_map_count=262144
sudo sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```
To make a permanent change, edit the file **/etc/security/limits.conf** and append the below

```
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```

**Before installing, let us update and upgrade system packages:**

```
sudo apt-get update
sudo apt-get upgrade
```
**Install [wget](https://www.gnu.org/software/wget/) and [unzip](https://linux.die.net/man/1/unzip) packages**

```
sudo apt-get install wget unzip -y
```
**Install [OpenJDK](https://openjdk.org/) and [Java Runtime Environment (JRE) 11](https://docs.oracle.com/goldengate/1212/gg-winux/GDRAD/java.htm#BGBFJHAB)**

```
 sudo apt-get install openjdk-11-jdk -y
 sudo apt-get install openjdk-11-jre -y
 ```

**Set default JDK – To set default JDK or switch to OpenJDK enter below command:**

```
 sudo update-alternatives --config java
```

If you have multiple versions of Java installed, you should see a list like below:

```
Selection    Path                                            Priority   Status

------------------------------------------------------------

  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode

  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode

  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

* 3            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode
```

**Type "1" to switch OpenJDK 11**

**Verify the set JAVA Version:**

```
java -version
```
**Output**

`
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment (build 11.0.7+10-post-Ubuntu-3ubuntu1)
OpenJDK 64-Bit Server VM (build 11.0.7+10-post-Ubuntu-3ubuntu1, mixed mode, sharing)
`

**Install and Setup PostgreSQL 10 Database for SonarQube**

The command below will add PostgreSQL repo to the repo list:

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```
**Download PostgreSQL software**

```
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
```
**Install PostgreSQL Database Server**

```
sudo apt-get -y install postgresql postgresql-contrib
```
**Start PostgreSQL Database Server**

```
sudo systemctl start postgresql
```

**Enable it to start automatically at boot time**

```
sudo systemctl enable postgresql
```

**Change the password for default **postgres** user (Pass in the password you intend to use, and remember to save it somewhere)**

```
sudo passwd postgres
```

**Switch to the postgres user**

```
su - postgres
```

**Create a new user by typing**

```
createuser sonar
```

**Switch to the PostgreSQL shell**

```
psql
```
**Set a password for the newly created user for SonarQube database**

```
ALTER USER sonar WITH ENCRYPTED password 'sonar';
```

**Create a new database for PostgreSQL database by running:**

```
CREATE DATABASE sonarqube OWNER sonar;
```
**Grant all privileges to sonar user on sonarqube Database.**

```
grant all privileges on DATABASE sonarqube to sonar;
```
**Exit from the psql shell:**

```
\q
```
**Switch back to the sudo user by running the exit command.**

```
exit
```

**Install SonarQube on Ubuntu 20.04 LTS**

**Navigate to the tmp directory to temporarily download the installation files**

```
cd /tmp && sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.3.zip
```

**Unzip the archive setup to **/opt directory**

```
sudo unzip sonarqube-7.9.3.zip -d /opt
```
**Move extracted setup to /opt/sonarqube directory**

```
sudo mv /opt/sonarqube-7.9.3 /opt/sonarqube
```


















