# Docker Infrastructure Project

This project focuses on containerizing applications using **Docker** to create a portable, self-contained environment. The goal is to build a robust infrastructure utilizing a **reverse proxy**, a **load balancer**, **two application servers**, and a **front-end server**.



## 📋 Context

Docker is a platform that allows you to containerize your applications, meaning that you can package them into portable, self-contained environments which can run anywhere. This enables you to quickly move your application from one environment to another without worrying about dependencies or configuration issues.

Docker achieves this using **containers**: isolated environments containing everything an application needs to run (libraries, dependencies, configurations). They are lightweight and fast, making them ideal for modern software development.

## 🏗️ High-level Design

In this design, a single server acts as the entry point for the application. This server functions as:
1.  **Reverse Proxy:** Routes traffic to either the API servers or the front-end static-content server.
2.  **Load Balancer:** Balances traffic between the API servers.

**Flow:**
* **Static Content:** If the request is for the front-end, it is routed to the static-content server. The client does not communicate directly with the static server.
* **API Requests:** If the request is for the API, it goes through a load-balancing algorithm (**Round Robin**) to determine which API server receives the request. The client does not communicate directly with the API servers.

> **What is Round Robin?**
> Round Robin load balancing distributes traffic sequentially across servers (A -> B -> C -> A...). This ensures equal traffic distribution and prevents any single server from becoming overwhelmed.

---

## 🛠️ Prerequisites

Before starting, ensure you have **Docker Desktop** installed on your local machine.

* [Windows Installation](https://docs.docker.com/desktop/install/windows-install/)
* [Mac Installation](https://docs.docker.com/desktop/install/mac-install/)
* [Linux Installation](https://docs.docker.com/desktop/install/linux-install/)

**Useful Resources:**
* [Docker Tutorial](https://www.docker.com/101-tutorial/)
* [Docker Cheatsheet](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
* [Proxy vs Reverse Proxy](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)

---

## 📂 Tasks

### 0. Create Your First Docker Image
Create a Dockerfile based on the latest Ubuntu image that updates and upgrades the installed software.

* **Base Image:** `ubuntu:latest`
* **Commands:** `apt-get update`, `apt-get upgrade -y`
* **Output:** The container should echo "Hello, World!"

**Example Build & Run:**
```bash
docker build -f ./Dockerfile -t softy-pinko:task0 .
docker run -it --rm --name softy-pinko-task0 softy-pinko:task0
# Output: Hello, World!
1. Back-end
Create a Python Flask application running inside a Docker container.

Installations: python3, python3-pip, flask.

Fix for externally managed environments: RUN rm /usr/lib/python*/EXTERNALLY-MANAGED

App: Create api.py that listens on 0.0.0.0:5252 and returns "Hello, World!".

api.py:

Python

from flask import Flask
app = Flask(__name__)

@app.route('/api/hello')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5252)
Run Command:

Bash

docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
2. Front-end
Set up a static front-end using Nginx.

Base Image: nginx:latest

Content: Clone the repository https://github.com/atlas-school/softy-pinko-front-end into task2/front-end.

Configuration: Copy softy-pinko-front-end.conf to /etc/nginx/conf.d/default.conf.

Port: Configure Nginx to listen on port 9000.

Run Command:

Bash

docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task2 softy-pinko-front-end:task2
3. Connecting Front-end and Back-end
Connect your front-end to the back-end to display dynamic data.

Update HTML: Add <h1 id="dynamic-content"></h1> to index.html.

Add JavaScript: Use AJAX to fetch data from http://localhost:5252/api/hello.

Update Back-end: Install flask-cors and update api.py to handle Cross-Origin Resource Sharing.

Updated api.py:

Python

from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

@app.route('/api/hello')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5252)
4. Making it Simpler with Docker Compose
Use Docker Compose to manage multiple containers simultaneously.

Create docker-compose.yml.

Define services: back-end and front-end.

Map ports: 5252:5252 and 9000:9000.

Commands:

Bash

docker-compose build
docker-compose up
5. Proxy Server
Introduce a Proxy Server to sit in front of the static and API servers. Clients will only communicate with the proxy.

Service: proxy (Nginx).

Configuration: proxy.conf copied to /etc/nginx/conf.d/default.conf.

Routing:

/ -> http://front-end:9000

/api -> http://back-end:5252

Update JS: Change AJAX URL to /api/hello (relative path).

Update Docker Compose: Map only the Proxy port (80:80). Remove host port mappings for front-end and back-end (keep them internal).

Run Command:

Bash

docker-compose up
6. Scale Horizontally
Scale the API to handle more traffic by adding a second API server and load balancing between them.

Goal: Run 2 instances of the back-end service.

Algorithm: Nginx will automatically use Round-Robin.

Command: Create a file 2-api-servers.txt containing the command to scale the service.

Example Output: You should see logs indicating traffic being served alternately by task6-back-end-1 and task6-back-end-2.

📋 Repository Information
GitHub repository: holbertonschool-softy-pinko-docker

Directories:

task0: Create First Docker Image

task1: Back-end (Flask)

task2: Front-end (Nginx)

task3: Connecting FE & BE

task4: Docker Compose

task5: Proxy Server

task6: Horizontal Scaling# holbertonschool-softy-pinko-docker
Docker Infrastructure Project
