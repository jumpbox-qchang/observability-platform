# Install Grafana Agent

## Prerequisite Installation
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

## Install grafana-agent (only 1st installation)
```bash
helm install grafana-agent grafana/grafana-agent -f values.yaml -n monitoring
```

## If grafana-agent has existing please use `upgrade` instead of `install`
```bash
helm upgrade grafana-agent grafana/grafana-agent -f values.yaml -n monitoring
```