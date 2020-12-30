# Perparation Tasks

## Setup AWS EKS Envirunment
## Install Jump Host. (For testing setup, we will setup jump host, CyberArk DAP Master and Testing MySQL DB in this Jump Host)



### Create World DB
Login Docker (If you don't have Docker Hub account, please signup in https://hub.docker.com
sudo docker login


Create a MySQL container as our database server
sudo docker pull mysql

```bash
mkdir db && cd db
wget https://downloads.mysql.com/docs/world.sql.gz
gunzip world.sql 
cd ..
docker run --name mysqldb -v /home/ec2-user/db:/docker-entrypoint-initdb.d \
     -e MYSQL_ROOT_PASSWORD=Cyberark1 \
     -e MYSQL_DATABASE=world \
     -e MYSQL_USER=cityapp \
     -e MYSQL_PASSWORD=Cyberark1 \
     -p "3306:3306" -d mysql:5.7.29
