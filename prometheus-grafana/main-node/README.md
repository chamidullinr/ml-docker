# Prometheus and Grafana (main node)

Run the following command to set up Prometheus and Grafana applications, as well as node exporter:
```bash
docker-compose up -d
```
Note: Grafana is set up with default admin credentials (admin/admin).
User is requested to change admin password after first login.

## Configure and Test infrastructure
Run the following to reload Prometheus configuration after making changes in `prometheus.yml` file in [prometheus/prometheus.yml](./prometheus/prometheus.yml).
```bash
curl -X POST localhost:9090/-/reload
```

Run the following to send a test alert and confirm Slack integration works:
```bash
curl -H 'Content-Type: application/json' -d '[{"labels":{"alertname":"myalert"}}]' http://127.0.0.1:9093/api/v1/alerts
```
