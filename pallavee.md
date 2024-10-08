
Conversation opened. 1 unread message.

Skip to content
Using FOSTERing Linux Mail with screen readers

1 of 2,235
(no subject)
Inbox

Pallavee Chaubey
17:28 (0 minutes ago)
to me

﻿**Monitoring & Ticketing task**

**Prometheus set up**

**Download and Install Prometheus**

1. Go to the website <https://prometheus.io/download/>
1. Use link to install prometheus for linux

   [prometheus-2.54.0.linux-amd64.tar.gz](https://github.com/prometheus/prometheus/releases/download/v2.54.0/prometheus-2.54.0.linux-amd64.tar.gz)

3. Extract the tarball Tar xvfz [prometheus-2.54.0.linux-amd64.tar.gz](https://github.com/prometheus/prometheus/releases/download/v2.54.0/prometheus-2.54.0.linux-amd64.tar.gz)
3. Move the Prometheus Binary to /usr/local/bin cd prometheus-2.45.0.linux-amd64
3. **Move the Prometheus and promtool binaries** to /usr/local/bin: sudo mv prometheus /usr/local/bin/

sudo mv promtool /usr/local/bin/

6. Move Configuration Files to /etc/prometheus sudo mkdir /etc/prometheus
6. **Move the configuration files** to /etc/prometheus: sudo mv prometheus.yml /etc/prometheus/

   ```
   sudo mv consoles /etc/prometheus/
   ```

   ```
   sudo mv console\_libraries /etc/prometheus/
   ```

**Set Up the Data Directory**

1. **Create a directory** for Prometheus data:


   ```
   sudo mkdir /var/lib/prometheus
   ```

3. **Ensure proper permissions** are set for the Prometheus user

   sudo chown prometheus:prometheus /var/lib/prometheus sudo chown -R prometheus:prometheus /etc/prometheus

   **Creating systemd service file**

1. **Create a Prometheus Systemd Service File**

sudo nano /etc/systemd/system/prometheus.service

2. **Add the following configuration** to the file: [Unit]

   Description=Prometheus Wants=network-online.target After=network-online.target

   [Service]

   User=prometheus

   Group=prometheus

   Type=simple

   ExecStart=/usr/local/bin/prometheus \ --config.file=/etc/prometheus/prometheus.yml \ --storage.tsdb.path=/var/lib/prometheus/data \

--web.console.templates=/etc/prometheus/consoles

\

--web.console.libraries=/etc/prometheus/console\_libra ries

Restart=always

[Install] WantedBy=multi-user.target

3. Reload Systemd to Apply the New Service

   **Reload the systemd manager configuration** to recognize the new service:

   sudo systemctl daemon-reload

   **Start the Prometheus Service** sudo systemctl start prometheus

   **Check the status** to ensure it’s running correctly: sudo systemctl status prometheus

   Output

   pallavee@pallavee:~$ systemctl status prometheus

- prometheus.service - Prometheus

Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)

Active: active (running) since Mon 2024-08-12 10:39:51 IST; 2h 19min ago

Main PID: 1758 (prometheus)

Tasks: 10 (limit: 9318)

Memory: 118.6M

CPU: 12.541s

CGroup: /system.slice/prometheus.service

└─1758 /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/data --web.console.templates=/etc/prometheus/consoles --we>

Aug 12 10:39:52 pallavee prometheus[1758]: ts=2024-08-12T05:09:52.056Z caller=main.go:1391 level=info msg="updated GOGC" old=100 new=75

Aug 12 10:39:52 pallavee prometheus[1758]: ts=2024-08-12T05:09:52.056Z caller=main.go:1402 level=info msg="Completed loading of configuration file" filename=/etc/prometheus/prometheus.yml t>

Aug 12 10:39:52 pallavee prometheus[1758]: ts=2024-08-12T05:09:52.062Z caller=main.go:1133 level=info msg="Server is ready to receive web requests."

Aug 12 10:39:52 pallavee prometheus[1758]: ts=2024-08-12T05:09:52.062Z caller=manager.go:164 level=info component="rule manager" msg="Starting rule manager..."

Aug 12 10:40:01 pallavee prometheus[1758]: ts=2024-08-12T05:10:01.411Z caller=compact.go:567 level=info component=tsdb msg="write block resulted in empty block" mint=1723399200000 maxt=1723>

Aug 12 10:40:01 pallavee prometheus[1758]: ts=2024-08-12T05:10:01.419Z caller=head.go:1355 level=info component=tsdb msg="Head GC completed" caller=truncateMemory duration=6.33163ms

Aug 12 10:40:01 pallavee prometheus[1758]: ts=2024-08-12T05:10:01.419Z caller=checkpoint.go:101 level=info component=tsdb msg="Creating checkpoint" from\_segment=422 to\_segment=423 mint=1723>

Aug 12 10:40:01 pallavee prometheus[1758]: ts=2024-08-12T05:10:01.437Z caller=head.go:1317 level=info component=tsdb msg="WAL checkpoint complete" first=422 last=423 duration=17.35924ms

Aug 12 12:40:02 pallavee prometheus[1758]: ts=2024-08-12T07:10:02.324Z caller=compact.go:576 level=info component=tsdb msg="write block" mint=1723435801172 maxt=1723442400000 ulid=01J52PRAF> Aug 12 12:40:02 pallavee prometheus[1758]: ts=2024-08-12T07:10:02.330Z caller=head.go:1355 level=info component=tsdb msg="Head GC completed" caller=truncateMemory duration=3.661482ms

lines 1-20/20 (END)

4\.Enable Prometheus to Start on Boot

sudo systemctl enable prometheus

**Troubleshooting**

**View Logs**: If Prometheus fails to start, you can check the logs for errors:

journalctl -u prometheus.service

To access prometheus go to the web browser and enter

[http://localhost:9090/graph?g0.expr=&g0.tab=1&g0.display_ mode=lines&g0.show_exemplars=0&g0.range_input=1h](http://localhost:9090/graph?g0.expr=&g0.tab=1&g0.display_mode=lines&g0.show_exemplars=0&g0.range_input=1h)

Prometheus dashboard visible

![unnamed](https://github.com/user-attachments/assets/d51b8096-5b48-4aee-bbf5-4b25b22a3cdd)

**Node exporter set up**

1. GO to the website https[://prometheus.io/download/](https://prometheus.io/download/)
1. And install node exporter [node_exporter-1.8.2.linux-amd64.tar.gz](https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz)
1. tar xvfz node\_exporter-1.8.2 linux-amd64.tar.gz
1. **Move the Node Exporter binary** to /usr/local/bin:

   sudo mv node\_exporter-1.6.1.linux-amd64/node\_exporter /usr/local/bin/

**Create a Node Exporter Systemd Service File**

1. **Create the service file**:

sudo nano /etc/systemd/system/node\_exporter.service

[Unit]

Description=Node Exporter Wants=network-online.target After=network-online.target

[Service]

User=node\_exporter Group=node\_exporter

Type=simple ExecStart=/usr/local/bin/node\_exporter

[Install] WantedBy=multi-user.target

2. Reload Systemd and Start Node Exporter

sudo systemctl daemon-reload

sudo systemctl start node\_exporter

3. **Enable Node Exporter to start on boot**: sudo systemctl enable node\_exporter
3. Verify the service

sudo systemctl status node\_exporter

Output

- node\_exporter.service - Node Exporter

Loaded: loaded (/etc/systemd/system/node\_exporter.service; enabled; vendor preset: enabled)

Active: active (running) since Mon 2024-08-12 10:39:51 IST; 2h 49min ago

Main PID: 1743 (node\_exporter)

Tasks: 7 (limit: 9318)

Memory: 21.6M

CPU: 27.119s

CGroup: /system.slice/node\_exporter.service

└─1743 /usr/local/bin/node\_exporter

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=time

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=timex

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=udp\_queues

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=uname

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=vmstat

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=watchdog

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=xfs

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.426Z caller=node\_exporter.go:118 level=info collector=zfs

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.433Z caller=tls\_config.go:313 level=info msg="Listening on" address=[::]:9100

Aug 12 10:39:51 pallavee node\_exporter[1743]: ts=2024-08-12T05:09:51.433Z caller=tls\_config.go:316 level=info msg="TLS is disabled." http2=false address=[::]:9100

~

Troubleshooting

journalctl -u node\_exporter.service

To access node exporter go to the web browser and enter localhost: 9100

Node exporter dashboard visible

![unnamed (1)](https://github.com/user-attachments/assets/92896b9f-9eba-415e-a17e-a0ec4746286a)

ALertmanager installation and set up

1. Go to the website https[://prometheus.io/download/](https://prometheus.io/download/)
1. Download alertmanager for linux [alertmanager-0.27.0.linux-amd64.tar.gz](https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz)
1. tar xvfz alertmanager-0.27.0.linux-amd64.tar.gz
1. **Move the Alertmanager binary and files**:

   sudo mv alertmanager-0.26.0.linux-amd64/alertmanager /usr/local/bin/

   sudo mv alertmanager-0.26.0.linux-amd64/amtool /usr/local/bin/

   sudo mkdir /etc/alertmanager

   sudo mv alertmanager-0.26.0.linux-amd64/alertmanager.yml /etc/alertmanager/

5. **Create the service file**:

sudo nano /etc/systemd/system/alertmanager.service Output

/etc/systemd/system/alertmanager.service

[Unit]

Description=Prometheus Alertmanager Service Wants=network-online.target

After=network-online.target

[Service]

User=alertmanager

Group=alertmanager

Type=simple

ExecStart=/usr/local/bin/alertmanager \

--config.file=/etc/alertmanager/alertmanager.yml \ --storage.path=/var/lib/alertmanager/data

[Install] WantedBy=multi-user.target

6. sudo systemctl start alertmanager
6. sudo systemctl enable alertmanager
6. sudo systemctl status alertmanager Output

   pallavee@pallavee:~$ sudo nano /etc/systemd/system/alertmanager.service pallavee@pallavee:~$ sudo systemctl status alertmanager

- alertmanager.service - Prometheus Alertmanager Service

  Loaded: loaded (/etc/systemd/system/alertmanager.service; enabled; vendor preset: enabled)

  Active: active (running) since Mon 2024-08-12 10:39:51 IST; 2h 54min ago

Main PID: 1730 (alertmanager)

Tasks: 9 (limit: 9318)

Memory: 36.3M

CPU: 11.076s

CGroup: /system.slice/alertmanager.service └─1730 /usr/local/bin/alertmanager

--config.file=/etc/alertmanager/alertmanager.yml --storage.path=/var/lib/alertmanager/data

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.513Z caller=main.go:181 level=info msg="Starting Alertmanager" version="(version=0.27.0, branch=HEAD, revision=0aa3c2aad1>

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.516Z caller=main.go:182 level=info build\_context="(go=go1.21.7, platform=linux/amd64, user=root@22cd11f671e9, date=202402>

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.518Z caller=cluster.go:186 level=info component=cluster msg="setting advertise address explicitly" addr=10.42.0.23 port=9>

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.527Z caller=cluster.go:683 level=info component=cluster msg="Waiting for gossip to settle..." interval=2s

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.617Z caller=coordinator.go:113 level=info component=configuration msg="Loading configuration file" file=/etc/alertmanager>

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.621Z caller=coordinator.go:126 level=info component=configuration msg="Completed loading of configuration file" file=/etc>

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.626Z caller=tls\_config.go:313 level=info msg="Listening on" address=[::]:9093

Aug 12 10:39:51 pallavee alertmanager[1730]: ts=2024-08-12T05:09:51.626Z caller=tls\_config.go:316 level=info msg="TLS is disabled." http2=false address=[::]:9093

Aug 12 10:39:53 pallavee alertmanager[1730]: ts=2024-08-12T05:09:53.528Z caller=cluster.go:708 level=info component=cluster msg="gossip not settled" polls=0 before=0 now=1 elapsed=2.0007791>

Aug 12 10:40:01 pallavee alertmanager[1730]: ts=2024-08-12T05:10:01.530Z caller=cluster.go:700 level=info component=cluster msg="gossip settled; proceeding" elapsed=10.002903882s

lines 1-20/20 (END)

Troubleshooting

journalctl -u alertmanager.service

To access node exporter go to the web browser and enter localhost: 9093

Alertmanager dashboard visible

![unnamed (2)](https://github.com/user-attachments/assets/7a59114b-4d30-4421-8737-e7e532145376)

Redmine set up in podman

1\.

2. Install Podman

sudo apt update

sudo apt install podman

3. **Create a Pod for Redmine**

podman pod create --name redmine -p 3100:3000

#!/bin/bash

- Create a folder for PostgreSQL data

echo "Creating folder for PostgreSQL data..." mkdir -p /home/pallavee/data/redmine/postgres

- Change ownership of the folder

echo "Changing ownership of the folder..."

podman unshare chown 999:999 /home/pallavee/data/redmine/postgres

- Create the Redmine Pod

echo "Creating the Redmine Pod..."

podman pod create --name redmine --publish 3100:3000 --publish 5433:5432

- Run the PostgreSQL container

echo "Running the PostgreSQL container..." podman run -dt \

--pod redmine \

--name redmine-postgres \

-e POSTGRES\_DB=keen \

-e POSTGRES\_USER=postgres \

-e POSTGRES\_PASSWORD=password \

-e POSTGRES\_HOST\_AUTH\_METHOD=trust \

-e PGDATA=/var/lib/postgresql/data/pgdata \

-v /home/nidhi/data/redmine/postgres:/var/lib/postgresql/data

\

[docker.io/postgres:latest](http://docker.io/postgres:latest)

- Run the Redmine application container

echo "Running the Redmine application container..." podman run -dt \

--pod redmine \

--name redmine-app \

-e REDMINE\_DB\_POSTGRES=127.0.0.1 \

-e REDMINE\_DB\_PORT=5432 \

-e REDMINE\_DB\_DATABASE=keen \

-e REDMINE\_DB\_USERNAME=postgres \

-e REDMINE\_DB\_PASSWORD=password \ [~~docker.io/redmine:latest~~](http://docker.io/redmine:latest)

4. **Access Redmine**

Once the containers are running, you can access Redmine by navigating to http://localhost:8080 in your web browser.

5. **Start redemine pod usind**

**Podman pod start redmine**

6. **Start redmine using**

   **Podman start redmine-app**

7. **Check status of redmine-app**

podman ps --pod

**Output**

![image](https://github.com/user-attachments/assets/87d7abb5-a2e6-4507-89f5-f1b547dc22b6)


**Access redmine**

1. **Go to the web browser and enter the following Localhost: 3100**
1. **Redmine dashboard visible**

  ![unnamed (3)](https://github.com/user-attachments/assets/34f4e3cb-8d26-47d9-9388-97b012263d1f)


**Integrating node exporter and alertmanager with prometheus**

Follow this steps for integrating prometheus node exporter and alertmanager

1\. Go to cd /etc/prometheus/prometheus.yml and add following content Using nano prometheus.yml

pallavee@pallavee:/etc/prometheus$ cat prometheus.yml

- my global config

global:

scrape\_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.

evaluation\_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.

- scrape\_timeout is set to the global default (10s).
- Alertmanager configuration alerting:

alertmanagers:

- static\_configs:
  - targets:
    - localhost:9093
- Load rules once and periodically evaluate them according to the global 'evaluation\_interval'. rule\_files:
  - - "rules1.yml"
    - "rules.yml"
  - - "second\_rules.yml"
- A scrape configuration containing exactly one endpoint to scrape:
- Here it's Prometheus itself.

scrape\_configs:

- The job name is added as a label `job=<job\_name>` to

any timeseries scraped from this config.

- job\_name: "prometheus"
- metrics\_path defaults to '/metrics'
- scheme defaults to 'http'.

static\_configs:

- targets: ["localhost:9090"]
- grafana
  - job\_name: "grafana"
- metrics\_path defaults to '/metrics'
- scheme defaults to 'http'.

static\_configs:

- targets: ["localhost:3000"]

#node-exporter

- job\_name: "node\_exporter"
- metrics\_path defaults to '/metrics'
- scheme defaults to 'http'.

static\_configs:

- targets: ["localhost:9100"]

AFter adding press ctrl+x Ctrl+y

Enter

**Now node exporter and alertmanager integrated with prometheus**

Add rule file for triggering alert

1. Cd /etc/prometheus
1. Ls
1. Nano rules.yml
1. Add follow content

   pallavee@pallavee:/etc/prometheus$ cat rules.yml groups:

- name: node\_exporter\_alerts

  rules:

- Alert for when Node Exporter is down
- alert: NodeExporterDown

  expr: up{job="node\_exporter"} == 0

  for: 1m

  labels:

severity: critical

priority: priority1

annotations:

summary: "Node Exporter Down (instance {{ $labels.instance }})"

description: "Node Exporter has been down for more than 1 minute."

status: "In progress"

assignee: "pallavee"

start\_date: "2024-07-22"

due\_date: "2024-07-30"

done\_ratio: "50"

- Alert for when CPU usage exceeds 80%
- alert: HighCPUUsage

expr: 100 - (avg by (instance) (rate(node\_cpu\_seconds\_total{mode="idle"}[5m])) \* 100) > 80

for: 1m

labels:

severity: warning

priority: priority2

annotations:

summary: "High CPU Usage (instance {{ $labels.instance }})"

description: "CPU usage has been above 80% for more than 1 minute."

status: "In progress"

assignee: "pallavee"

start\_date: "2024-07-22"

due\_date: "2024-07-30"

done\_ratio: "50"

- Alert for when disk space consumption exceeds

80%

- alert: HighDiskUsage

expr: (node\_filesystem\_size\_bytes{fstype!~"tmpfs|aufs|overl ay"} - node\_filesystem\_free\_bytes{fstype!~"tmpfs|aufs|overla y"}) / node\_filesystem\_size\_bytes{fstype!~"tmpfs|aufs|overla y"} > 0.8

for: 1m

labels:

severity: warning

priority: priority3

annotations:

summary: "High Disk Usage (instance {{ $labels.instance }})"

description: "Disk usage has been above 80% for more than 1 minute."

status: "In progress"

assignee: "pallavee"

start\_date: "2024-07-22"

due\_date: "2024-07-30"

done\_ratio: "50"

**Integrating webhook in alertmanager**

1. Cd /etc/alertmanager
1. Ls
1. Nano alertmanager.yml

pallavee@pallavee:/etc/alertmanager$ cat alertmanager.yml global:

resolve\_timeout: 5m

smtp\_smarthost: 'smtp.gmail.com:587'

smtp\_from: 'alertmanager52@gmail.com' smtp\_auth\_username: 'alertmanager52@gmail.com' smtp\_auth\_password: 'kotlsd'

smtp\_auth\_identity: 'alertmanager52@gmail.com'

route:

group\_by: ['alertname'] group\_wait: 30s group\_interval: 5m repeat\_interval: 3h receiver: 'email-receiver'

routes:

- receiver: 'webhook-receiver' matchers:
  - alertname=~".\*"

receivers:

- name: 'email-receiver'

  email\_configs:

- to: 'testpallaveetest@gmail.com' send\_resolved: true

  headers:

  Subject: '[{{ .CommonLabels.alertname }}]' html: |

<!DOCTYPE html>

<html>

<head>

</head>

<body>

<div class="content">

Tracker: Monitioring-tracker<br>

Status: {{ range .Alerts }}{{ .Annotations.status }}{{ end }}<br>

Priority: {{ range .Alerts }}{{ .Labels.priority }}{{ end }}<br>

Assignee: {{ range .Alerts }}{{ .Annotations.assignee }}{{ end }}<br>

Start date: {{ range .Alerts }}{{ .Annotations.start\_date }}{{ end }}<br>

Due date: {{ range .Alerts }}{{ .Annotations.due\_date }}{{ end }}<br>

Done ratio: {{ range .Alerts }}{{ .Annotations.done\_ratio }}{{ end }}<br>

Description: {{ range .Alerts }}{{ .Annotations.description }}{{ end }}<br>

</div>

</body>

</html>

- name: 'webhook-receiver'

  webhook\_configs:

- url: 'http://192.168.0.107:5100/api/v1/alerts' send\_resolved: true

Create

Create webhook service/script

1\. Set up a basic web server using python with flask

Pip install flask

nano alert\_webhook.py

And add following content

To run script use - Python3 alert\_webhook.py

Output

![unnamed (4)](https://github.com/user-attachments/assets/17049f59-7b41-437f-bb36-b6f9cfd71327)


