# DEVOPS TOOLING WEBSITE SOLUTION

## TASK

•	DevOps tooling website solution

•	Step 1 – prepare nfs server

•	Step 2 — [configure the database server](https://www.darey.io/docs/step-2-configure-the-database-server/)

In previous [Project 6](https://www.darey.io/docs/project-6-step-1/) you implemented a WordPress-based solution that is ready to be filled with content and can be used as a full-fledged website or blog. Moving further we will add some more value to our solutions that your DevOps team could utilize. We want to introduce a set of DevOps tools that will help our team in day-to-day activities in managing, developing, testing, deploying, and monitoring different projects.

The tools we want our team to be able to use are well-known and widely used by multiple DevOps teams, so we will introduce a single DevOps Tooling Solution that will consist of:

1.	[Jenkins](https://www.jenkins.io/) – free and open source automation server used to build [CI/CD](https://en.wikipedia.org/wiki/CI/CD) pipelines.
2.	[Kubernetes](https://kubernetes.io/) – an open-source container-orchestration system for automating computer application deployment, scaling, and management.
3.	[Jfrog Artifactory](https://jfrog.com/artifactory/) – Universal Repository Manager supporting all major packaging formats, build tools, and CI servers. Artifactory.
4.	[Rancher](https://www.rancher.com/products/rancher) – an open-source software platform that enables organizations to run and manage [Docker](https://en.wikipedia.org/wiki/Docker_(software)) and Kubernetes in production.
5.	[Grafana](https://grafana.com/) – a multi-platform open-source analytics and interactive visualization web application.
6.	[Prometheus](https://prometheus.io/) – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.
7.	[Kibana](https://www.elastic.co/kibana/) – Kibana is a free and open user interface that lets you visualize your [Elasticsearch](https://www.elastic.co/elasticsearch/) data and navigate the [Elastic Stack](https://www.elastic.co/elastic-stack/).

Note: Do not feel overwhelmed by all the tools and technologies listed above, we will gradually get ourselves familiar with them in upcoming projects!
Side Self Study

Read about [Network-attached storage (NAS)](https://en.wikipedia.org/wiki/Network-attached_storage), [Storage Area Network (SAN)](https://en.wikipedia.org/wiki/Storage_area_network) and related protocols like NFS, (s)FTP, SMB, iSCSI. Explore what [Block-level storage](https://en.wikipedia.org/wiki/Block-level_storage) is and how it is used by Cloud Service providers, and know the difference from [Object storage](https://en.wikipedia.org/wiki/Object_storage).
On the [example of AWS services](https://dzone.com/articles/confused-by-aws-storage-options-s3-ebs-amp-efs-explained) understand the difference between Block Storage, Object Storage, and Network File System.
Setup and technologies used in Project 7
As a member of a DevOps team, you will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.
In this project, you will implement a solution that consists of the following components:
1.	Infrastructure: AWS
2.	Webserver Linux: Red Hat Enterprise Linux 8
3.	Database Server: Ubuntu 20.04 + MySQL
4.	Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5.	Programming Language: PHP
6.	Code Repository: [GitHub](https://github.com/darey-io/tooling)
For Rhel 8 server use this ami **ami-07b1305cd78401892

On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using [Network File Sytem (NFS)](https://en.wikipedia.org/wiki/Network_File_System) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.

![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/20f62d5a-dbd4-4c37-bc36-8e8ee4b24765)

