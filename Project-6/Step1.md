# Step1. Prepare a Web Server

Lunch two(2) Redhat EC2 instances, name them Webserver and Database Server
1.	Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.
Learn How to Add EBS Volume to an EC2 instance [here](https://www.youtube.com/watch?v=HPXnXkBzIHw)

![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/e63bd46f-d2ff-41d7-9e97-a0496ea9bc9d)

![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/fa24763d-9903-4a41-9923-ebfc2816661b)

![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/71d3f75c-7cde-4c78-bf99-414c596d2b09)

2.	Attach all three volumes one by one to your Web Server EC2 instance

![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/b5c2a351-987d-4ab5-8729-37de7dface07)

![Picture1 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a035e127-64c7-4968-8b49-b4297f4ba55a)

3.	Open up the Linux terminal to begin configuration.
4.	Use **lsblk** command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with **ls /dev/** and make sure you see all  newly created block devices there – their names will likely be **xvdf, xvdh, xvdg.
 
![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/fa0848cc-3f91-4a7d-b1d4-9e3cd992afc2)

5.	Use df -h command to see all mounts and free space on your server

![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7a756253-2aa9-422b-92d4-a9a8f18a3376)

Shows no mount is done yet. NOTE: **/dev** means device
U can run 
```
sudo ls /dev
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/15abbcf7-f32c-4bc9-8004-5117656836f3)

5.	Use **gdisk** utility to create a single partition on each of the 3 disks
```
 sudo gdisk /dev/xvdf
 sudo gdisk /dev/xvdg
 sudo gdisk /dev/xvdh
 ```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f99b4b73-a1a6-4eca-9ea2-b51f653cd389)

6.	Use **lsblk** utility to view the newly configured partition on each of the 3 disks.

![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d1e26f44-3244-4ae3-a813-b6ff512d1a9c)

7.	Install [lvm2](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)) package using 
```
sudo yum install lvm2 -y
```
Run which **lvm** to confirm lvm is installed and where it is installed.

![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c1386f9b-379b-4b99-b878-45438a5cedae)

Run **sudo lvmdiskscan** command to check for available partitions.
```
sudo lvmdiskscan
```
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/75d4037b-bd2f-46ed-a44e-ed9f4407cc92)

Note: Previously, in Ubuntu we used **apt** command to install packages, in RedHat/CentOS a different package manager is used, so we shall use yum command instead.

8.	Use **pvcreate** utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```
**or run it at once**
```
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```
![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0ef08400-dad1-4a0d-a12d-417667a20cf4)

9.	Use **vgcreate** utility to add all 3 PVs to a volume group (VG). Name the VG **webdata-vg**
```
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```
![Picture1 9](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/dec60e82-46f5-47ec-9ad4-8b4271ee54cf)

10.	Verify that your VG has been created successfully by running 
```
sudo vgs
```
![Picture2 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/c5038b31-20af-45e8-8e34-a953f9a30017)

11.	Use **lvcreate** utility to create 2 logical volumes. **apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size**. **NOTE:** apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
12.	Verify that your Logical Volume has been created successfully by running 
```
sudo lvs
```
![Picture2 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/70ab6a8d-1713-4ee0-9b03-7a491d57a2a1)

13.	Verify the entire setup.
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```
![Picture2 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/dd80b9bc-9f75-48dd-beb1-aba32e9a2f6e)

![Picture2 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/4aebbe44-d8a6-4e73-994c-9718a6acbf6a)

14.	Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
15.	Create **/var/www/html** directory to store website files
```
sudo mkdir -p /var/www/html
```
16.	Create **/home/recovery/logs** to store backup of log data
```
sudo mkdir -p /home/recovery/logs
```
17.	Mount **/var/www/html** on **apps-lv** logical volume
Note before mounting: to check if there are existing files run;
```
ls -l /var/www/html 
```
![Picture1 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/dfd401af-a783-488e-8f21-7a98ad4eb178)

```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
18.	Use **rsync** utility to backup all the files in the log directory **/var/log** into **/home/recovery/logs** (This is required before mounting the file system)
```
ls -l /var/log 
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/0c6ce0ae-3db6-41b6-a892-f2cd111c77cd)
```
sudo rsync -av /var/log/. /home/recovery/logs/
```

19.	Mount **/var/log** on **logs-lv** logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very
important)
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
20.	Restore log files back into /var/log directory
```
sudo rsync -av /home/recovery/logs/. /var/log
```
double check 
```
sudo ls -l /var/log
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/3e02b9ad-d836-41a4-9bf2-9ba2376cb880)
```
df -h
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/849e3e58-1298-4336-bdbf-4822cd44447a)

21.	Update /etc/fstab file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the **/etc/fstab** file;
```
sudo blkid
```
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/8aba999e-c493-4a65-8888-cfb02af1986a)

## Modify the highlighted as below:
```UUID=a35f4065-df34-458a-9356-18cf0078ece7 /var/www/html ext4 defaults 0 0```
```UUID=e9ad8c34-6965-44d0-ab12-f3c54142de80 /var/log      ext4 defaults 0 0```

![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/1b25704d-9d99-4db1-9cf4-8639fdc31c19)

1.	Test the configuration and reload the daemon
```
sudo mount -a
sudo systemctl daemon-reload
```
2.	Verify your setup by running **df -h**, output must look like this:
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/f1c5e0b6-4c94-4d34-aa1b-9db8a4e6dcef)





