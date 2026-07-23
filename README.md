# DevOps Project 8: Monitoring Applications Using Prometheus and Grafana

## Project Overview

This project demonstrates how to monitor a Linux server using **Prometheus**, **Node Exporter**, and **Grafana**. Prometheus collects system metrics from Node Exporter, and Grafana visualizes these metrics through interactive dashboards.

This project is suitable for beginners learning DevOps monitoring and observability.

---

## Project Architecture

```
                +----------------------+
                |   Ubuntu EC2 Server  |
                +----------+-----------+
                           |
                    Node Exporter
                      Port: 9100
                           |
                           |
                     Prometheus
                      Port: 9090
                           |
                           |
                       Grafana
                      Port: 3000
                           |
                 Monitoring Dashboard
```

---

## Technologies Used

- Ubuntu 24.04
- Prometheus
- Node Exporter
- Grafana
- Linux
- AWS EC2
- Git
- GitHub

---

## Project Objectives

- Install Prometheus
- Configure Prometheus scraping
- Install Node Exporter
- Install Grafana
- Add Prometheus as a Grafana data source
- Build monitoring dashboards
- Visualize Linux system metrics

---

## Prerequisites

- Ubuntu 24.04 Server
- AWS EC2 Instance
- SSH Access
- Internet Connection

---

## AWS Security Group

| Port | Purpose |
|-------|----------|
|22|SSH|
|3000|Grafana|
|9090|Prometheus|
|9100|Node Exporter|

---

# Installation Steps

## Step 1

Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 2

Install Prometheus

Download

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.5.0/prometheus-3.5.0.linux-amd64.tar.gz
```

Extract

```bash
tar -xvf prometheus-3.5.0.linux-amd64.tar.gz
```

Move

```bash
sudo mv prometheus-3.5.0.linux-amd64 /opt/prometheus
```

Create Prometheus user

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

Change ownership
```
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create Prometheus Service
```
sudo nano /etc/systemd/system/prometheus.service
```
Paste
```
[Unit]
Description=Prometheus Monitoring
After=network.target

[Service]
User=prometheus
Group=prometheus
Type=simple

ExecStart=/opt/prometheus/prometheus \
--config.file=/opt/prometheus/prometheus.yml \
--storage.tsdb.path=/opt/prometheus/data

Restart=always

[Install]
WantedBy=multi-user.target
```
save


Create data directory
```
sudo mkdir /opt/prometheus/data
```
Ownership
```
sudo chown -R prometheus:prometheus /opt/prometheus/data
```
Reload
```
sudo systemctl daemon-reload
```
Enable
```
sudo systemctl enable prometheus
```
Start
```
sudo systemctl start prometheus
```
Check
```
sudo systemctl status prometheus
```
Open browser
```
http://YOUR_PUBLIC_IP:9090
```
---

## Step 3

Install Node Exporter

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
```

---

## Step 4

Install Grafana

```bash
sudo apt install grafana -y
```

---

## Step 5

Configure Prometheus

Edit

```
prometheus.yml
```

Add

```yaml
scrape_configs:

- job_name: node_exporter

  static_configs:

  - targets:

    - localhost:9100
```

Restart

```bash
sudo systemctl restart prometheus
```

---

## Step 6

Add Prometheus Data Source

Grafana

```
Connections
↓

Data Sources
↓

Prometheus
```

URL

```
http://localhost:9090
```

Save & Test

---

## Step 7

Create Dashboard

Add panels

- CPU Usage
- Memory Usage
- Disk Usage
- Network Receive
- Network Transmit

---

## PromQL Queries

### CPU

```promql
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

### Memory %

```promql
100*(1-node_memory_MemAvailable_bytes/node_memory_MemTotal_bytes)
```

### Disk %

```promql
100-(node_filesystem_avail_bytes{mountpoint="/"}*100/node_filesystem_size_bytes{mountpoint="/"})
```

### Network Receive

```promql
rate(node_network_receive_bytes_total[5m])
```

### Network Transmit

```promql
rate(node_network_transmit_bytes_total[5m])
```

---

## Project Structure

```
monitoring-project/
│
├── README.md
├── prometheus.yml
├── screenshots/
│   ├── prometheus-home.png
│   ├── prometheus-targets.png
│   ├── grafana-login.png
│   ├── grafana-dashboard.png
│   ├── datasource.png
│   └── cpu-test.png
│
└── queries.md
```

---

## Screenshots

### Prometheus Home

<img width="2880" height="1704" alt="Screenshot 2026-07-23 191657" src="https://github.com/user-attachments/assets/a1698de8-b564-43c6-bf8d-fd01ad329454" />


---

### Prometheus Targets

<img width="2880" height="1704" alt="Screenshot 2026-07-23 191632" src="https://github.com/user-attachments/assets/f74fb80a-551a-415c-88e9-5e55d0f3fb75" />


---

### Grafana Login

<img width="2720" height="1418" alt="Screenshot 2026-07-23 195701" src="https://github.com/user-attachments/assets/1f1827c9-ae32-4426-8fb4-e996629ec43c" />


---

### Data Source

<img width="2880" height="1166" alt="Screenshot 2026-07-23 195922" src="https://github.com/user-attachments/assets/be0d081f-4554-446e-8139-f6025342cd3e" />


---

### Dashboard

<img width="2880" height="1704" alt="Screenshot 2026-07-23 191617" src="https://github.com/user-attachments/assets/7b8ca778-95c1-4c6f-956b-d70e52913393" />


---

### CPU Stress Test

<img width="2406" height="272" alt="Screenshot 2026-07-23 200826" src="https://github.com/user-attachments/assets/e2b7d7d5-1fe2-4bd2-8a38-429579c79619" />


---

## Expected Output

- Prometheus Running
- Node Exporter Running
- Grafana Running
- Prometheus Targets UP
- Interactive Grafana Dashboard
- Real-time CPU Monitoring
- Memory Monitoring
- Disk Monitoring
- Network Monitoring

---

## Future Improvements

- Alertmanager Integration
- Email Alerts
- Slack Notifications
- Docker Monitoring
- Kubernetes Monitoring
- Blackbox Exporter
- Loki Log Monitoring

---

## Author

Jeny

DevOps Engineer

GitHub:
https://github.com/YOUR_USERNAME

LinkedIn:
https://linkedin.com/in/YOUR_PROFILE

---

## License

This project is for learning and educational purposes.
