# Install Grafana Loki
References: [Link](https://github.com/grafana/helm-charts/blob/main/charts/grafana/README.md)

## Prerequisite Installation
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

## Install loki (only 1st installation)
```bash
helm install loki grafana/loki-distributed -f values.yaml -n monitoring
```

## If loki has existing please use `upgrade` instead of `install`
```bash
helm upgrade loki grafana/loki-distributed -f values.yaml -n monitoring
```