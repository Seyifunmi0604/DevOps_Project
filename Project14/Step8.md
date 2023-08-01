# CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE

•	In Jenkins, install [SonarScanner plugin](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/jenkins-extension-sonarqube/) 

![14 93](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c03f2f59-ba57-4b61-bb06-48884b8027c0)

•	Navigate to configure system in Jenkins. Add SonarQube server as shown below:

**Manage Jenkins >> Configure System**

![14 94](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f4b7b2b0-565c-4076-be33-07d6b1c61da3)

**Generate authentication token in SonarQube**

**User > My Account > Security > Generate Tokens**

![14 95](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ff7b7741-1f32-4128-a903-3c80e74cd317)

•	** Configure Quality Gate Jenkins Webhook in SonarQube – The URL should point to your Jenkins server http://{JENKINS_HOST}/sonarqube-webhook/**

**Administration > Configuration > Webhooks > Create**

![14 96](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0ec17f0a-5284-4a80-80b4-728cf4a52809)

![14 97](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/9c19b4bb-b85b-419e-b021-6ca8319e33cc)

![14 98](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/328ee1e9-f13f-4d1c-920b-1dd15b7be4b7)

**Setup SonarQube scanner from Jenkins – Global Tool Configuration**

**Manage Jenkins > Global Tool Configuration**

![14 99](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c203ada8-fb91-43fc-aeb6-0e7d43ac68f0)

![14 100](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ca1dfd35-6028-48d9-8182-496510aa1e44)

**Update Jenkins Pipeline to include SonarQube scanning and Quality Gate**

Below is the snippet for a Quality Gate stage in **Jenkinsfile**.

```
stage('SonarQube Quality Gate') {
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
            }

        }
    }
```

**NOTE: The above step will fail because we have not updated `sonar-scanner.properties**

•	Configure **sonar-scanner.properties** – From the step above, Jenkins will install the scanner tool on the Linux server. You will need to go into the tools directory on the server to configure the properties file in which SonarQube will require to function during pipeline execution.

```
cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/
```
![14 101](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0b98b81d-4743-4a45-ae61-a93d10524be4)

**Open sonar-scanner.properties file**

```
sudo vi sonar-scanner.properties
```

![14 102](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a5dc0846-d361-4167-ad43-a2344b2bd960)

**Add configuration related to php-todo project**

```
sonar.host.url=http://<SonarQube-Server-IP-address>:9000
sonar.projectKey=php-todo
#----- Default source code encoding
sonar.sourceEncoding=UTF-8
sonar.php.exclusions=**/vendor/**
sonar.php.coverage.reportPaths=build/logs/clover.xml
sonar.php.tests.reportPath=build/logs/junit.xml
```

![14 103](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/782c8d1b-641a-4bf2-9aef-b036f4f0322e)

**HINT: To know what exactly to put inside the sonar-scanner.properties file, SonarQube has a configurations page where you can get some directions.**

![14 104](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e488fc8b-7578-42c1-b4d2-7c44b759375e)

A brief explanation of what is going on the the stage – set the environment variable for the scannerHome use the same name used when you configured SonarQube Scanner from **Jenkins Global Tool Configuration**. If you remember, the name was SonarQubeScanner. Then, within the steps use shell to run the scanner from bin directory.

To further examine the configuration of the scanner tool on the Jenkins server – navigate into the tools directory

```
cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/bin
```

List the content to see the scanner tool sonar-scanner. That is what we are calling in the pipeline script.

Output of **ls -latr**

`
ubuntu@ip-172-31-16-176:/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/bin$ ls -latr
total 24
-rwxr-xr-x 1 jenkins jenkins 2550 Oct  2 12:42 sonar-scanner.bat
-rwxr-xr-x 1 jenkins jenkins  586 Oct  2 12:42 sonar-scanner-debug.bat
-rwxr-xr-x 1 jenkins jenkins  662 Oct  2 12:42 sonar-scanner-debug
-rwxr-xr-x 1 jenkins jenkins 1823 Oct  2 12:42 sonar-scanner
drwxr-xr-x 2 jenkins jenkins 4096 Dec 26 18:42 .
`
So far you have been given code snippets on each of the stages within the Jenkinsfile. But, you should also be able to generate Jenkins configuration code yourself.
•	To generate Jenkins code, navigate to the dashboard for the **php-todo** pipeline and click on the **Pipeline Syntax** menu item

`
Dashboard > php-todo > Pipeline Syntax 
`
![14 105](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b65f7ef1-dd1c-494b-b008-9165596bee0d)

![14 106](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/899b4314-f900-4897-a686-349e3a169a1b)

![14 107](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/62eb243b-7167-4ae7-9b8e-e00b420c9d1e)

![14 108](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6c2a5de8-c2c4-4fa5-ba4f-e0aec2754bbf)

![14 109](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/84199b91-4f35-4524-b954-6a576dbc9e46)

In the development environment, this is acceptable as developers will need to keep iterating over their code towards perfection. But as a DevOps engineer working on the pipeline, we must ensure that the quality gate step causes the pipeline to fail if the conditions for quality are not met.

# Conditionally deploy to higher environments

In the real world, developers will work on feature branch in a repository (e.g., GitHub or GitLab). There are other branches that will be used differently to control how software releases are done. You will see such branches as:

•	Develop

•	Master or Main

(The * is a place holder for a version number, Jira Ticket name or some description. It can be something like Release-1.0.0)

•	Feature/*

•	Release/*

•	Hotfix/*
etc.

There is a very wide discussion around release strategy, and git branching strategies which in recent years are considered under what is known as GitFlow (Have a read and keep as a bookmark – it is a possible candidate for an interview discussion, so take it seriously!)

Assuming a basic gitflow implementation restricts only the develop branch to deploy code to Integration environment like sit.
Let us update our Jenkinsfile to implement this:

•	First, we will include a When condition to run Quality Gate whenever the running branch is either develop, hotfix, release, main, or master

```
when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
```
•	Then we add a timeout step to wait for SonarQube to complete analysis and successfully finish the pipeline only when code quality is acceptable.

```
    timeout(time: 1, unit: 'MINUTES') {
        waitForQualityGate abortPipeline: true
    }
```

**The complete stage will now look like this:**

```
    stage('SonarQube Quality Gate') {
      when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
            }
            timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }
```

To test, create different branches and push to GitHub. You will realise that only branches other than develop, hotfix, release, main, or master will be able to deploy the code.

If everything goes well, you should be able to see something like this:

![14 107](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f5633ca4-90e8-43e7-b27c-1fe880c7db13)

Notice that with the current state of the code, it cannot be deployed to Integration environments due to its quality. In the real world, DevOps engineers will push this back to developers to work on the code further, based on SonarQube quality report. Once everything is good with code quality, the pipeline will pass and proceed with sipping the codes further to a higher environment.

**Complete the following tasks to finish Project 14**

1.	**Introduce Jenkins agents/slaves – Add 2 more servers to be used as Jenkins slave. Configure Jenkins to run its pipeline jobs randomly on any available slave nodes.**

**Spin up Redhad ec2 instance**

![14 108](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d8a7acda-71cd-427f-a6dd-da5d048b246d)

![14 110](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/59145f8c-58d9-4645-8ffd-a90ce7994e69)

![14 111](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a1dec204-5a49-479f-9dbb-5750d5767ecf)


2.	**Configure webhook between Jenkins and GitHub to automatically run the pipeline when there is a code push.**

![14 112](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/29a19557-ad48-40c7-875b-dfb93097122c)

![14 113](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/53a72496-0c5b-44db-beb5-055ec0c69345)

![14 114](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/23602358-09d7-40c7-ac62-bc8833f63594)

3.	Deploy the application to all the environments
   
4.	**Optional** – Experience pentesting in pentest environment by [configuring Wireshark](https://www.wireshark.org/) there and just explore for information sake only. Watch Wireshark Tutorial here. VIdeo is no longer availabe.

o	Ansible Role for Wireshark:
o	https://github.com/ymajik/ansible-role-wireshark (Ubuntu)
o	https://github.com/wtanaka/ansible-role-wireshark (RedHat)


[14.video.zip](https://github.com/Seyifunmi0604/DevOps_Project/files/12226129/14.video.zip)


Congratulations! You have just experienced one of the most interesting and complex projects in you Project Based Learning journey so far. The vast experience and knowledge you have acquired here will set the stage for the next 6 projects to come. You should be ready to start applying for DevOps jobs after completing Project 20.





