# Prometheus and Grafana

Resources:
[Grafana Docs](https://grafana.com/docs/grafana-cloud/quickstart/docker-compose-linux/)
and [Setup Alertmanager for Slack](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/)

Run the following commands to set up Prometheus and Grafana applications:
```bash
# set grafana admin user credentials
ADMIN_USER=admin
ADMIN_PASSWORD=admin
docker-compose up -d
```

## Test infrastructure
Run the following to send a test alert.
```bash
curl -H 'Content-Type: application/json' -d '[{"labels":{"alertname":"myalert"}}]' http://127.0.0.1:9093/api/v1/alerts
```
