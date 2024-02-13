# Grafana Stack with docker-compose

Here is a combination of monitoring using Grafana, Prometheus, Node Exporter, Blackbox Exporter, Cadvisor, and sending notifications to Telegram.

## How to use

Run below command to copy repository

```
git clone https://github.com/agungngrh/docker-grafana-stack.git
```

Go into the directory

```
cd docker-grafana-stack
```

> Change the configuration according to your needs.

By default the user and password when login is **admin:admin**. To change this you can edit grafana/config.monitoring

```
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=admin
```

To change the target to be monitored you can edit prometheus/prometheus.yml
