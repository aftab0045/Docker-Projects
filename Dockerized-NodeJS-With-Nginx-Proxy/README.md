# Node.js + Nginx Reverse Proxy Deployment using Docker on AWS EC2

## Project Overview

This project demonstrates how to deploy a **Node.js application** using **Docker** on an AWS EC2 instance and expose it to users through an **Nginx reverse proxy container**.

The setup follows a **production-style architecture** where:

* Node.js runs as a backend service inside a container
* Nginx acts as a reverse proxy to handle incoming traffic
* Docker networking is used for container communication
* Images are pushed to Docker Hub for reuse and deployment

---

## Architecture

```
User → Nginx Container → Node.js Container
```

![](./img/Screenshot%202026-04-23%20142532.png)
---

## 🛠️ Tech Stack

* Node.js
* Docker
* Nginx
* AWS EC2 (Amazon Linux)
* Git & GitHub
* Docker Hub

---

## Project Structure

```
nodejs-nginx-docker-ec2-deployment/
│
├── node-app/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
│
├── nginx/
│   ├── default.conf
│   └── Dockerfile
│
└── README.md
```

---

## Step-by-Step Setup

###  Step 1: Launch EC2 Instance

* Launch an EC2 instance (Amazon Linux / Ubuntu)
* Configure Security Group:

  * Allow **Port 22 (SSH)**
  * Allow **Port 80 (HTTP)**
  * Allow **Port 3000 (Optional for testing)**

###  Step 2: Connect to EC2

```bash
ssh -i your-key.pem ec2-user@your-public-ip
```

Switch to root user:

```bash
sudo -i
```

---

###  Step 3: Install Docker

```bash
yum update -y
yum install docker -y
systemctl start docker
systemctl enable docker
```

Verify installation:

```bash
docker --version
```

---

###  Step 4: Install Git & Clone Repository

```bash
yum install git -y
git clone https://github.com/your-username/node-app.git
cd node-app
```
![](./img/Screenshot%202026-04-23%20133750.png)
---

## Docker Setup

###  Step 5: Create Node.js Dockerfile

```Dockerfile
FROM node:18

WORKDIR /app

COPY ./nodeApp/ .
RUN npm install

EXPOSE 3000

CMD ["node", "app.js"]
```

![](./img/Screenshot%202026-04-23%20133844.png)

---

### 🔹 Step 6: Build Docker Image

```bash
docker build -t node-app .
```
![](./img/Screenshot%202026-04-23%20134039.png)

Check images:

```bash
docker images
```

---

### 🔹 Step 7: Run Node Container

```bash
docker run -d -p 3000:3000 --name node-container node-app
```
![](./img/Screenshot%202026-04-23%20134220.png)

Test:

```bash
curl http://localhost:3000
```

---

## Nginx Reverse Proxy Setup

### 🔹 Step 8: Create Nginx Configuration

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://node-container:3000;
    }
}
```
![](./img/Screenshot%202026-04-23%20134956.png)

---

### 🔹 Step 9: Create Nginx Dockerfile

```Dockerfile
FROM nginx

COPY default.conf /etc/nginx/conf.d/default.conf
```
![](./img/Screenshot%202026-04-23%20134402.png)

---

## Docker Networking

### Step 10: Create Network

```bash
docker network create my-network
```

---

### Step 11: Run Containers in Network

#### Restart Node Container in Network

```bash
docker stop node-container
docker rm node-container

docker run -d \
--name node-container \
--network my-network \
node-app
```

#### Build & Run Nginx Container

```bash
cd nginx

docker build -t nginx-proxy .

docker run -d \
--name nginx-container \
--network my-network \
-p 80:80 \
nginx-proxy
```

---

## Access Application

Open in browser:

```
http://<EC2-PUBLIC-IP>
```


![](./img/Screenshot%202026-04-23%20135204.png)


✅ Your Node.js app will be served via Nginx

---

## Push Images to Docker Hub

### Login

```bash
docker login
```

### 🔹 Tag Images

```bash
docker tag node-app yourusername/node-app:v1
docker tag nginx-proxy yourusername/nginx-proxy:v1
```

### 🔹 Push Images

```bash
docker push yourusername/node-app:v1
docker push yourusername/nginx-proxy:v1
```
![](./img/Screenshot%202026-04-23%20141520.png)



![](./img/Screenshot%202026-04-23%20141340.png)


---

##  Useful Commands

### Check Running Containers

```bash
docker ps
```

### View Logs

```bash
docker logs nginx-container
docker logs node-container
```

### Inspect Network

```bash
docker network inspect my-network
```
---

## Conclusion

This project provides hands-on experience with:

* Docker containerization
* Nginx reverse proxy
* Cloud deployment on AWS

It is a strong addition to your **GitHub portfolio** and **resume** for DevOps and Full Stack roles.

---
