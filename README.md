# node-exporter
Prometheus Node Exporter

# download the Node Exporter binary for Linux, even though you're using WSL:
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz

# Extract Node Exporter:
tar xvfz node_exporter-1.8.0.linux-amd64.tar.gz

# Move Node Exporter:
sudo mv node_exporter-1.8.0.linux-amd64/node_exporter /usr/local/bin/

# Verify Installation: 
/usr/local/bin/node_exporter

# Access Node Exporter Metrics:
curl http://localhost:9100/metrics

# 
# 
# 
