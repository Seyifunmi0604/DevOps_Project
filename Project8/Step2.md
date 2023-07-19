## FIXING UMOUNT BLOCKER GETTING DEVICE IS BUSY.

```
sudo umount -f /var/log/httpd/ 
```
![8 15](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/ca0acd20-c455-4f77-bff1-338533fc5445)

**To the current process running. If it says command not found, run 
```
sudo yum install lsof. 
```
## Then rerun:

```
sudo lsof +D /var/log/httpd/ 
```

![8 16](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/05a09a7e-349d-4bea-9816-614fbbacddf6)

# To stop httpd

```
sudo systemctl stop httpd 
sudo umount -f /var/log/httpd
```

## Verify if umount command is successful;

```
mount | grep /var/log/httpd
```
```
sudo systemctl start httpd
sudo systemctl status httpd
```
