# Install Redis Exporter

$ export VER="1.3.1"

$ useradd --no-create-home --shell /bin/false redis_exporter

$ wget https://github.com/oliver006/redis_exporter/releases/download/v${VER}/redis_exporter-v${VER}.linux-amd64.tar.gz

$ tar -xf redis_exporter-v${VER}.linux-amd64.tar.gz

$ cp redis_exporter-v${VER}.linux-amd64/redis_exporter /usr/local/bin

$ chown redis_exporter:redis_exporter /usr/local/bin/redis_exporter

$ rm -rf redis_exporter-v${VER}.linux-amd64*



# Create the systemd unit file:

$ nano /etc/systemd/system/redis_exporter.service

#Content

[Unit]
Description=Redis Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=redis_exporter
Group=redis_exporter
Type=simple
ExecStart=/usr/local/bin/redis_exporter \
  --log-format=txt \
  --namespace=redis \
  --web.listen-address=:9121 \
  --web.telemetry-path=/metrics
SyslogIdentifier=redis_exporter
Restart=always

[Install]
WantedBy=multi-user.target


# Reload the systemd daemon and start node exporter:

$ systemctl daemon-reload
$ systemctl start redis_exporter

#View status
$ systemctl status redis_exporter

#If everything looks good, enable the service on boot:
$ systemctl enable redis_exporter


# Reference 
https://computingforgeeks.com/how-to-monitor-redis-server-with-prometheus-and-grafana-in-5-minutes/
