 🚀 Docker Monitoring, Logging & Alerting Suite

A **secure, out-of-the-box monitoring, logging, and alerting stack** for Docker hosts and containers.

This stack provides:

* 📊 Monitoring (metrics)
* 📜 Logging (log aggregation & search)
* 🚨 Alerting (metrics & logs)
* 🔐 Optional secure HTTPS mode with reverse proxy

---

# 🧩 Tech Stack

## 📊 Monitoring

* cAdvisor – Container metrics
* node_exporter – Host metrics
* Prometheus – Metrics storage
* Grafana – Visualization dashboards

## 📜 Logging (ELK Stack 6.3.0)

* Filebeat – Log collection
* Logstash – Log processing
* Elasticsearch – Log storage
* Kibana – Log visualization

## 🚨 Alerting

* Elastalert – Log-based alerts
* Prometheus Alertmanager – Metric-based alerts

## 🔐 Security (Optional Secure Mode)

* nginx reverse proxy
* Let's Encrypt SSL
* HTTPS only
* Basic authentication for dashboards
* No direct port exposure

---

# 📦 Setup Instructions

## 1️⃣ Clone Repository

```bash
git clone https://github.com/uschtwill/docker_monitoring_logging_alerting.git
cd docker_monitoring_logging_alerting
```

---

## 2️⃣ Install Prerequisites

Check or run:

```bash
sh install-prerequisites.sh
```

---

## 🔐 Secure Mode (Recommended)

Before running setup:

Create DNS A-records for:

* grafana.YOUR_DOMAIN
* kibana.YOUR_DOMAIN
* prometheus.YOUR_DOMAIN
* alertmanager.YOUR_DOMAIN

Point them to your server IP.

Then run:

```bash
sh setup.sh secure YOUR_DOMAIN VERY_STRONG_PASSWORD
```

### Secure Mode Features

* HTTPS enabled automatically
* SSL certificates auto-generated & renewed
* Basic authentication enabled
* Dashboards accessible at:

```
https://grafana.YOUR_DOMAIN
https://kibana.YOUR_DOMAIN
https://prometheus.YOUR_DOMAIN
https://alertmanager.YOUR_DOMAIN
```

---

## 🔓 Unsecure Mode (Local Testing)

```bash
sh setup.sh unsecure
```

Access dashboards locally:

| Service      | URL                                            |
| ------------ | ---------------------------------------------- |
| Kibana       | [http://localhost:5601](http://localhost:5601) |
| Grafana      | [http://localhost:3000](http://localhost:3000) |
| Prometheus   | [http://localhost:9090](http://localhost:9090) |
| Alertmanager | [http://localhost:9093](http://localhost:9093) |
| cAdvisor     | [http://localhost:8080](http://localhost:8080) |

Test Elasticsearch:

```bash
curl localhost:9200
```

---

# 📁 Adding Your Own Containers

To enable monitoring & logging for your containers:

1. Use the same logging configuration as in `docker-compose.yml`
2. Add label:

```yaml
labels:
  - container_group=your_group
```

---

# 🔔 Alerting Configuration

## Log Alerts (Elastalert)

Rules location:

```
./elastalert/rules/
```

## Monitoring Alerts (Prometheus)

Rules location:

```
./prometheus/rules/
```

Alert outputs supported:

* Logstash
* Slack (add webhook URL)

---

# 📊 Grafana Dashboards

Main dashboards:

* Container & Host Overview
* Data Exploration
* Alerts with Annotations

Note:
After ELK 6.3.0 update, Kibana index patterns must be created manually.

Wait 2–3 minutes after startup for logs to appear.

---

# 🛠 Debugging Tips

If ELK logging causes issues, comment logging section in docker-compose:

```yaml
# logging:
#   driver: gelf
#   options:
#     gelf-address: udp://YOUR_IP:12201
#     labels: container_group
```

Logs will go to stdout instead.

---

# 🧹 Cleanup

To remove everything:

```bash
sh cleanup.sh secure
```

or

```bash
sh cleanup.sh unsecure
```

---

# ⚠ Known Issue

Bad `umask` may cause permission problems (e.g., Kibana not starting).

Fix before cloning:

```bash
umask 0022
```

---


