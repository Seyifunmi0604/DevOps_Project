## Prepare the Database Server
Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of **apps-lv** create **db-lv** and mount it to **/db** directory instead of **/var/www/html/**.
1.	Use [lsblk](https://man7.org/linux/man-pages/man8/lsblk.8.html) command to inspect what block devices are attached to the server
```
lsblk
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/09abd90c-7705-4c17-aa84-9c49d73220e2)

2.	Use [df -h](https://en.wikipedia.org/wiki/Df_(Unix)) command to see all mounts and free space on your server
```
df -h
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/60ef1dc2-d693-44fc-b4e8-6375690f131e)

3.	Use **gdisk** utility to create a single partition on each of the 3 disks
```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/fe45391e-c42a-450e-adc1-d48aa6989191)

4.	Use lsblk utility to view the newly configured partition on each of the 3 disks.
```
lsblk
```
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7d40b8fd-169a-46bb-bfa7-f18a995b1fbd)

5.	Install **lvm2** package using **sudo yum install lvm2 -y**
```
sudo yum install lvm2 -y
```
![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/a75cbbb9-196a-4a19-8c7c-6d206eb2ecd7)

6.	Run **which lvm** to confirm lvm is installed and where it is installed.
```
which lvm
```
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/161344f0-26de-4416-bd7f-5140eb85ccba)

7.	Run sudo lvmdiskscan command to check for available partitions.
```
sudo lvmdiskscan
```
![Picture1 8](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/23daf2fa-9141-4fd4-941e-99ba3afeff6c)

8.	Use **pvcreate** utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```
9.	Verify that your Physical volume has been created successfully by running **sudo pvs**
```
sudo pvs
```
![Picture1 2](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ada0383e-3ddc-4e42-ab8b-48bf95dc687b)

10.	Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the **VG database-vg**
```
sudo vgcreate database-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```
11.	Verify that your VG has been created successfully by running **sudo vgs**
```
sudo vgs
```
![Picture1 3](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5df9b89b-a8d7-442c-80a2-aa339af6fa60)

12.	Use **lvcreate** utility to create 2 logical volumes. **db-lv**
```
sudo lvcreate -n db-lv -L 20G database-vg
```
13.	Verify that your Logical Volume has been created successfully by running **sudo lvs**
```
sudo lvs
sudo lsblk
```
![Picture1 4](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/7f5bd007-941a-4957-b9e3-c59b7f7f9185)

14.	Verify the entire setup.
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```
15.	Use **mkfs.ext4** to format the logical volumes with **ext4** filesystem
```
sudo mkfs -t ext4 /dev/database-vg/db-lv
```
![Picture1 5](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/dfcf3c42-5a92-464a-823a-1410978d82af)

16.	Create **/db** directory to store database files
```
sudo mkdir /db
sudo mount /dev/database-vg/db-lv /db
sudo blkid
```
![Picture1 6](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/5aadd812-299a-497d-91cd-077e854f173d)

**Update /etc/fstab**
```
sudo vi /etc/fstab
```
**UUID=7d5a2bae-5300-4c7e-916e-1aed6ad9024e /db ext4 defaults 0 0**
```
sudo mount -a
sudo systemctl daemon-reload
df -h
```
![Picture1 7](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/37b685ae-9478-4445-9cd8-8e389af600bf)













