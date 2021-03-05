# Objectives
We are going to create a Database (MySQL) in lab envirunment. Later, we will deploy container app to access this MySQL Database and show case how application access resouces without hardcode password

### Create World DB
Login Docker (If you don't have Docker Hub account, please signup in https://hub.docker.com
sudo docker login


Create a MySQL container and run it in your jump host server

- Login to you Jump Host
```bash
cd ~
mkdir db && cd db
wget https://downloads.mysql.com/docs/world.sql.gz
gunzip world.sql 
cd ..
sudo docker run --name mysqldb -v /home/ec2-user/db:/docker-entrypoint-initdb.d \
     -e MYSQL_ROOT_PASSWORD=Cyberark1 \
     -e MYSQL_DATABASE=world \
     -e MYSQL_USER=cityapp \
     -e MYSQL_PASSWORD=Cyberark1 \
     -p "3306:3306" -d mysql:5.7.29
```

Check if your World mysql contrainer running correctly
```bash
sudo docker ps
```
- Example output
- 76e3a316a7ee        mysql:5.7.29        "docker-entrypoint.s…"   9 seconds ago       Up 6 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp   mysqldb 

Check if your World Database is running correctly
```bash
sudo docker exec -it mysqldb bash 
mysql -u cityapp -p 
use world; 
show tables; 
```
- Example output
```bash
mysql> show tables;
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
```

