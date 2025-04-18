## Docker supports **multiple logging drivers**, giving you flexibility in how container logs are collected, stored, and shipped. Here's a breakdown of the main ones:

---

## 🧾 Docker Logging Drivers Overview

You can specify the logging driver **per container** or **globally** via the Docker daemon.

### ✅ 1. **`json-file`** (Default)
- Stores logs in JSON format on the host
- Location: `/var/lib/docker/containers/<container-id>/<container-id>-json.log`
- Supports log rotation with options like `max-size`, `max-file`

```bash
docker run --log-driver=json-file ...
```

### ✅ 2. **`journald`**
- Sends logs to the **systemd journal**
- Useful if your host OS uses systemd (`journalctl`)

```bash
docker run --log-driver=journald ...
```

### ✅ 3. **`syslog`**
- Sends logs to the **syslog daemon**
- Can forward logs to remote syslog servers
- Works with `rsyslog`, `syslog-ng`, etc.

```bash
docker run --log-driver=syslog ...
```

### ✅ 4. **`local`**
- A newer default option on some systems
- Uses a custom binary format (faster than `json-file`)
- Supports better performance and rotation
- Stored at: `/var/lib/docker/containers/...`

```bash
docker run --log-driver=local ...
```

### ✅ 5. **`fluentd`**
- Sends logs to a **Fluentd** collector
- Useful for structured logging and shipping to Elasticsearch, etc.

```bash
docker run --log-driver=fluentd ...
```

### ✅ 6. **`gelf`**
- Sends logs to a **Graylog Extended Log Format** endpoint
- Used with Graylog and other GELF-compatible log systems

```bash
docker run --log-driver=gelf ...
```

### ✅ 7. **`awslogs`**
- Sends logs to **Amazon CloudWatch Logs**
- Must configure AWS credentials and log group

```bash
docker run --log-driver=awslogs ...
```

### ✅ 8. **`splunk`**
- Sends logs to **Splunk HTTP Event Collector (HEC)**
- Useful in enterprise log aggregation setups

```bash
docker run --log-driver=splunk ...
```

### ✅ 9. **`logentries`** (deprecated)
- Used to send logs to **Logentries** (now InsightOps)

### ✅ 10. **`none`**
- Disables logging entirely

```bash
docker run --log-driver=none ...
```

## 🔧 Set Global Default Logging Driver

Edit or create `/etc/docker/daemon.json`:

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

Then restart Docker:
```bash
sudo systemctl restart docker
```

---

## ✅ Summary Table

| Driver     | Destination            | Rotation | Use Case                     |
|------------|------------------------|----------|------------------------------|
| json-file  | Local file (JSON)      | ✅       | Default, local debugging     |
| local      | Local (binary)         | ✅       | Performance-focused logging  |
| journald   | systemd journal        | ❌       | Systemd-based systems        |
| syslog     | syslog/rsyslog         | ❌       | Legacy/central syslog        |
| fluentd    | Fluentd collector      | ❌       | Structured log pipelines     |
| gelf       | Graylog, GELF server   | ❌       | Enterprise logging           |
| awslogs    | AWS CloudWatch         | ❌       | AWS environments             |
| splunk     | Splunk HEC             | ❌       | Enterprise monitoring        |
| none       | Disables logging       | —        | No logs needed               |

---

Want help configuring any of these in a real-world setup or forwarding logs to a central location like ELK, Loki, or Datadog?

Walk through **how to forward Docker logs to a central logging system**. I’ll give you an overview of popular options, and then we can go step-by-step into whichever one you want.

---

## 🔁 Centralized Logging Options for Docker Containers

### 🟢 Option 1: **ELK Stack (Elasticsearch + Logstash + Kibana)**
- Use **Filebeat** or **Logstash** to ship logs from `/var/lib/docker/containers`
- Visualize logs with Kibana
- Scalable and very flexible

### 🔵 Option 2: **Grafana Loki + Promtail**
- Lightweight, simpler than ELK
- Promtail scrapes container logs and ships to Loki
- View via Grafana

### 🟣 Option 3: **Fluentd / Fluent Bit**
- Lightweight log collector/forwarder
- Supports Docker integration and output plugins for:
  - Elasticsearch
  - Kafka
  - CloudWatch
  - Databases
- Works well with Kubernetes too

### 🟡 Option 4: **Datadog / Loggly / Splunk / CloudWatch**
- SaaS-based centralized logging
- Use Docker log drivers (e.g., `awslogs`, `splunk`, etc.)
- Or sidecar agents to collect logs

---

## 🔧 Want to Start With ELK?

Here’s a typical Docker + ELK flow:

### 💡 Overview Diagram

```text
Docker Containers
     ↓ (json logs)
/var/lib/docker/containers/*.log
     ↓
Filebeat (or Logstash)
     ↓
Elasticsearch
     ↓
Kibana (View logs)
```

---

## 🛠️ Steps: Send Docker Logs to ELK Stack Using Filebeat

### ✅ Step 1: Install Filebeat on the Docker host

On Ubuntu:
```bash
sudo apt install filebeat
```

### ✅ Step 2: Configure Filebeat

Edit `/etc/filebeat/filebeat.yml`:

```yaml
filebeat.inputs:
  - type: container
    paths:
      - /var/lib/docker/containers/*/*.log
    processors:
      - add_docker_metadata: ~

output.elasticsearch:
  hosts: ["http://localhost:9200"]
```

> You can replace `localhost:9200` with a remote Elasticsearch server if needed.

---

### ✅ Step 3: Start Filebeat

```bash
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

Check logs:
```bash
sudo journalctl -u filebeat
```

---

### ✅ Step 4: Open Kibana

- Go to `http://localhost:5601`
- Use the **Discover** tab to see logs
- You can filter by container ID, container name, etc.

---
