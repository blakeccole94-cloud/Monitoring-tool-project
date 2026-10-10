# Monitoring-tool-project
# Monitoring Stack (Cloud+ Project H)

A Prometheus + Grafana monitoring stack running in Docker on an Ubuntu Server VM (Hyper-V), with a CPU alert rule tested under load.

## What's in it
- **node-exporter**: exposes the VM's CPU, memory, disk and network metrics
- **Prometheus**: scrapes node-exporter every 15 seconds and stores the data
- **Grafana**: dashboards (Node Exporter Full, ID 1860) and alerting

## How to run it
1. Create a `.env` file next to `docker-compose.yml`:
   `GRAFANA_ADMIN_PASSWORD=<your-password>`
2. Start the stack: `docker compose up -d`
3. Prometheus: `http://<vm-ip>:9090` | Grafana: `http://<vm-ip>:3000`
4. In Grafana, add a Prometheus data source with URL `http://prometheus:9090`

## Alert test
Rule: CPU usage above 80% for 2 minutes. I ran `stress-ng --cpu 0 --timeout 420s` and watched the rule go Normal, then Pending, then Firing, then back to Normal. Screenshots are in `screenshots/`.

## Lessons learned
- A YAML parse error crash-looped Prometheus until I fixed `prometheus.yml`; the container logs pointed at the exact line.
- `.env` is gitignored so the Grafana password never reaches the repo.
