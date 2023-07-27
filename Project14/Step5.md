## CI/CD PIPELINE FOR TODO APPLICATION

We already have **tooling** website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

Our goal here is to deploy the application onto servers directly from Artifactory rather than from git. If you have not updated Ansible with an Artifactory role, simply use this guide to create an Ansible role for Artifactory (ignore the Nginx part). [Configure Artifactory on Ubuntu 20.04](https://www.howtoforge.com/tutorial/ubuntu-jfrog/)

**Phase 1 – Prepare Jenkins**

1.  **Fork the repository below into your GitHub account**
   
```
https://github.com/darey-devops/php-todo.git
```
2.  **On you Jenkins server, install PHP, its dependencies and [Composer tool](https://getcomposer.org/) (Feel free to do this manually at first, then update your Ansible accordingly later)**

```
sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}
```
Installing php on Redhat
```
yum module reset php -y
yum module enable php:remi-7.4 -y
yum install -y php  php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd    php-fpm php-json
systemctl start php-fpm
systemctl enable php-fpm
```

3.  **Install Jenkins plugins**
   
    i.  [Plot plugin](https://plugins.jenkins.io/plot/)

![14 24](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/9a9c0115-bd69-4a8d-8a08-f86faa725d5f)

   ii. [Artifactory plugin](https://jfrog.com/help/r/jfrog-integrations-documentation/jenkins-artifactory-plug-in)
      
![14 25](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5f11ac23-fc04-48ea-96b9-81f4a2ce37f0)
        
•  We will use **plot** plugin to display tests reports, and code coverage information.

•  The **Artifactory** plugin will be used to easily upload code artifacts into an Artifactory server.

**Spin up REDHAT ec2 instance for artifacory server**

**Modify inventory/ci**

![14 26](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/564b8520-eafd-4d77-bd3d-c807182e8921)

**Modify playbooks/site.yml**

![14 27](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5fc55629-225e-48ef-b86f-555ea90986c7)

**Push the changes to git, create pull request and checkout to main branch and run git pull**

Let’s run our playbook against inventory/ci from Jenkins UI

![14 28](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f62c3f5f-a963-4dde-86ef-082064539331)

![14 29](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a8727efb-dca7-4540-bc56-e76b9b34ebab)

**Check from the browser with artifactory publicIP if JFrog is accessible but note open port 8082**

![14 30](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/21702f9b-62e7-42f0-a664-64081ffad9fe)

![14 31](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/362824e9-4dfe-4fd1-89b7-3c80680ea794)

![14 32](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/88d6b980-6b5c-4e65-b180-3d22b83fde90)

**Enter required length of password and skip and then finish**

![14 33](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6cd02dc1-aa17-4838-9586-7ef1130df870)

![14 34](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/17141104-bc1b-497f-a160-e2979f185163)

![14 35](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e8375e69-7c5a-4cd5-a6ea-36f74c96e899)

![14 36](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/08ce6d79-be3f-45bf-bf78-e7f758eb4423)

![14 37](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c1bc7427-aa07-4da5-af80-00d96f733e3c)

![14 38](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/593b5089-2a53-47da-a018-18df0ae926cf)

4. **In Jenkins UI configure Artifactory**

From Dashboard>>Manage Jenkins>>System, scroll down to locate JFROG

![14 39](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/49a383fc-3bfe-4695-8639-a3debc9a1873)

![14 40](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/36f42818-1250-47a3-b4a2-8385de9a0520)

## Phase 2 – Integrate Artifactory repository with Jenkins

1. Create a dummy **Jenkinsfile** in the repository

2. Using Blue Ocean, create a multibranch Jenkins pipeline

3. On the database server, create database and user
   
```
Create database homestead;
CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';
GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';
```
![14 41](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/20aac938-588a-48a5-8522-f14d2c278f29)

**Update playbooks/site.yml and push to the repo**

![14 42](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a9763706-7b06-4bc3-9d5e-7dd1ccb5916b)

![14 43](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f64d797f-4106-43ad-8f82-47724ff3468e)

![14 44](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a837973d-c5a6-4ada-bd0e-b33ae0a9e9dc)

![14 45](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ac3f4b9e-083b-4110-a261-7acbe94506de)

Verify that database homestead is created by ssh into db server

![14 46](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e4d26458-42d2-4f48-b15a-063e361a8777)

4. Update the database connectivity requirements in the file **.env.sample**

5. Update **Jenkinsfile** with proper pipeline configuration

```
pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/darey-devops/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}
```
Create jenkinsfile in php-todo folder

![14 47](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b8c085d8-26a1-4de9-9525-427cbc727113)

Add window and switch to php.todo folder

![14 48](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f2490232-6478-49d3-9461-259fa82eed5e)

Create a new Pipeline

![14 49](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a37c13e9-528f-429a-85e0-3c9b63ae5c4d)

![14 50](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3b019ac8-7185-46ac-a2ce-f98f74560b9b)

![14 51](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/75140e10-43ff-49ae-aa86-5cff8ccb53df)

To resolve the above, remember Jenkins is like our client server:

•	Install mysql: sudo yum install mysql -y

•	ssh into db server and set bind address: sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

![14 52](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/32841766-dd3b-49eb-bafe-4cf0ead7154f)

```
sudo systemctl restart mysql
sudo systemctl status mysql
```
![14 53](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/db0ef136-781d-4e0a-87d6-7d76db44aff5)

Notice the Prepare Dependencies section
•	The required file by PHP is .env so we are renaming .env.sample to .env

![14 54](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a2f18cb9-2d78-42a7-9b72-9c87a1655a57)

```
mysql -h 10.0.16.15 -u homestead -p
```

```
SELECT User FROM mysql.user;
```
![14 55](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4a4642cd-9bb6-4de4-872d-3d81b1f688eb)

**Now push the changes:**

```
git add .
git commit -m “commit message”
git push
```

![14 56](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/32c8153e-50c0-48c8-909e-fb28ec4be01d)

**Notice the Prepare Dependencies section**

• The required file by PHP is .env so we are renaming .env.sample to .env

• Composer is used by PHP to install all the dependent libraries used by the application

• php artisan uses the .env file to setup the required database objects – (After successful run of this step, login to the database, run **show tables** and you will see the tables being created for you)

1.	Update the **Jenkinsfile** to include Unit tests step

```
    stage('Execute Unit Tests') {
      steps {
             sh './vendor/bin/phpunit'
      } 
```

![14 57](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5db18996-45f2-4b9c-93b9-76f33131452c)

Push to github and run the job on JenkinsUI

![14 58](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6cfe7a91-b96b-4c62-94e2-89018814282c)

# Phase 3 – Code Quality Analysis
This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is [phploc](https://phpqa.io/projects/phploc.html). [Read the article here for more](https://matthiasnoback.nl/2019/09/using-phploc-for-quick-code-quality-estimation-part-1/)

The data produced by phploc can be ploted onto graphs in Jenkins.

Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.

```
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}
```

2. Plot the data using **plot** Jenkins plugin.
This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc.

```
stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'

      }
    }
```

![14 59](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/19efcdc1-37b2-48ce-963c-d861e2b15d14)

**Push the chances to github and run on Jenkins UI**

![14 60](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/76f6019c-4873-4795-9ba0-43c9cd9e8ea7)

#### Install phpunit, phploc
=====================================
```
sudo dnf --enablerepo=remi install php-phpunit-phploc -y
wget -O phpunit https://phar.phpunit.de/phpunit-7.phar
chmod +x phpunit
sudo yum  install php-xdebug
```










