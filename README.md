# Grafana Stack with docker-compose

Hello guys! Here is a combination of monitoring using Grafana, Prometheus, Node Exporter, Blackbox Exporter, Cadvisor, and sending notifications to Telegram.

## How to use

Run command below to copy repository

```
git clone https://github.com/goongrh/docker-grafana-stack.git
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

![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/home-page.png)

## How to setup Telegram notifications

### Blackbox sends a message if any target website is down

1. First of all, you must create a Telegram bot with BotFather. Click on this link https://t.me/botfather or you can open Telegram then search for BotFather

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/botfather.png)

2. Type /newbot and then follow the instructions

3. Copy your BOT API Token

4. Create a new Telegram group. This group will receive notifications

5. Add your new bot to the group

6. Open below link in browser
    ```
    https://api.telegram.org/bot< YOUR BOT API TOKEN >/getUpdates
    ```
    To get your Telegram group ID you must send 1 message in the group. Refresh the tab then find the ID. The ID begins with -

7. Open your Grafana UI, click on ☰ Menu > 🔔Alerting > Contact points

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/alerting.png)

8. Click on Notification Templates > + Add notification template

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/notification-template.png)

    You can copy the template below then save it

    ```
    {{ define "telegram_blackbox" }}
      {{ if .Alerts.Firing }}
      *FIRING: {{ len .Alerts.Firing }} alerts*
      {{ range .Alerts.Firing }} 
      {{ if .Annotations }}
      {{- end -}}
      Description: {{ .Annotations.description }}
      Instance: {{ .Labels.instance }}
      Summary: {{ .Annotations.summary }}
      {{- end -}}
      {{ end }}
      {{ if .Alerts.Resolved }}
      RESOLVED: {{ len .Alerts.Resolved }} alerts
      {{ range .Alerts.Resolved }}
      {{ if .Annotations }}
      {{- end -}}
      Description: {{ .Annotations.description }}
      Instance: {{ .Labels.instance }}
      Summary: {{ .Annotations.summary }}
      {{- end -}}
      {{ end }}
    {{ end }}
    ```

9. Back to Contact Points and click + Add contact point

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/contact-points.png)

    Fill the Name > select Integration (Telegram) > insert BOT Api Token > insert group Chat ID > Optional Telegram settings > fill the Message 
    
    ```
    {{ template "telegram_blackbox" . }}
    ```

    select Parse Mode > Test > Save contact point

    You will get a test notification in the group that has been created

10. Go to Notification policies > Edit

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/notification-policies.png)

    Change Default contact point from grafana-default-email to Telegram then click Update default policy. 
    
    (Optional) Back to Contact points and delete grafana-default-email by click More > Delete

11. Go to Alert rules > + New alert rule

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/new-alert-rule.png)

    Fill the name > on Metric select probe_http_status_code > on Label filters select instance = target website > + > select job = blackbox_exporter > click Set as alert condition

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/expressions.png)

    Scroll down to Expressions > edit Threshold to IS BELOW 1 > click Set as alert condition

    ![](https://github.com/goongrh/docker-grafana-stack/blob/main/images/behavior%20%26%20annotations.png)

    Scroll down to Set evaluation behavior > create new folder > create new group > add summary > add description

    If everything is done you can save it

    ***That's all I can share, hopefully it can help you guys***
