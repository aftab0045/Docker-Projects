#  Containerizing WordPress on AWS EC2 using Docker

##  Project Overview
This project demonstrates how to deploy a WordPress application using Docker on an AWS EC2 instance.  
It includes:
-  MySQL container (database)
-  WordPress container (application)
-  Docker network for communication

---

##  Architecture
![](./img/Architecture%20Dig.png)

```
User (Browser)
↓
EC2 Instance (Docker Installed)
↓
WordPress Container (Port 8080)
↓
Docker Network (wp-network)
↓
MySQL Container
```

---

##  Step 1: Launch EC2 Instance

Go to AWS Console → EC2 → Launch Instance

###  Configuration:
- AMI: Amazon Linux 2 / Ubuntu
- Instance Type: t2.micro (Free Tier)
- Key Pair: Create new or use existing
- Security Group:
  - Allow **SSH (22)**
  - Allow **HTTP (80)**
  - Allow **Custom TCP (8080)**

- EC2 instance configuration page
- Security group rules

![](./img/Screenshot%202026-04-22%20114758.png)

---

##  Step 2: Connect to EC2 (SSH)

```bash
ssh -i your-key.pem ec2-user@your-public-ip
```

##  Step 3: Install Docker

###  For Amazon Linux:
```bash
sudo yum update -y
sudo yum install docker -y
```
![](./img/Screenshot%202026-04-22%20115127.png)

### Start Docker:
```
sudo systemctl start docker
sudo systemctl enable docker
```

![](./img/Screenshot%202026-04-22%20115201.png)

## Step 4: Create Docker Network
```
docker network create appnetwork
```

![](./img/Screenshot%202026-04-22%20115632.png)

 docker network ls output

## Step 5: Run MySQL Container
```
docker run -d --name mydb \
--network appnetwork \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=wordpressdb \
mysql
```
![](./img/Screenshot%202026-04-22%20120528.png)

docker ps showing MySQL container

## Step 6: Run WordPress Container
```
docker run -d -p 8080:80 --name mywordpress \
--network appnetwork \
-e WORDPRESS_DB_HOST=mydb \
-e WORDPRESS_DB_USER=root \
-e WORDPRESS_DB_PASSWORD=root \
-e WORDPRESS_DB_NAME=wordpressdb \
wordpress
```
![](./img/Screenshot%202026-04-22%20121010.png)

docker ps showing both containers

## Step 7: Access WordPress

Open browser:
```
http://<EC2-Public-IP>:8080
```
![](./img/Screenshot%202026-04-22%20121251.png)

---

## Useful Docker Commands
### List containers:
```
docker ps
```
### Stop containers:
```
docker stop mywordpress mydb
```
### Start containers:
```
docker start mywordpress mydb
```
### Remove containers:
```
docker rm mywordpress mydb
```

## Conclusion

This project demonstrates how to deploy a multi-container application on a cloud server using Docker, improving portability and scalability.