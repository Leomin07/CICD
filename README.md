## CICD tutorial

1. Install SSH
```
sudo apt-get update
```
```
sudo apt install ssh
```
```
sudo systemctl enable --now ssh
```
```
sudo systemctl status ssh
```
```
cd ~/.ssh
```
```
sudo nano authorized_keys (Add ssh key to server)
```
Set server SSH key to git hub.
```
ssh-keygen -t rsa -b 4096 -C "truongezgg@gmail.com"
```
```
cat ~/.ssh/id_rsa.pub
```
```
sudo apt install curl
```
Install node@14
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
```
sudo apt-get install -y nodejs
```
```
sudo npm i -g yarn
```
Swap ram trong linux/ubuntu
Creating a Swap File
```
sudo fallocate -l 2G /swapfile
```
Set up Swap File Permissions
```
sudo chmod 600 /swapfile
```
Set up a Swap Space
```
sudo mkswap /swapfile
```
```
Output
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=f59595fb-754b-47ae-af6b-8dd6e98654d8
```
Enable Swap Space
```
sudo swapon /swapfile
```
Verify that the swap is available by typing:
```
sudo swapon --show
```

```
Output
NAME      TYPE  SIZE USED PRIO
/swapfile file 1024M   0B   -2
```
You can check the output of the free utility again.
```
free -h
```

```
Output
               total        used       free        shared      buff/cache  available
Mem:           581M         275M        62M        103M        243M        110M
Swap:          1.0G          0B        1.0G
```
2. Install Redis
```
sudo apt update
```
```
sudo apt install redis
```
```
sudo systemctl start redis-server
```
```
sudo systemctl status redis
```
3. Install MongoDb
MongoDB 6.0
```
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```
```
sudo apt-get install gnupg
```
```
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y mongodb-org
```
```
sudo systemctl start mongod
```
```
sudo systemctl status mongod
```
4. Install PM2
```
yarn global add pm2
```
```
pm2 start dist/index.js(on dotakyan project)
```
5. Install Nginx.
```
sudo apt install nginx
```
Check firewall server (ubuntu).
- If inactive, skip
```
sudo ufw app list
```
```
Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
```
sudo ufw allow 'Nginx HTTP'
```
```
sudo ufw status
```
Checking web server.
```
systemctl status nginx
```
```
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```
6. Install Mysql
```
sudo apt update
```
```
sudo apt install mysql-server
```
Setup mysql and create new username: dotachan, password: Amela@123a@
```
CREATE USER 'runcrew'@'%' IDENTIFIED WITH mysql_native_password BY 'Amela@123a@';
```
```
GRANT ALL PRIVILEGES ON *.* TO 'runcrew'@'%' WITH GRANT OPTION;
```
```
FLUSH PRIVILEGES;
```
```
CREATE DATABASE runcrew CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
```
mkdir backup
sudo vi backup.sh
mysqldump -u runcrew -p'Amela@123a@' runcrew | gzip>/home/ubuntu/backup/runcrew _`date +"%y%m%d_%H%M%S"`.sql.gz
find /home/ubuntu/backup -mtime +13 -type f -delete
crontab -e
mm hh * * *
```
```
29 11 * * * /bin/bash /home/ubuntu/backup.sh
```
