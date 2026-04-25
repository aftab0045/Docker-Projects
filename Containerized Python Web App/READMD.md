# Containerized Python Web App with Nginx Proxy

This project demonstrates a **complete Docker workflow** for deploying a Python Flask application using containers and a reverse proxy.

---

##  Project Overview

This project includes:

* Python Flask Web Application
*  Docker Image Creation
*  Container Deployment
*  Nginx Reverse Proxy Setup
*  Creating Image from Running Container
*  Pushing Image to Docker Hub

---

##  Architecture

![](./img/Image%20Apr%2024,%202026,%2011_57_59%20AM.png)

```
Client (Browser)
        ↓
   Nginx Container (Port 80)
        ↓
 Python Flask Container (Port 5000)
```

---

##  Project Structure

```
.
├── app.py
├── requirements.txt
├── Dockerfile
```

---

## Step-by-Step Setup

### 1️. Clone Repository

```
git clone <your-repo-url>
cd <project-folder>
```

---

### 2. Create Dockerfile
```
FROM python:3.10-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt


EXPOSE 5000

CMD ["python", "app.py"]
```

![](./img/Screenshot%202026-04-24%20112618.png)

### 2️. Build Docker Image

```
docker build -t pythonimg .
```
![](./img/Screenshot%202026-04-24%20112555.png)

- Check Images

![](./img/Screenshot%202026-04-24%20112747.png)
---

### 3️. Run Python Container

```
docker run -d --name pythonapp pythonimg
```

![](./img/Screenshot%202026-04-24%20113013.png)

---

### 4️. Run Nginx Proxy Container

```
docker run -d \
  --name proxyserver \
  -p 80:80 \
  --link pythonapp:pythonimg \
  nginx
```
![](./img/Screenshot%202026-04-24%20113339.png)

---

### 5️. Verify Running Containers

```
docker ps
```

---

### 6️. Access Application

Open in browser:

```
http://<EC2-PUBLIC-IP>
```
![](./img/Screenshot%202026-04-24%20113510.png)

---

## Create Image from Running Container

```
docker commit pythonapp
```
![](./img/Screenshot%202026-04-24%20113657.png)


---

##  Tag Image for Docker Hub

```
docker tag <image_id> aftabattar/containerized-python-web-app
```
![](./img/Screenshot%202026-04-24%20114557.png)

---

##  Login to Docker Hub

```
docker login
```

---

##  Push Image to Docker Hub

```
docker push aftabattar/containerized-python-web-app
```

![](./img/Screenshot%202026-04-24%20114616.png)

---

##  Useful Docker Commands

### List Images

```
docker images
```

### Remove Image

```
docker rmi <image_id>
```

### Restart Container

```
docker restart <container_id>
```

### Access Container Terminal

```
docker exec -it <container_id> /bin/bash
```

---

##  Issues Faced & Fixes

###  Issue:

* Docker push failed with:

  ```
  denied: requested access to the resource is denied
  ```

###  Fix:

* Correctly tagged image with Docker Hub username:

  ```
  aftabattar/containerized-python-web-app
  ```

---

##  Key Learnings

* Docker image lifecycle (build → run → commit → push)
* Multi-container setup using reverse proxy
* Importance of correct image tagging
* Container networking basics
* Debugging Docker errors

---

##  Conclusion

This project demonstrates a **real-world Docker deployment workflow**, including:

* Application containerization
* Reverse proxy integration
* Image creation and distribution

A solid project for **DevOps, Cloud, and Backend roles** 
---
