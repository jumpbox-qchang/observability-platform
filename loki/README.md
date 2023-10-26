## Install Grafana Tempo
References: [Link](https://github.com/grafana/helm-charts/blob/main/charts/grafana/README.md)
```
helm repo add grafana https://grafana.github.io/helm-charts
```
```
helm install loki grafana/loki-distributed -f values.yaml -n monitoring
```