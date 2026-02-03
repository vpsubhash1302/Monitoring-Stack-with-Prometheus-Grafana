# Monitoring-Stack-with-Prometheus-Grafana


Objective

Set up a local monitoring stack using **Prometheus** and **Grafana** to monitor a **containerized Python application** exposing custom metrics via a `/metrics` endpoint.

This project demonstrates an end-to-end monitoring flow commonly used in real-world production systems.

---

Architecture

```
User Request
     ↓
Sample App (Flask + Prometheus Client)
     ↓  /metrics
Prometheus (Scraping Metrics)
     ↓
Grafana (Visualization)
```

All services run as Docker containers and communicate via Docker’s internal network.

---

Project Structure

```
Assessment-6/
│
├── docker-compose.yml
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── prometheus/
    └── prometheus.yml
```

---

Prerequisites

* Docker (v20+)
* Docker Compose (v2+)
* Browser

Verify installation:

```bash
docker --version
docker compose version
```

---

Setup Instructions

Clone the Repository

```bash
git clone <your-repo-url>
cd Assessment-6
```

---

Build and Start the Stack

```bash
docker compose build
docker compose up -d
```

Verify containers are running:

```bash
docker ps
```

Expected services:

* Application
* Prometheus
* Grafana

---

 Service Access URLs

| Service          | URL                                                            |
| ---------------- | -------------------------------------------------------------- |
| Sample App       | [http://localhost:5001](http://localhost:5001)                 |
| Metrics Endpoint | [http://localhost:5001/metrics](http://localhost:5001/metrics) |
| Prometheus       | [http://localhost:9090](http://localhost:9090)                 |
| Grafana          | [http://localhost:3000](http://localhost:3000)                 |

---

Verification Steps

Generate Application Traffic

Refresh the app page multiple times:

```
http://localhost:5001
```

---

Verify Metrics Endpoint

```bash
curl http://localhost:5001/metrics | grep app_requests_total
```

Expected output:

```
app_requests_total <number>
```

---

Verify Prometheus Scraping

Open:

```
http://localhost:9090/targets
```

Target status should be **UP**.

Run query:

```
app_requests_total
```

---

Configure Grafana

Login:

```
http://localhost:3000
Username: admin
Password: admin
```

Add Prometheus data source:

* Type: Prometheus
* URL: `http://prometheus:9090`
* Save & Test

---
View Dashboard Metrics

Example PromQL queries:

```
app_requests_total
```

```
rate(app_requests_total[30s])
```

Ensure the time range is set to **Last 5 minutes**.

---

