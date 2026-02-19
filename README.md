## What We Did (Simple Overview)

In this setup, we created a small local server system to understand how web requests work.

- We installed **Nginx** on our local machine.
- We created a simple backend application using Python.
- Nginx was configured to receive requests from the browser.
- Nginx forwarded those requests to the backend application.
- The backend processed the request and sent a response.
- Nginx returned that response back to the user.

This setup shows how a real server works using a reverse proxy.

### Request Flow

Browser  
→ Nginx Server  
→ Backend Application  
→ Nginx  
→ Browser

### Purpose

The goal of this setup was to understand:
- how a server receives requests
- how requests are forwarded internally
- how responses are returned to users

This is the same concept used in real production systems.


---

````markdown
# Nginx Reverse Proxy Setup (Local Machine)

This project demonstrates how a server works by:
- receiving a request from a browser
- forwarding the request to a backend application
- returning the response back to the user

The setup is done **locally** on a Linux machine using **Nginx** and a **Python backend app**.

---

## Architecture (High Level)

Browser  
→ Nginx Server (port 80)  
→ Backend Application (port 3000)  
→ Nginx  
→ Browser

---

## Prerequisites

- Linux system (Ubuntu)
- Internet connection
- Terminal access
- sudo privileges

---

## Step 1: Install Nginx

Update the system:
```bash
sudo apt update
````

Install Nginx:

```bash
sudo apt install nginx -y
```

Verify installation:

```bash
nginx -v
```

Check Nginx directories:

```bash
ls /etc/nginx
```

You should see:

```
nginx.conf
sites-available
sites-enabled
```

---

## Step 2: Create a Backend Application

Create a backend directory:

```bash
mkdir backend
cd backend
```

Create a Python backend file:

```bash
nano app.py
```

Paste the following code:

```python
from http.server import BaseHTTPRequestHandler, HTTPServer

class MyHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type", "text/plain")
        self.end_headers()
        self.wfile.write(b"Hello user! This response came from BACKEND app")

server = HTTPServer(("localhost", 3000), MyHandler)
print("Backend running on port 3000")
server.serve_forever()
```

Run the backend:

```bash
python3 app.py
```

Test backend directly in browser:

```
http://localhost:3000
```

---

## Step 3: Configure Nginx as Reverse Proxy

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/backend-demo
```

Add this configuration:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

Save and exit:

* CTRL + O
* ENTER
* CTRL + X

---

## Step 4: Enable the Nginx Configuration

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/backend-demo /etc/nginx/sites-enabled/
```

Remove default site:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Test configuration:

```bash
sudo nginx -t
```

Reload Nginx:

```bash
sudo systemctl reload nginx
```

---

## Step 5: Final Test

Ensure backend is running:

```bash
python3 app.py
```

Open browser:

```
http://localhost
```

Expected output:

```
Hello user! This response came from BACKEND app
```

---

## What This Setup Demonstrates

* How a web server receives requests
* How Nginx forwards requests to a backend
* How responses are returned to the user
* Real-world reverse proxy behavior

---

## Key Learning Points

* Nginx listens on port 80
* Backend runs on port 3000
* Nginx acts as a middle layer
* Backend is hidden from the user

---

## Conclusion

This setup simulates a real production-style request flow using Nginx and a backend service, all running locally for learning and practice.





# Writing a Docker image involves creating a plain text file called a Dockerfile which contains instructions for the Docker engine to assemble the image. [1, 2]  
Step-by-Step Guide 

1. Install Docker: Ensure you have Docker installed on your machine. You can download Docker Desktop for your operating system. 
2. Create a Project Directory: Make a new folder for your application and navigate into it using your terminal. 
3. Create a Dockerfile: Inside the directory, create a new text file named  (with no file extension). 
4. Write the Dockerfile Instructions: Define the steps needed to set up your application's environment. A basic structure includes: 
5. Create a  file (Optional but Recommended): In the same directory, create a file named  to exclude unnecessary files (like local development files or git repositories) from the build context, which helps keep the image small and speeds up builds. 
6. Build the Docker Image: Open your terminal, navigate to your project directory, and run the  command. The  flag tags the image with a human-readable name and optional tag (e.g., ), and the  at the end specifies the current directory as the build context. 
7. Run and Test the Image: Start a container from your new image to ensure it works correctly. The  flag maps a host port to the container's port, and  runs it in detached mode. 
8. Push to a Registry (Optional): To share your image, you can push it to a container registry like Docker Hub using the  command after logging in via the CLI.
Perfect 👍
Here is a **clean, simple, copy-paste-ready README.md** **only for the Jenkins part**, written in **easy language** and giving the **full picture**.

You can paste this **as it is** into a `README.md` file.

---

```markdown





We installed Jenkins locally on a Linux machine, completed the initial setup through the web interface, created an admin user, and ran a basic job to confirm Jenkins can automate tasks.
Basic Jenkins job that runs simple shell commands like printing a message, showing the current date, and displaying the user running the job, to verify that Jenkins automation is working
# Jenkins Local Setup and Basic Usage

This document explains how Jenkins was set up and used locally for learning automation.

---

## Overview

In this setup, Jenkins was installed on a local Linux machine to understand how automation works. Jenkins was configured, accessed through a web browser, and used to run a simple job.

This setup does not include any web server, backend application, or cloud service. It focuses only on Jenkins.

---

## What is Jenkins

Jenkins is an automation tool used to run tasks automatically instead of doing them manually. It is commonly used for building, testing, and deploying applications.

---

## Steps Performed

### 1. Installed Java
Jenkins requires Java to run. Java was installed and verified before installing Jenkins.

---

### 2. Installed Jenkins
Jenkins was installed using the official Jenkins repository. After installation, the Jenkins service was started and enabled so that it runs automatically.

---

### 3. Accessed Jenkins Web Interface
Jenkins was accessed locally through a web browser using:

```

[http://localhost:8080](http://localhost:8080)

```

The initial admin password was used to unlock Jenkins.

---

### 4. Completed Initial Jenkins Setup
- Suggested plugins were installed
- An admin user was created
- Jenkins instance configuration was completed

After this step, the Jenkins dashboard became available.

---

### 5. Created First Jenkins Job
A simple freestyle Jenkins job was created to verify that Jenkins works correctly.

The job executed basic shell commands such as:
- printing a message
- showing the current date
- displaying the user running the job

---

### 6. Ran the Job Successfully
The job was executed using the **Build Now** option, and the output was verified in the Jenkins console output.

This confirmed that Jenkins is running correctly and can automate tasks.

---

## What This Setup Demonstrates

- Jenkins can run locally on a machine
- Jenkins provides a web-based interface
- Jenkins can execute automated jobs
- Jenkins is used by developers and DevOps engineers, not end users

---

## Conclusion

Jenkins was successfully set up on a local machine, configured for first-time use, and tested by running a basic automation job. This setup forms the foundation for learning CI/CD pipelines and DevOps automation.
```

---

### One-line takeaway (easy to remember)

> Jenkins was installed locally, configured through the web UI, and used to run a basic automation job.

If you want, next I can:

* shorten this for **college submission**
* rewrite it for **interview explanation**
* extend it with **Jenkins pipeline**
* combine it with **Git or Docker**






