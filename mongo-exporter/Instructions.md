# Install Mongo Exporter

$ export VER="0.10.0"

$ useradd --no-create-home --shell /bin/false mongodb_exporter

$ wget https://github.com/percona/mongodb_exporter/releases/download/v${VER}/mongodb_exporter-${VER}.linux-amd64.tar.gz

$ tar -xf mongodb_exporter-${VER}.linux-amd64.tar.gz

$ cp mongodb_exporter /usr/local/bin

$ chown mongodb_exporter:mongodb_exporter /usr/local/bin/mongodb_exporter

$ rm -rf mongodb_exporter*



# Create the systemd unit file:

$ nano /etc/systemd/system/mongodb_exporter.service

#Content

[Unit]
Description=MongoDb Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=mongodb_exporter
Group=mongodb_exporter
Type=simple
ExecStart=/usr/local/bin/mongodb_exporter \
  --mongodb.uri=mongodb://mongodb_exporter:password@localhost:27017 \
  --namespace=redis \
  --web.listen-address=:9121 \
  --web.telemetry-path=/metrics
SyslogIdentifier=mongodb_exporter
Restart=always

[Install]
WantedBy=multi-user.target


# Reload the systemd daemon and start node exporter:

$ systemctl daemon-reload
$ systemctl start mongodb_exporter

#View status
$ systemctl status mongodb_exporter

#If everything looks good, enable the service on boot:
$ systemctl enable mongodb_exporter


# Reference 
