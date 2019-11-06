# Install Node Exporter
```
useradd --no-create-home --shell /bin/false node_exporter

wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz

tar -xf node_exporter-0.18.1.linux-amd64.tar.gz

cp node_exporter-0.18.1.linux-amd64/node_exporter /usr/local/bin

chown node_exporter:node_exporter /usr/local/bin/node_exporter

rm -rf node_exporter-0.18.1.linux-amd64*
```

# Setup Autorun
## Create the systemd unit file:
```
nano /etc/systemd/system/node_exporter.service
```
#Content
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

## Reload the systemd daemon and start node exporter:
```
systemctl daemon-reload
systemctl start node_exporter
```

View status
```
systemctl status node_exporter
```

If everything looks good, enable the service on boot:
```
systemctl enable node_exporter
```

# Reference 
https://blog.ruanbekker.com/blog/2019/05/07/setup-prometheus-and-node-exporter-on-ubuntu-for-epic-monitoring/
