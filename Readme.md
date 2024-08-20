# Docker monitoring stack
It's a production ready demonstration featuring monitoring for web applications.

# Included components
## Server
- **Uptime Kuma** - monitoring of availability of HTTP and TCP services
- **Graylog** - storing and processing logs
- **Prometheus** - storing server metrics
- **Grafana** - visualiztion of metrics

## Client
- **rsyslog** - injecting logs to `graylog`
- **node_exporter** - exporting node metrics

# Quickstart
## Server
- Clone the repo
```bash
git clone https://github.com/bakabtw/docker-monitoring-stack
```

- Install `docker` and `docker-compose`
```bash
apt install docker.io docker-compose-v2
```

- Enter the directory
```bash
cd docker-monitoring-stack
```

- Launch `docker-compose`
```bash
docker compose up -d
```

# Configuration
## Server
- Generate `GRAYLOG_DATANODE_ROOT_PASSWORD_SHA2`:
```bash
$ echo -n 'password' | sha256sum
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8  -
```
- Generate `GRAYLOG_DATANODE_PASSWORD_SECRET`:
```bash
pwgen -N 1 -s 96
```

- Updated prometheus scraping targets:
```yml
- job_name: "node"

static_configs:
    - targets: ["de1.node.payload.network:9100"]
```

## Client
- Install `prometheus-node-exporter`:
```bash
apt install prometheus-node-exporter
```

- Restrict access to `prometheus-node-exporter`:
```bash
iptables -I INPUT -p tcp ! -s yourIPaddress --dport 9100 -j DROP
```

- Copy the contect of [client/rsyslog/graylog.conf](client/rsyslog/graylog.conf) to `/etc/rsyslog/graylog.conf`

