# Prometheus and Grafana

The project contains configuration to set up and deploy docker containers with Prometheus, Grafana, and exporters.
Exporters collect and provide metrics to Prometheus.

Resources:
* [Docker compose guide](https://grafana.com/docs/grafana-cloud/quickstart/docker-compose-linux/) in Grafana Docs
* [Docker-Compose-Prometheus-and-Grafana](https://github.com/Einsteinish/Docker-Compose-Prometheus-and-Grafana) GitHub repository
* [Prometheus Exporters](https://prometheus.io/docs/instrumenting/exporters/)
* [Alertmanager Configuration](https://prometheus.io/docs/alerting/latest/configuration/)
* [Setup Alertmanager for Slack](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/)
  * Note: **Incoming Webhooks** in Slack app directory are deprecated. Instead, create a custom Slack App with a webhook.  

## Main Node
The main node contains Prometheus and Grafana applications, as well as node exporter
which collects metrics about the node (server).
See [deployment description](./main-node/README.md)
and [Prometheus configuration](./main-node/prometheus/prometheus.yml) with a list of scrape configurations.

## Worker Node
The worker node contains node exporter which collects metrics about the node (server).
See [deployment description](./worker-node/README.md).

## Exporters

System exporters:
* [Node exporter](https://github.com/prometheus/node_exporter) -
exports hardware and OS metrics.
* [cAdvisor](https://github.com/google/cadvisor) -
exports information about running containers.
* [NVIDIA GPU exporter](https://github.com/mindprince/nvidia_gpu_prometheus_exporter) -
exports NVIDIA GPU metrics.

Application exporters:
* [Python FastAPI exporter](https://github.com/trallnag/prometheus-fastapi-instrumentator) -
exports HTTP request metrics like number of requests, request size, response size, and request duration. 

Unstructured log exporters:
* [Grok exporter](https://github.com/fstab/grok_exporter)
* [Fluendt exporter](https://github.com/V3ckt0r/fluentd_exporter)
* [mtail exporter](https://github.com/google/mtail)

Applications with build-in exporters:
* [Docker Daemon](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-metrics) -
Requires one or more Docker engines are joined into a Docker swarm. See [docs](https://docs.docker.com/config/daemon/prometheus/).
* [GitLab](https://docs.gitlab.com/ee/administration/monitoring/prometheus/gitlab_metrics.html) -
requires admin user to enable Prometheus metrics.
