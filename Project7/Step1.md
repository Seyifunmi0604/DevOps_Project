## STEP 1 – PREPARE NFS SERVER
Step 1 – Prepare NFS Server
1.	Spin up a new EC2 instance with RHEL Linux 8 Operating System.
2.	Based on your LVM experience from Project 6, Configure LVM on the Server.

Create 3 EBS Volume and attach to NFS Server
Run: 
```
lsblk
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/31a78376-0739-403c-be11-f2c35002034d)

**Let’s use gdisk utility to create a single partition on each of the 3 disks.**
```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/06a45043-bfb9-46c4-8b5a-08c28e24556b)

![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6167d812-0f46-4ccc-abaa-2cd0ef6b7d0e)

**Partition completed**
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3639c478-8bd4-4ddf-a3e0-13fa70de00a4)

Installing LVM package
```
sudo yum install lvm2 -y
```
Check for available partition
```
sudo lvmdiskscan
```
![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3fb278e6-00b1-4109-85b0-cfed5ec7a9bf)

**Now let’s create Physical volumes to be used by lvm with pvcreate utility**
```
sudo pvcreate /dev/xvdf1 
sudo pvcreate /dev/xvdg1 
sudo pvcreate /dev/xvdh1
```
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e4aa3b19-a01d-4ade-925a-f25421b7323a)

Test if your Physical volumes are there with
```
sudo pvs
```
![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b529bfa1-4a61-4510-b919-124c5ea036fb)

**Create Volume group with vgcreate utility**
```
sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1 
```
to confirm,
```
sudo vgs
```
![Picture1 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/60b88bbd-71a2-4b67-9421-c4d9f74e3534)

**Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs**
```
sudo lvcreate -n lv-apps -L 9G webdata-vg
sudo lvcreate -n lv-logs -L 9G webdata-vg
sudo lvcreate -n lv-opt -L 9G webdata-vg
```
 To verify our logical volume
 ```
 sudo lvs
 ```
 ```
lsblk
```
![Picture10 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/6dcb2af4-644e-49b1-ba55-a7c8e3c3d879)

You can also use the below # to view complete setup -VG, PV and LV:
```
sudo vgdisplay -v
```
![Picture10 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ebf6de1a-6725-4dbd-9496-42c6b255be32)

**Instead of formating the disks as ext4 you will have to format them as xfs**
```
sudo mkfs -t xfs /dev/webdata-vg/lv-apps
sudo mkfs -t xfs /dev/webdata-vg/lv-logs
sudo mkfs -t xfs /dev/webdata-vg/lv-opt
```
![Picture10 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d0b1ddca-af24-4ec4-b10a-c8217eedd367)

4.	**Create mount points on /mnt directory for the logical volumes as follow:**
**Create dir /mnt/apps, /mnt/logs, and /mnt/opt**
```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ba11d92c-cb8d-4e42-ad95-bc60e57233aa)

**Mount lv-apps on /mnt/apps – To be used by webservers**
```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
```
**Mount lv-logs on /mnt/logs – To be used by webserver logs**
```
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
```
**Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8**
```
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```
![Picture12](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e6edd8b1-95ac-489f-9ea2-61fb343e0cde)

**4.	Install NFS server, configure it to start on reboot and make sure it is u and running**
```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d8a2e610-7e39-4f43-908b-0bf1062e380e)

5.	Export the mounts for webservers’ **subnet cidr** to connect as clients. For simplicity, you will install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
To check your **subnet cidr** – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:

![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/89fbd420-37d5-43c2-9cff-ec583f5e4b31)

![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/883ae5d6-04e4-441d-afe5-686df99e60e3)

**Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:**
```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/41c4c914-7067-4321-988a-e11fa15939a3)

**Configure access to NFS for clients within the same subnet (example of Subnet CIDR – 10.0.16.0/20 ):**
```
sudo vi /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0cff8579-84ec-4e0d-bfd5-ad1b0347e900)

Let's export dir specified in the **/etc/exports file** with the below:
To ensure that the directories specified in the NFS export configuration file (/etc/exports) are made accessible to remote NFS clients, and it refreshes the export information to reflect any changes made
```
sudo exportfs -arv
```
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b0e3eed7-74ce-41b0-8716-d3d43d4966ca)

6.	**Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)**
```
rpcinfo -p | grep nfs
```
![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4322c86d-db13-4b91-bca0-5815c230d6b1)

Important note: In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/72910857-d8ad-4dea-a701-c4b0aec49706)



