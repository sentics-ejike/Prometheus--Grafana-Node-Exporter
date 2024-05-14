# Download the Node Exporter binary for Linux/WSL:
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz

`sudo apt update`
`sudo apt install docker.io`

`sudo systemctl start docker`
`sudo systemctl enable docker`
`sudo apt install docker-compose`

# Extract Node Exporter:
`tar xvfz node_exporter-1.8.0.linux-amd64.tar.gz`

# Move Node Exporter:
`sudo mv node_exporter-1.8.0.linux-amd64/node_exporter /usr/local/bin/`

# Verify Installation:
`/usr/local/bin/node_exporter`

# Access Node Exporter Metrics:
`curl http://localhost:9100/metrics`

# Ceate a directory prometheus

`mkdir prometheus`
`nano prometheus.yml`


```
global:
  scrape_interval: 1m

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']
  - job_name: 'node'
    static_configs:
      - targets: ['node_exporter:9100']

  ```

# Create a docker-compose configuration file
`nano docker-compose.yml`

  ```
version: '3.8'

volumes:
  prometheus-data:  {}
  grafana-data: {}

services:
  node_exporter:
    image: quay.io/prometheus/node-exporter:latest
    container_name: node_exporter
    command:
      - '--path.rootfs=/host'
    pid: host
    restart: unless-stopped
    ports:
      - '9100:9100'
    volumes:
      - '/:/host:ro'

  prometheus:
    image: prom/prometheus:latest  # Corrected to lowercase
    container_name: prometheus
    ports:
      - '9090:9090'
    restart: unless-stopped
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    command:
      - '--web.enable-lifecycle'
      - '--config.file=/etc/prometheus/prometheus.yml'
  grafana:
   image: grafana/grafana:latest
   container_name: grafana
   ports:
     - '3000:3000'
   restart: unless-stopped
   volumes:
    - grafana-data:/var/lib/grafana
 
  ```

# Starts all the services defined in the current directory
`docker compose up -d`

`https://youtu.be/jj38y6f6UpE?si=xeBPIt3rXBOZuKTF`
