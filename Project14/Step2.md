## SIMULATING A TYPICAL CI/CD PIPELINE FOR A PHP BASED APPLICATION
As part of the ongoing infrastructure development with Ansible started from Project 11, you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both **Tooling** and **TODO** Web Applications are based on an interpreted ([scripting)](https://en.wikipedia.org/wiki/Scripting_language) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible [uri module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html).

![14 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b1805c1f-6b40-4f1f-ad8a-2d9135da5bb7)

Set Up
This project is partly a continuation of your Ansible work, so simply add and subtract based on the new setup in this project. It will require a lot of servers to simulate all the different environments from **dev/ci** all the way to **production**. This will be quite a lot of servers altogether (But you don’t have to create them all at once. Only create servers required for an environment you are working with at the moment. For example, when doing deployments for development, do not create servers for integration, pentest, or production yet).

Try to utilize your AWS free tier as much as you can, you can also register a new account if you have exhausted the current one. Alternatively, you can use [Google Cloud (GCP)](https://cloud.google.com/) to rent virtual machines from this cloud service provider – you can get $300 credit [here](https://clozon.com/try-google-cloud-services-and-get-300-credit-with-a-12-month-free-trial/) or [here](https://www.startups.com/products/benefits/googlecloud) (NOTE: Please read instructions carefully to get your credits)

NOTE: This is still NOT a cloud-focus project yet. AWS cloud end to end project begins from project-15. Therefore, do not worry about advanced AWS or GCP configuration. All we need here is virtual machines that can be accessed over SSH.

To minimize the cost of cloud servers, you don not have to create all the servers at once, simply spin up a minimal server set up as you progress through the project implementation and have reached a need for more.

To get started, we will focus on these environments initially.

Ci
Dev
Pentest

Both **SIT** – For **System Integration Testing** and **UAT** – User **Acceptance Testing** do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But **Pentest** – For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for **Performance and Load testing**. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams.

<img width="832" alt="14 5" src="https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/24a83486-ac23-4113-a627-b1f5dfc1aa5a">

**CI-Environment**

![14 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6585af37-575d-413b-9b0e-09db55d22c83)

**Other Environments from Lower To Higher**

![14 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/90ee0afe-d605-4cab-b29e-4d0a153c1b63)

**DNS requirements**

Make DNS entries to create a subdomain for each environment. Assuming your main domain is darey.io

You should have a subdomains list like this:

![14 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a2d31cc4-23e1-4259-950c-0f464c90174c)

1.	Spin up a redhat t2.medum

  •	Update .ssh/config file on vscode
  •Install git
  
```
sudo yum install git -y
```
```
git clone https://github.com/Seyifunmi0604/proj14ansible-config.git
```
	
2. **Install wget**

```
sudo yum install wget -y
```
3.	[Install Jenkins](https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos)

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key 
sudo yum upgrade -y
**Add required dependencies for the jenkins package**
sudo yum install java-17-openjdk -y
sudo yum install jenkins -y
sudo systemctl daemon-reload
```

```
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```
**Update bash profile**
```
##### open the bash profile 
sudo vi .bash_profile 

##### paste the below in the bash profile
export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which javac)))))
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar

##### reload the bash profile
source ~/.bash_profile

```
**Restart Jenkins**

```
sudo systemctl start Jenkins
sudo systemctl status Jenkins
```
**copy jenkins PublicIP on the browser to grab default password to access jenkins UI**

![14 10](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/403f09be-de1f-42e2-b987-f97af247c61f)

**Ansible Inventory should look like this**

```
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat
```
**ci inventory file**

```
[jenkins]
<Jenkins-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[sonarqube]
<SonarQube-Private-IP-Address>

[artifact_repository]
<Artifact_repository-Private-IP-Address>
```
**dev Inventory file**

```
[tooling]
<Tooling-Web-Server-Private-IP-Address>

[todo]
<Todo-Web-Server-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<DB-Server-Private-IP-Address>
```
**pentest inventory file**
```
[pentest:children]
pentest-todo
pentest-tooling

[pentest-todo]
<Pentest-for-Todo-Private-IP-Address>

[pentest-tooling]
<Pentest-for-Tooling-Private-IP-Address>
```
**Observations:**

1. You will notice that in the pentest inventory file, we have introduced a new concept **pentest:children** This is because, we want to have a group called **pentest** which covers Ansible execution against both **pentest-todo** and **pentest-tooling** simultaneously. But at the same time, we want the flexibility to run specific Ansible tasks against an individual group.
2. The **db** group has a slightly different configuration. It uses a RedHat/Centos Linux distro. Others are based on Ubuntu (in this case user is ubuntu). Therefore, the user required for connectivity and path to python interpreter are different. If all your environment is based on Ubuntu, you may not need this kind of set up. Totally up to you how you want to do this. Whatever works for you is absolutely fine in this scenario.

This makes us to introduce another Ansible concept called **group_vars**. With group vars, we can declare and set variables for each group of servers created in the inventory file.

For example, If there are variables we need to be common between both **pentest-todo** and **pentest-tooling**, rather than setting these variables in many places, we can simply use the **group_vars** for **pentest**. Since in the inventory file it has been created as **pentest:children** Ansible recognizes this and simply applies that variable to both children.





