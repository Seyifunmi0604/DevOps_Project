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




























