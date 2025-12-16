# Monitoring & Observability Project 

This is a **basic DevOps beginner project** that shows how to monitor a simple Node.js application using **Prometheus** and **Grafana** on an **AWS EC2 Ubuntu instance**.

Anyone follow the steps below and run this project.

---

## 1. Prerequisites

* AWS EC2 instance (t3.micro – Free Tier)
* Ubuntu 20.04 / 22.04
* Security Group ports open:

  * 22 (SSH)
  * 3001 (Node App)
  * 9090 (Prometheus)
  * 3000 (Grafana)

---

## 2. Install Required Tools

### Update system

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Git, curl

```bash
sudo apt install -y git curl wget
```

### Install Node.js (LTS)

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify:

```bash
node -v
npm -v
```

---

## 3. Clone Project from GitHub

```bash
git clone https://github.com/mohit7821/monitoring-observability-project.git
cd monitoring-observability-project
```

---

## 4. Run Node.js Application

### Install dependencies

```bash
npm install
```

### Start application

```bash
node server.js
```

App will run on port **3001**.

Check:

```bash
curl http://localhost:3001/health
curl http://localhost:3001/metrics
```

---

## 5. Run App Using PM2 (Recommended)

```bash
sudo npm install -g pm2
pm2 start server.js --name monitoring-app
pm2 save
```

Check status:

```bash
pm2 status
```

---

## 6. Install Prometheus

```bash
cd ~
wget https://github.com/prometheus/prometheus/releases/download/v2.53.1/prometheus-2.53.1.linux-amd64.tar.gz
tar -xvf prometheus-2.53.1.linux-amd64.tar.gz
mv prometheus-2.53.1.linux-amd64 prometheus
cd prometheus
```

### Prometheus configuration

Create config file:

```bash
nano prometheus.yml
```

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "monitoring-app"
    static_configs:
      - targets: ["localhost:3001"]
```

### Start Prometheus

```bash
./prometheus --config.file=prometheus.yml
```

Access Prometheus UI:

```
http://<EC2_PUBLIC_IP>:9090
```

---

## 7. Install Grafana

```bash
sudo apt install -y apt-transport-https software-properties-common
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt update
sudo apt install grafana -y
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Access Grafana:

```
http://<EC2_PUBLIC_IP>:3000
```

Default login:

```
Username: admin
Password: admin
```

---

## 8. Add Prometheus in Grafana

* Go to **Settings → Data Sources**
* Add **Prometheus**
* URL: `http://localhost:9090`
* Click **Save & Test**

---

## 9. Application Endpoints

| Endpoint   | Description        |
| ---------- | ------------------ |
| `/`        | Main app           |
| `/health`  | Health check       |
| `/ready`   | Readiness check    |
| `/metrics` | Prometheus metrics |

---

## 10. What You Learn From This Project

* How monitoring works in real systems
* Prometheus metrics scraping
* Grafana dashboards
* Basic observability concepts
* AWS EC2 + Linux hands-on

---

## 11. Future Improvements

* Dockerize application
* Add Alertmanager
* Use Terraform for infrastructure

---

## Author

DevOps Intern 

