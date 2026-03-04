# 📊 Unified Server Monitoring Dashboard
## Prometheus + Grafana + Node Exporter for Multi-Server Infrastructure

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Prometheus](https://img.shields.io/badge/Prometheus-2.47+-orange)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-9.0+-blue)](https://grafana.com/)
[![Node Exporter](https://img.shields.io/badge/Node_Exporter-1.7+-green)](https://github.com/prometheus/node_exporter)
[![Production](https://img.shields.io/badge/Production-IMS_Software-success)](https://imssoftware.com.np)

Production-ready unified monitoring dashboard deployed at **IMS Software** to monitor **100+ servers** with real-time visibility into application health, service downtimes, and resource utilization across the entire infrastructure.

---

## 🎯 Problem Solved

At **IMS Software**, managing 100+ production servers running multiple critical applications created severe operational challenges:

### Before This Solution:
- ❌ **Application Downtime Blindness** - No visibility when critical services went down
- ❌ **Manual Service Checks** - DevOps team had to SSH into each server to check application status
- ❌ **Delayed Incident Response** - Service failures discovered by customers, not monitoring
- ❌ **No Historical Data** - Couldn't identify patterns in service restarts or failures
- ❌ **Resource Exhaustion** - Servers running out of memory/disk discovered too late
- ❌ **Multiple Applications** - Dozens of systemd services across 100+ servers with no centralized view

### After This Solution:
- ✅ **Real-Time Service Monitoring** - Instant visibility when any application goes down
- ✅ **Centralized Dashboard** - Monitor all 100+ servers and their applications from single interface
- ✅ **Proactive Alerting** - Email notifications before services fail (high CPU/memory warnings)
- ✅ **Service Uptime Tracking** - See which applications restart frequently
- ✅ **Historical Analysis** - Identify patterns in service failures and resource usage
- ✅ **Reduced MTTR** - Mean Time To Recovery reduced from hours to minutes

---

## 🏢 Production Deployment at IMS Software

### Scale & Impact
- **Servers Monitored**: 100+ production servers
- **Applications Tracked**: Multiple systemd services per server (web apps, databases, APIs, background workers)
- **Primary Use Case**: Application downtime detection and service health monitoring
- **Team**: DevOps and Operations teams
- **Uptime Improvement**: 99.5% → 99.9% application availability
- **Alert Response Time**: Reduced from 30+ minutes to <2 minutes

### Key Metrics Monitored
1. **Application Service Status** - Running/Failed/Inactive states for all critical services
2. **Service Restart Counts** - Identify unstable applications
3. **Service Uptime** - Track how long each application has been running
4. **Resource Usage** - CPU, Memory, Disk to prevent resource exhaustion
5. **Server Health** - Overall system status across all infrastructure

---

## 📊 What's Monitored

### ✅ Application & Service Monitoring (Primary Focus)
1. **Systemd Service Status** - Running, Failed, Inactive, Activating states
2. **Service Uptime** - Time since each application last started
3. **Restart Count** - Number of times each service has restarted (identifies unstable apps)
4. **Failed Services Alert** - Immediate notification when any application goes down
5. **Custom Service Filtering** - Monitor specific applications (e.g., `webapp.*`, `api.*`, `worker.*`)

### ✅ System Metrics (Per Server)
1. **CPU Usage** - Real-time utilization percentage, idle time, per-core breakdown
2. **Memory Usage** - Total RAM, available RAM, used RAM, swap usage
3. **Disk Usage** - Per-mount point utilization, available space, total capacity
4. **Network I/O** - Inbound/outbound traffic rates, packet counts, errors
5. **System Load** - 1-minute, 5-minute, 15-minute load averages
6. **Uptime** - Server uptime since last reboot

### ✅ Global Overview
1. **Total Servers** - Count of all monitored servers (100+)
2. **Running Services** - Total applications running across infrastructure
3. **Failed Services** - Critical alert count for down applications
4. **System Health Score** - Overall infrastructure health percentage

### ❌ What's NOT Monitored (By Default)
- Application-specific metrics (requires custom exporters)
- Database query performance (use dedicated DB exporters)
- Container metrics (use cAdvisor for Docker/Kubernetes)
- Log aggregation (use Loki/ELK stack)
- APM traces (use Jaeger/Zipkin)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│              IMS Software Production Servers (100+)          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Server 1   │  │   Server 2   │  │  Server 100  │      │
│  │              │  │              │  │              │      │
│  │ Applications:│  │ Applications:│  │ Applications:│      │
│  │ - webapp.svc │  │ - api.svc    │  │ - worker.svc │      │
│  │ - db.svc     │  │ - queue.svc  │  │ - cache.svc  │      │
│  │              │  │              │  │              │      │
│  │ Node Exporter│  │ Node Exporter│  │ Node Exporter│      │
│  │   :9100      │  │   :9100      │  │   :9100      │ ...  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
└─────────┼──────────────────┼──────────────────┼─────────────┘
          │                  │                  │
          │    Scrapes metrics every 15s        │
          └──────────────────┼──────────────────┘
                             │
                    ┌────────▼─────────┐
                    │   Prometheus     │
                    │     :9090        │
                    │                  │
                    │ - Stores metrics │
                    │ - Evaluates rules│
                    │ - Triggers alerts│
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │    Grafana       │
                    │     :3000        │
                    │                  │
                    │ - Visualizes data│
                    │ - Dashboards     │
                    │ - DevOps access  │
                    └──────────────────┘
```

**How It Works:**
1. **Node Exporter** runs on each of 100+ servers, exposing metrics on port 9100
2. **Prometheus** scrapes metrics from all Node Exporters every 15 seconds
3. **Prometheus** stores time-series data and evaluates alert rules for service failures
4. **Grafana** queries Prometheus and displays application status in real-time dashboards
5. **Alertmanager** sends email notifications when applications go down or resources are critical

---

## 📊 Grafana Dashboard Features

### 20+ Monitoring Panels Organized in 5 Categories:

#### 1. Global Overview (4 panels)
- **Total Servers** - Count of all 100+ monitored servers with UP status
- **Running Services** - Total applications running across all infrastructure
- **Failed Services** - **CRITICAL** - Count of down applications requiring immediate attention
- **System Health Score** - Overall infrastructure health percentage

#### 2. Application Service Monitoring (4 panels) - **PRIMARY FOCUS**
- **Service Status Table** - All monitored applications with Running/Failed/Inactive states
- **Service Uptime** - Time since each application last started
- **Service Restart Count** - Identify unstable applications that restart frequently
- **Failed Services Alert** - **RED ALERT** - Immediate notification of any down applications

#### 3. Per-Server Resource Metrics (Dynamic Panels)
- **CPU Usage** - Real-time utilization with hostname identification
- **Memory Usage** - RAM consumption with available/used/total breakdown
- **Disk Usage** - Filesystem utilization by mount point (/, /home, /var, etc.)
- **Network I/O** - Inbound/outbound traffic rates in MB/s

#### 4. System Performance (4 panels)
- **System Load Average** - 1-min, 5-min, 15-min load averages
- **Server Uptime** - Time since last reboot for each server
- **Process Count** - Total running processes per server
- **Context Switches** - System context switch rate

#### 5. Resource Alerts (3 panels)
- 🟡 **Warning Alerts** - CPU > 80%, Memory > 85%, Disk > 80%
- 🔴 **Critical Alerts** - CPU > 90%, Memory > 95%, Disk > 90%, **Service Down**
- 📊 **Alert History** - Timeline of past alerts and service failures

### Dashboard Features
- **Server Filter Dropdown** - Multi-select filter for specific servers or groups
- **Service Filter** - Filter by application name to see specific service status
- **Time Range Selector** - View metrics from last 5m to 30 days
- **Auto-refresh** - Real-time updates every 10 seconds for service status
- **Variable Templates** - Dynamic panels that adapt to your infrastructure
- **Drill-down Capability** - Click any metric to see detailed breakdown
- **Export Options** - Download dashboard as JSON or PDF for reports

---

## 🚀 Quick Start

### Prerequisites

| Component | Min Version | Purpose |
|-----------|-------------|---------|
| **Prometheus** | 2.40+ | Metrics collection & storage |
| **Grafana** | 9.0+ | Dashboard visualization |
| **Node Exporter** | 1.5+ | System & service metrics collection |
| **Alertmanager** | 0.27+ | Alert notifications (optional) |

### Required Ports

| Port | Service | Direction |
|------|---------|-----------|
| 9100 | Node Exporter | Inbound from Prometheus |
| 9090 | Prometheus | Inbound from Grafana |
| 3000 | Grafana | Inbound from Users |
| 9093 | Alertmanager | Inbound (optional) |

### System Requirements

| Component | CPU | RAM | Disk |
|-----------|-----|-----|------|
| Prometheus | 2 cores | 4 GB | 50 GB+ |
| Grafana | 1 core | 1 GB | 10 GB |
| Node Exporter | 0.5 core | 256 MB | 100 MB |

---

## 📦 Installation

### Step 1: Install Prometheus (Main Server)

```bash
# Download Prometheus
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.47.0/prometheus-2.47.0.linux-amd64.tar.gz
tar xvfz prometheus-2.47.0.linux-amd64.tar.gz

# Install binaries
sudo mv prometheus-2.47.0.linux-amd64/prometheus /usr/local/bin/
sudo mv prometheus-2.47.0.linux-amd64/promtool /usr/local/bin/

# Create directories
sudo mkdir -p /etc/prometheus
sudo mkdir -p /var/lib/prometheus

# Create user
sudo useradd -rs /bin/false prometheus

# Set permissions
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus

# Create systemd service
sudo tee /etc/systemd/system/prometheus.service << 'EOF'
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
EOF

# Start Prometheus
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
```

### Step 2: Install Grafana (Main Server)

```bash
# Add Grafana repository
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

# Install Grafana
sudo apt-get update
sudo apt-get install -y grafana

# Start Grafana
sudo systemctl daemon-reload
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
sudo systemctl status grafana-server

# Access Grafana
# URL: http://<server-ip>:3000
# Default credentials: admin/admin
```

### Step 3: Install Node Exporter (All 100+ Monitored Servers)

```bash
# Download Node Exporter
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz

# Install binary
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/

# Create user
sudo useradd -rs /bin/false node_exporter

# Create systemd service
sudo tee /etc/systemd/system/node_exporter.service << 'EOF'
[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
  --collector.systemd \
  --collector.systemd.unit-include="(webapp|api|worker|db|cache).*"

[Install]
WantedBy=multi-user.target
EOF

# Start Node Exporter
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

# Verify metrics
curl http://localhost:9100/metrics | grep node_uname_info
curl http://localhost:9100/metrics | grep node_systemd_unit_state
```

**⚠️ Important**: Change `(webapp|api|worker|db|cache).*` to match your actual service names

### Step 4: Configure Prometheus

Edit `/etc/prometheus/prometheus.yml`:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets:
          - 'server1-ip:9100'
          - 'server2-ip:9100'
          - 'server3-ip:9100'
          # ... add all 100+ servers
        labels:
          env: 'production'
          company: 'ims-software'
```

Reload Prometheus:
```bash
sudo systemctl restart prometheus
```

Verify targets: `http://<prometheus-ip>:9090/targets`

### Step 5: Import Dashboard to Grafana

**Method 1: Via UI**
1. Login to Grafana: `http://<grafana-ip>:3000`
2. Go to: **Dashboards → Import**
3. Upload: `unified_server_monitoring.json`
4. Select Prometheus data source
5. Click **Import**

**Method 2: Via API**
```bash
curl -X POST \
  http://admin:admin@localhost:3000/api/dashboards/db \
  -H 'Content-Type: application/json' \
  -d @unified_server_monitoring.json
```

---

## 🔥 Firewall Configuration

### UFW (Ubuntu/Debian)

**On Prometheus/Grafana Server:**
```bash
sudo ufw allow 9090/tcp  # Prometheus
sudo ufw allow 3000/tcp  # Grafana
sudo ufw allow 9093/tcp  # Alertmanager (optional)
```

**On All 100+ Monitored Servers:**
```bash
sudo ufw allow 9100/tcp  # Node Exporter
```

### firewalld (RHEL/CentOS)

**On Prometheus/Grafana Server:**
```bash
sudo firewall-cmd --permanent --add-port=9090/tcp
sudo firewall-cmd --permanent --add-port=3000/tcp
sudo firewall-cmd --permanent --add-port=9093/tcp
sudo firewall-cmd --reload
```

**On All Monitored Servers:**
```bash
sudo firewall-cmd --permanent --add-port=9100/tcp
sudo firewall-cmd --reload
```

---

## 📈 Adding New Servers

### Step 1: Install Node Exporter on New Server
Follow **Step 3** from Installation section

### Step 2: Add to Prometheus Configuration
Edit `/etc/prometheus/prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets:
          - 'existing-server1:9100'
          - 'existing-server2:9100'
          - 'new-server:9100'  # Add this line
```

### Step 3: Reload Prometheus
```bash
sudo systemctl restart prometheus
```

### Step 4: Verify in Grafana
The new server will automatically appear in the dashboard within 15 seconds!

---

## 🎨 Customizing Service Monitoring

To monitor your specific applications, edit the Node Exporter service file:

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

Change the `--collector.systemd.unit-include` flag to match your service names:

```ini
# Example for IMS Software applications
ExecStart=/usr/local/bin/node_exporter \
  --collector.systemd \
  --collector.systemd.unit-include="(webapp|api|worker|database|cache|queue).*"

# Monitor all services (not recommended - too noisy)
ExecStart=/usr/local/bin/node_exporter \
  --collector.systemd
```

Restart Node Exporter:
```bash
sudo systemctl restart node_exporter
```

---

## 🔔 Email Alerting for Service Downtime

### Step 1: Install Alertmanager

```bash
cd /tmp
wget https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz
tar xvfz alertmanager-0.27.0.linux-amd64.tar.gz
sudo mv alertmanager-0.27.0.linux-amd64/alertmanager /usr/local/bin/
sudo mkdir -p /etc/alertmanager
```

### Step 2: Configure Alertmanager

Create `/etc/alertmanager/alertmanager.yml`:

```yaml
global:
  smtp_smarthost: 'smtp.gmail.com:587'
  smtp_from: 'alerts@imssoftware.com.np'
  smtp_auth_username: 'your-email@gmail.com'
  smtp_auth_password: 'your-app-password'

route:
  receiver: 'devops-team'
  group_by: ['alertname', 'instance']
  group_wait: 10s
  group_interval: 2m
  repeat_interval: 4h

receivers:
  - name: 'devops-team'
    email_configs:
      - to: 'devops@imssoftware.com.np'
        headers:
          Subject: '🚨 {{ .GroupLabels.alertname }} - {{ .GroupLabels.instance }}'
```

### Step 3: Create Alert Rules for Service Downtime

Create `/etc/prometheus/alert_rules.yml`:

```yaml
groups:
  - name: service_alerts
    interval: 15s
    rules:
      - alert: ServiceDown
        expr: node_systemd_unit_state{state="failed"} == 1
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service {{ $labels.name }} is DOWN on {{ $labels.instance }}"
          description: "Application {{ $labels.name }} has failed and needs immediate attention"

      - alert: ServiceRestarting
        expr: rate(node_systemd_unit_state{state="activating"}[5m]) > 0.1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Service {{ $labels.name }} is restarting frequently on {{ $labels.instance }}"
          description: "Application {{ $labels.name }} has restarted multiple times in the last 5 minutes"

      - alert: HighCPUUsage
        expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage on {{ $labels.instance }}"
          description: "CPU usage is {{ $value }}%"

      - alert: HighMemoryUsage
        expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 85
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High memory usage on {{ $labels.instance }}"
          description: "Memory usage is {{ $value }}%"

      - alert: HighDiskUsage
        expr: (1 - (node_filesystem_avail_bytes / node_filesystem_size_bytes)) * 100 > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High disk usage on {{ $labels.instance }}"
          description: "Disk usage is {{ $value }}%"
```

### Step 4: Update Prometheus Configuration

Edit `/etc/prometheus/prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['localhost:9093']

rule_files:
  - '/etc/prometheus/alert_rules.yml'

scrape_configs:
  # ... existing scrape configs
```

Restart services:
```bash
sudo systemctl restart prometheus
sudo systemctl restart alertmanager
```

---

## 🛠️ Troubleshooting

### Node Exporter Not Showing Metrics

```bash
# Check service status
sudo systemctl status node_exporter

# Check logs
sudo journalctl -u node_exporter -n 50

# Test metrics endpoint
curl http://localhost:9100/metrics

# Check firewall
sudo ufw status
```

### Prometheus Not Scraping Targets

```bash
# Check Prometheus logs
sudo journalctl -u prometheus -n 50

# Verify configuration
promtool check config /etc/prometheus/prometheus.yml

# Check targets in UI
# http://<prometheus-ip>:9090/targets
```

### Dashboard Not Showing Data

1. **Verify Prometheus data source** in Grafana:
   - Go to: Configuration → Data Sources
   - Test connection to Prometheus

2. **Check time range** in dashboard (top-right corner)

3. **Verify metrics exist** in Prometheus:
   ```
   http://<prometheus-ip>:9090/graph
   Query: up{job="node_exporter"}
   ```

### Services Not Appearing in Dashboard

1. **Check Node Exporter systemd collector**:
   ```bash
   curl http://localhost:9100/metrics | grep node_systemd_unit_state
   ```

2. **Verify service name pattern** in Node Exporter config:
   ```bash
   sudo systemctl cat node_exporter | grep unit-include
   ```

3. **Check service is running**:
   ```bash
   sudo systemctl status your-service-name
   ```

---

## 📁 Repository Files

### Core Configuration Files
- **`unified_server_monitoring.json`** - Grafana dashboard with 20+ panels
- **`prometheus.yml`** - Example Prometheus configuration for 100+ servers
- **`alert_rules.yml`** - Alert rules for service downtime and resource usage
- **`alertmanager.yml`** - Alertmanager email configuration

### Documentation
- **`README.md`** - This file
- **`SERVER_MONITORING_GUIDE.pdf`** - Detailed setup guide with screenshots
- **`UNIFIED_SERVER_MONITORING_DOCUMENTATION.pdf`** - Complete technical documentation

---

## 📊 Sample Queries

### All Failed Services
```promql
node_systemd_unit_state{state="failed"} == 1
```

### Service Uptime
```promql
time() - node_systemd_unit_start_time_seconds
```

### CPU Usage by Server
```promql
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

### Memory Usage Percentage
```promql
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100
```

### Disk Usage by Mount Point
```promql
(1 - (node_filesystem_avail_bytes / node_filesystem_size_bytes)) * 100
```

---

## 📈 Performance & Scalability

### IMS Software Production Stats
- **Servers Monitored**: 100+ production servers
- **Metrics Collected**: ~100,000 metrics per scrape cycle
- **Storage**: ~2 GB/day for all servers
- **Query Performance**: <1 second for most dashboard queries
- **Uptime**: 99.9% monitoring system availability

### Resource Usage
- **Prometheus**: ~4 GB RAM for 100 servers, 15-day retention
- **Grafana**: ~1 GB RAM for 10 concurrent users
- **Node Exporter**: ~50 MB RAM per server

---

## 🔐 Security Best Practices

- ✅ Run Prometheus/Grafana behind reverse proxy (Nginx/Apache)
- ✅ Enable HTTPS with Let's Encrypt certificates
- ✅ Use strong passwords for Grafana admin account
- ✅ Restrict Prometheus/Node Exporter ports to internal network
- ✅ Enable Grafana authentication (LDAP/OAuth)
- ✅ Regularly update all components to latest versions
- ✅ Use firewall rules to limit access to monitoring ports
- ✅ Enable audit logging in Grafana

---

## 📝 License

MIT License - See LICENSE file for details

---

## 👥 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

---

## 📧 Support

For issues or questions:
- Open an issue on GitHub
- Check `SERVER_MONITORING_GUIDE.pdf` for detailed instructions
- Review Prometheus documentation: https://prometheus.io/docs/

---

## 🙏 Acknowledgments

- [Prometheus](https://prometheus.io/) - Monitoring and alerting toolkit
- [Grafana](https://grafana.com/) - Visualization platform
- [Node Exporter](https://github.com/prometheus/node_exporter) - Hardware and OS metrics exporter
- **IMS Software** - Production deployment and real-world validation

---

**Built for production environments requiring comprehensive, scalable, and reliable server monitoring. Successfully deployed at IMS Software to monitor 100+ servers and multiple critical applications.**
