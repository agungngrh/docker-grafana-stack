# Grafana Stack with docker-compose

Here is a combination of monitoring using Grafana, Prometheus, Node Exporter, Blackbox Exporter, Cadvisor, and sending notifications to Telegram.

## How to use

Run command below to copy repository

```
git clone https://github.com/agungngrh/docker-grafana-stack.git
```

Go into the directory

```
cd docker-grafana-stack
```

> **Change the configuration depends to your needs.**

By default user and password when login is **admin:admin**. To change that you can edit grafana/config.monitoring

```
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=admin
```

To change the target to be monitored you can edit prometheus/prometheus.yml

If the configuration is complete, you can run it with command below

```
docker compose up -d
```

![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/docker-compose-up-d.png)

To check if the container is running use the following command

```
docker ps -a
```

![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/docker-ps-a.png)

You can access Grafana UI from the browser by entering ip_address:3000, enter the username and password that have been set, then it will appear like this:

![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/home-grafana.png)
