# Install Grafana Agent

## Prerequisite Installation
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

## Install grafana (only 1st installation)
```bash
helm install grafana grafana/grafana -f values.yaml -n monitoring
```

## If grafana has existing please use `upgrade` instead of `install`
```bash
helm upgrade grafana grafana/grafana -f values.yaml -n monitoring
```