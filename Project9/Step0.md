# PROJECT9: CONTINOUS INTEGRATION PIPELINE FOR TOOLING WEBSITE

## TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

## Steps of What to do

**i. Tooling website deployment automation with continuous integration. introduction to jenkins**

**ii. Install and configure jenkins server**

**iii. Configure jenkins to copy files to nfs server via ssh**

In previous Project 8 we introduced horizontal scalability concept, which allow us to add new Web Servers to our Tooling Website and you have successfully deployed a setup with 2 Web Servers and also a Load Balancer to distribute traffic between them. If it is just two or three servers – it is not a big deal to configure them manually. Imagine that you would need to repeat the same task over and over again adding dozens or even hundreds of servers.

DevOps is about Agility and speedy release of software and web solutions. One of the ways to guarantee fast and repeatable deployments is the Automation of routine tasks.
In this project we are going to start automating part of our routine tasks with a free and open-source automation server – [Jenkins](https://en.wikipedia.org/wiki/Jenkins_(software)). It is one of the most popular [CI/CD](https://en.wikipedia.org/wiki/CI/CD) tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project originally had a named "Hudson".

According to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.

In our project, we are going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub **https://github.com/<yourname>/tooling** will be automatically updated to the Tooling Website.

## Side Self Study
Read about [Continuous Integration, Continuous Delivery, and Continuous Deployment]((https://circleci.com/continuous-integration/)).

## Task
Enhance the architecture prepared in Project 8 by adding a Jenkins server, and configuring a job to automatically deploy source code changes from Git to NFS server.
Here is what your updated architecture will look like upon completion of this project:

![9 0](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/8e01779d-2dc3-4323-a7bf-9989a5c0cc32)

