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



















