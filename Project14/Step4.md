## RUNNING ANSIBLE PLAYBOOK FROM JENKINS
Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

1.	**Installing Ansible on Jenkins**
```
sudo yum install ansible -y
```
![14 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c7ad4903-9433-43f3-8030-632e361ae37d)

**Install the below dependencies:**

```
sudo python3 -m pip install --upgrade setuptools
sudo python3 -m pip install --upgrade pip
sudo python3 -m pip install PyMySQL
sudo python3 -m pip install mysql-connector-python
sudo python3 -m pip install psycopg2==2.7.5 --ignore-installed
```

```
ansible-galaxy collection install community.mysql
```

2.	**Installing Ansible plugin in Jenkins UI**

![14 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/08dbb0d4-259b-450c-b320-fde5689dd49a)

3.	Creating **Jenkinsfile** from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)

You can [watch a 10 minutes video here](https://www.youtube.com/watch?v=PRpEbFZi7nI) to guide you through the entire setup
Note: Ensure that Ansible runs against the Dev environment successfully.
Possible errors to watch out for:

1.	Ensure that the git module in **Jenkinsfile** is checking out SCM to **main** branch instead of **master** (GitHub has discontinued the use of **Master** due to Black Lives Matter. You can read more [here](https://www.cnet.com/tech/computing/microsofts-github-is-removing-coding-terms-like-master-and-slave/))
	
2.	Jenkins needs to export the **ANSIBLE_CONFIG** environment variable. You can put the **.ansible.cfg** file alongside **Jenkinsfile** in the **deploy** directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set.

![14 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ade3a94d-a4bf-44ae-8ae4-a4227b42070c)

https://wiki.jenkins.io/display/JENKINS/Building+a+software+project

![14 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/05006c37-915d-430b-b644-4d37f893a30c)

Possible issues to watch out for when you implement this
1.	Remember that **ansible.cfg** must be exported to environment variable so that Ansible knows where to find **Roles**. But because you will possibly run Jenkins from different git branches, the location of Ansible roles will change. Therefore, you must handle this dynamically. You can use Linux [Stream Editor](https://www.gnu.org/software/sed/manual/sed.html)  sed to update the section **roles_path** each time there is an execution. You may not have this issue if you run only from the main branch.

**ansible.cfg**

```
[defaults]
timeout = 160
callback_whitelist = profile_tasks
log_path=~/ansible.log
host_key_checking = False
gathering = smart
ansible_python_interpreter=/usr/bin/python3
allow_world_readable_tmpfiles=true

[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=30m -o ControlPath=/tmp/ansible-ssh-%h-%p-%r -o ServerAliveInterval=60 -o ServerAliveCountMax=60 -o ForwardAgent=yes
```
2.	If you push new changes to **Git** so that Jenkins failure can be fixed. You might observe that your change may sometimes have no effect. Even though your change is the actual fix required. This can be because Jenkins did not download the latest code from GitHub. Ensure that you start the **Jenkinsfile** with a clean up step to always delete the previous workspace before running a new one. Sometimes you might need to login to the Jenkins Linux server to verify the files in the workspace to confirm that what you are actually expecting is there. Otherwise, you can spend hours trying to figure out why Jenkins is still failing, when you have pushed up possible changes to fix the error.

3.	Another possible reason for Jenkins failure sometimes, is because you have indicated in the **Jenkinsfile** to check out the **main** git branch, and you are running a pipeline from another branch. So, always verify by logging onto the Jenkins box to check the workspace, and run **git branch** command to confirm that the branch you are expecting is there.

spin up instance for nginx Redhat and db ubuntu.

Create ansible.cfg file inside deploy folder

**Configure Global Credentials in Jenkins UI**









