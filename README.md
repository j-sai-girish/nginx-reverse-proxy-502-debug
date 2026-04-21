
# NGINX Reverse Proxy + 502 Bad Gateway Debugging

## 📌 Project Overview
This project demonstrates how NGINX acts as a reverse proxy for a Flask backend and how backend failures result in a 502 Bad Gateway error. It also covers step-by-step troubleshooting and resolution.

---

## 🧠 Architecture
Client → NGINX (Port 80) → Flask Backend (Port 5000)

---

## ⚙️ Technologies Used
- AWS EC2
- NGINX
- Python (Flask)

---

## 🚀 Setup Steps

### 1. Launch EC2 Instance
- Create and start an EC2 instance
- Connect using SSH (EC2 Instance Connect)

### 2. Install Required Packages
```bash
sudo dnf install nginx -y
sudo dnf install python3 -y
pip3 install flask
````

### 3. Create Flask Backend

```bash
nano app.py
```

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Backend"

app.run(host="0.0.0.0", port=5000)
```

### 4. Start Backend

```bash
python3 app.py
```

---

## 🔧 Configure Reverse Proxy

Edit NGINX config:

```bash
sudo nano /etc/nginx/nginx.conf
```

Inside `server {}` block:

```nginx
location / {
    proxy_pass http://127.0.0.1:5000;
}
```

---

### 5. Allow HTTP Traffic

* Go to EC2 Security Group
* Add inbound rule:

  * Type: HTTP
  * Port: 80
  * Source: 0.0.0.0/0

---

### 6. Restart NGINX

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

## ✅ Working State

* Open browser:

```
http://<public-ip>
```

* Output:

```
Hello from Backend
```

---

## ❌ Failure Simulation (502 Bad Gateway)

### Step:

Stop backend:

```bash
CTRL + C
```

### Result:

* NGINX cannot reach backend
* Browser shows:

```
502 Bad Gateway
```

---

## 🔍 Root Cause Analysis

Check running processes:

```bash
ps aux | grep app.py
```

### Observation:

* No Flask process running
* Backend is down

---

## 🛠️ Fix

Restart backend:

```bash
python3 app.py
```

### Result:

* Application works again

---

## 📸 Screenshots

1. Backend Running (Flask on port 5000)
2. NGINX Configuration (proxy_pass)
3. Working Output (Hello from Backend)
4. 502 Error Page
5. Backend Not Running (ps command)
6. Security Group (Port 80 open)

---

## 🎯 Key Learnings

* Reverse proxy concept
* Difference between frontend and backend
* Importance of backend availability
* Understanding 502 Bad Gateway error
* Layered debugging approach:

  * Network (Security Group)
  * Server (NGINX)
  * Application (Flask)

---

## 💡 Conclusion

This project simulates a real-world scenario where a web server is running but the backend service is down. It demonstrates how to identify, debug, and resolve such issues effectively.

---

## 🔗 Author

Built as part of hands-on cloud and DevOps learning journey.

```

---

## 👍 One quick improvement (don’t skip)

After pasting:
- Add your **GitHub repo link in LinkedIn post**
- Make sure screenshots are inside `/screenshots` folder and match names

---

If you want next:
👉 say **“ready for project 5”**  
We’ll move into **S3 + 403 Forbidden (IAM + policies)** — deeper and more interview-relevant.
```
