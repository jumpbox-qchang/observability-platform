# Installation Steps

## Prerequisite Step before installation grafana tempo
1. Add `nodepool` for observability stack and set naming: `qch-prod-gke-monitor-node-ase-private-ab`
the detail as shown below: 
- Enable cluster autoscaler: `true`, Location policy: `Balanced`, Size limits type: `Per zone limits`, Minimum: `1`, Maximum: `2`
- instance type: `c2-standard-4 (4 vCPU, 2 core, 16 GB memory)`
- Enable nodes on spot VMs: `true`
- Kubernetes labels: `nodepool: monitoring`
- Node taints: `nodepool=monitoring:NoSchedule`
- Labels: `nodepool: monitoring`

- another: let's everythings default
2. Create namespace on Kubernetes `monitoring`
```bash
kubectl create ns monitoring
```
3. Tools Installation
- [Helm3](https://helm.sh/docs/intro/install/)

## Install Grafana Tempo
References: [Link](https://github.com/grafana/helm-charts/blob/main/charts/grafana/README.md)
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
```
helm install tempo grafana/tempo-distributed -f values.yaml -n monitoring
```

## Install Grafana Dashboard
```
helm install grafana grafana/grafana -f values.yaml -n monitoring
```
## Forward Port Testing via Tunnel to Bastion Host
- Tunnel
```
gcloud compute ssh --zone "asia-southeast1-a" "qchang-prod-bastion-asia-southeast1-public-a" --project "qchang-prod"  -- -NL 3000:localhost:3000
```
- Expose Grafana Port: `3000`

## Apply Vitual service
- Grafana: `3100`
- Tempo Distributor: `4318`
