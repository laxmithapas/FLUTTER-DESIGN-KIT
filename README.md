

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

```

---

If you want, next I can:
- simplify this further for **college submission**
- convert it into a **DevOps mini-project**
- add **Docker version**
- add **diagram section**

Just tell me 👍
```
