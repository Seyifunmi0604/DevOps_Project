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

•	spin up instance for nginx Redhat and db ubuntu.

•	Create ansible.cfg file inside deploy folder

**Configure Global Credentials in Jenkins UI**

![14 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/463661bc-ae4e-4f12-890d-3d7908d98185)

![14 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c9d7405d-b142-4331-9763-7779575c79ed)

**Add ansible in Jenkins UI**

![14 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/8cd5b0d7-3dbb-497e-88e4-ddb0c7a663ac)

• Create pipeline syntax

![14 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c5a193ab-ac07-4bae-b7b0-f1c82c227dbd)

![14 10](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/9c3bb7dd-6576-41a0-b329-a3788d537cc5)

![14 11](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5c800748-9984-461d-9867-6e348733186c)

![14 12](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a4d2adea-7a53-47e7-8adf-d5e8f116c3c8)

![14 13](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/52402e5d-bdb6-4f3e-ad6a-1cabced39848)

If everything goes well for you, it means, the Dev environment has an up-to-date configuration. But what if we need to deploy to other environments?

•	Are we going to manually update the Jenkinsfile to point inventory to those environments? such as sit, uat, pentest, etc.

•	Or do we need a dedicated git branch for each environment, and have the inventory part hard coded there.

Think about those for a minute and try to work out which one sounds more like a better solution.

Manually updating the Jenkinsfile is definitely not an option. And that should be obvious to you at this point. Because we try to automate things as much as possible.
Well, unfortunately, we will not be doing any of the highlighted options. What we will be doing is to parameterise the deployment. So that at the point of execution, the appropriate values are applied.

**Parameterizing Jenkinsfile For Ansible Deployment**

To deploy to other environments, we will need to use parameters.

1.	Update sit inventory with new servers
   
```
[tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>

```

2.	Update Jenkinsfile to introduce parameterization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose.
   
```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }

```

3.	In the Ansible execution section, remove the hardcoded inventory/dev and replace with `${inventory}
From now on, each time you hit on execute, it will expect an input.

![14 20](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/65f40faf-11ab-4a66-b37d-5b383afd870f)

Notice that the default value loads up, but we can now specify which environment we want to deploy the configuration to. Simply type sit and hit Run

![14 21](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/200040f1-702d-428d-918d-07b0ea94061e)

4.	Add another parameter. This time, introduce tagging in Ansible. You can limit the Ansible execution to a specific role or playbook desired. Therefore, add an Ansible tag to run against webserver only. Test this locally first to get the experience. Once you understand this, update Jenkinsfile and run it from Jenkins.

Jenkinsfile

```
pipeline {
    agent any

  environment {
    ANSIBLE_CONFIG = "${WORKSPACE}/deploy/ansible.cfg"
  }

  parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }

  stages{
    stage("Initial cleanup") {
      steps {
        dir("${WORKSPACE}") {
          deleteDir()
        }
      }
    }

    stage('Checkout SCM') {
      steps {
        git branch: 'feature/jenkinspipeline-stages', url: 'https://github.com/Seyifunmi0604/proj14-ansible-config.git'
      }
    }

    stage('Prepare Ansible For Execution') {
      steps {
        sh 'echo ${WORKSPACE}'
        sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'
      }
    }

    stage('Run Ansible playbook') {
      steps {
        ansiblePlaybook become: true, colorized: true, credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory/${inventory}', playbook: 'playbooks/site.yml'
      }
    }

    stage('Clean Workspace after build') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
      }
    }
  }

}


```

![14 14](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/8f80586f-857d-4d7a-a2be-f72093dd0656)

![14 15](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/9f360e25-51b3-475f-b792-8ea103f9d5a7)

![14 16](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/37f5e460-465b-4c7b-acb9-b0a71ece1549)

![14 17](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/1ea5ce32-9a53-491e-bb9b-99ef8fed2748)

![14 18](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/18379965-fa2a-4b3a-935d-2ee0505900ab)

![14 19](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4acd607a-b2bd-40f5-887e-4beb08bd59e7)

















