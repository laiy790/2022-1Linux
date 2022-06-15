# Grafana+Pushgateway

gedit /etc/yum.repos.d/grafana.repo

```c
[grafana]
name = grafana
baseurl = https://packages.grafana.com/oss/rpm
repo_gpgcheck = 1
enabled = 1
gpgcheck = 1
gpgkey = http://packages.grafana.com/gpg.key
sslverify = 1
sslcacert = /etc/pki/tls/certs/ca-bundle.crt
```

yum install grafana

systemctl daemon-reload

systemctl start grafana-server

網頁:

192.168.23.@@:3000

帳號:admin

密碼:admin

Data Sources→Configuration→Prometheus

URL→IP+戶號

Create → import upload 

點擊Prometheus

---

wget https://github.com/prometheus/pushgateway/releases/download/v1.4.2/pushgateway-1.4.2.linux-amd64.tar.gz

tar xvfz pushgateway-1.4.2.linux-amd64.tar.gz

mv pushgateway-1.4.2.linux-amd64 pushgateway

mv pushgateway /opt/module/

gedit /usr/lib/systemd/system/pushgateway.service

```c
[Unit]
Description=pushgateway
After=network.target

[Service]
Type=simple
ExecStart=/opt/module/pushgateway/pushgateway 
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

systemctl daemon-reload

systemctl start pushgateway

cd ../prometheus

gedit prometheus.yml

```c
- job_name: 'pushgateway'
    honor_labels: true
    static_configs:
      - targets: ['192.168.23.@@:9091']
```

systemctl restart prometheus

網頁Status→target

---