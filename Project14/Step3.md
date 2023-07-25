## ANSIBLE ROLES FOR CI ENVIRONMENT
Now go ahead and Add two more roles to Ansible:

1.	[SonarQube](https://www.sonarsource.com/products/sonarqube/) (Scroll down to the Sonarqube section to see instructions on how to set up and configure SonarQube manually)
	
2.	[Artifactory](https://jfrog.com/artifactory/)
   
**Why do we need SonarQube?**

SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, [code smells](https://en.wikipedia.org/wiki/Code_smell), and security vulnerabilities. [Watch a short description here](https://www.youtube.com/watch?v=vE39Fg8pvZg). There is a lot more hands-on work ahead with SonarQube and Jenkins. So, the purpose of SonarQube will be clearer to you very soon.

**Why do we need Artifactory?**

Artifactory is a product by [JFrog](https://jfrog.com/) that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.
[Watch a short description here](https://www.youtube.com/watch?v=upJS4R6SbgM) Focus more on the first 10.08 mins
Configuring Ansible For Jenkins Deployment
In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI.
To do this,
1.	Navigate to Jenkins URL
2.	Install & Open BlueOcean Jenkins Plugin

![14 11](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/52c4f322-6f30-4475-8a36-7ef1ee0cb237)

![14 12](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/aee3ff15-510c-4444-95bc-74ce59adcf33)

3.	**Create a new pipeline**

![14 13](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/adba5394-2fab-448b-b189-d125524c86cc)

![14 14](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c20494a6-7a39-45d5-9b65-ad1be380ffec)

4.	**Select GitHub**

![14 15](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/018d4e11-446b-4f9f-aa47-c688cf04909a)

5.	**Connect Jenkins with GitHub**

![14 16](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7e4b7e8f-b55f-4a5a-8f9c-0bcefa5112b2)

6. **Login to GitHub & Generate an Access Tokenhttps://www.dareyio.com/wp-content/uploads/2021/07/Jenkins-Create-Access-Token-To-Github.png**

<img width="1057" alt="14 21" src="https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/eeb2e538-2865-4a77-9092-2586c3639bae">

7. **Copy Access Token**

<img width="1185" alt="14 22" src="https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0c54ea88-3ff8-4755-b12a-7a703324ee30">

At this point, you may not have a [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/) in the Ansible repository, so Blue Ocean will attempt to give you some guidance to create one. But we do not need that. We will rather create one ourselves. So, click on Administration to exit the Blue Ocean console.

![14 17](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0d74f8c7-d2a4-4a99-be7b-02df8184143b)

Here is our newly created pipeline. It takes the name of your GitHub repository.

![14 18](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a1add5d1-f244-474b-80e9-a38008b705b5)

**Let us create our Jenkinsfile**

Inside the Ansible project, create a new directory **deploy**, and start a new file **Jenkinsfile** inside the directory.

Add the code snippet below to start building the **Jenkinsfile** gradually. This pipeline currently has just one stage called **Build** and the only thing we are doing is using the **shell script** module to echo **Building Stage**

```
pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}

```
![14 19](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e5fe6851-e0de-41a6-901b-70b8b36ad056)

**Now go back into the Ansible pipeline in Jenkins, and select configure**

![14 20](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/657fb494-42bf-48bd-a0a9-03f3402bb16a)

**Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile**

![14 23](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e6841c46-1eb9-46ea-8125-4b9ca67f1c72)

**Back to the pipeline again, this time click "Build now"**

<img width="810" alt="14 27" src="https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4cd593d9-32ee-47c9-b961-c4274b5183b7">


**NOTE:Commit the changes in vscode and push to github**

1.	**Click on Blue Ocean**
   
2.	**Select your project**

![14 24](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/dce07ec1-016d-48d0-a0cd-73468efba8fe)

![14 25](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/fff2349f-d27f-43fc-9383-51318f17ff00)

3.	**Click on the play button against the branch**

![14 26](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6636328c-6f58-4da7-9fdd-fc865ceb818d)

Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

**Let us see this in action.**

1.	Create a new git branch and name it feature/jenkinspipeline-stages

![14 28](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6dafa3cf-fc18-4c06-90c9-8b1efd1e3b5f)


2.	Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.

```
   pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}

```

![14 29](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/49e36c96-796c-4374-af74-5d23bbad4e8a)

![14 30](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3f8bd5be-21d5-4692-b4b9-6c3f6d907b62)

To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

1.	Click on the "Administration" button

![14 31](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/981852b4-d698-4a0d-a211-411c21528e69)

2.	Navigate to the Ansible project and click on "Scan repository now"

![14 32](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/9a9c029a-58a5-4e65-ae4f-07b2632263a9)

![14 33](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/47155e5a-0aa7-45de-afe0-2640275f9ab1)

![14 34](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3ccca54b-a81f-4381-a6c0-20e5894e87ba)

![14 35](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ada56426-2946-48da-a20d-853659c1fb66)

**A QUICK TASK FOR YOU!**
`
1. Create a pull request to merge the latest code into the main branch
   
2. After merging the PR, go back into your terminal and switch into the main branch.

3. Pull the latest change.
   
4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
   1. Package 
   2. Deploy 
   3. Clean up
5. Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch

6. Eventually, your main branch should have a successful pipeline like this in blue ocean










































