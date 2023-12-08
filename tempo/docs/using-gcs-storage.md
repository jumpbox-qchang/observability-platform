## In case using external storage

# set up: services account

```bash
gcloud iam service-accounts create grafana-tempo-prd --project=qchang-prod
```

```bash
gcloud projects add-iam-policy-binding qchang-prod \
    --member "serviceAccount:grafana-tempo-prd@qchang-prod.iam.gserviceaccount.com" \
    --role "roles/storage.admin" --condition 'expression=resource.name.startsWith("projects/_/buckets/grafana-tempo-prd"),title=only-tempo,description=Reduce the binding scope to affect only buckets used by Tempo'
```

```bash
gcloud iam service-accounts add-iam-policy-binding grafana-tempo-prd@qchang-prod.iam.gserviceaccount.com --role roles/iam.workloadIdentityUser --member "serviceAccount:qchang-prod.svc.id.goog[monitoring/tempo]"
```

# for connection and permission testing (ในกรณี test ต้อง create service account แล้วแปะ annotation เอง)
```bash
gcloud compute ssh --zone "asia-southeast1-a" "qchang-prod-bastion-asia-southeast1-public-a" --project "qchang-prod" 
```

```bash
(คำสั่ง create service account) kubectl create sa test-wait-delete -n monitoring
```

```bash
(คำสั่งแปะ annotation) kubectl annotate serviceaccount test-wait-delete --namespace monitoring iam.gke.io/gcp-service-account=grafana-nonprd@qchang-dev.iam.gserviceaccount.com
```

```bash
vi workload-test.yaml (เข้าไปสร้างไฟล์ declaretive ไฟล์ที่ qchang non prod )
```

```bash
- ใส่ค่าข้างล่างนี้เข้าไปใน workload-test.yaml
```

```bash
apiVersion: v1
kind: Pod
metadata:
  name: workload-identity-test
  namespace: monitoring
spec:
  containers:
  - image: google/cloud-sdk:slim
    name: workload-identity-test
    command: ["sleep","infinity"]
  serviceAccountName: tempo
  nodeSelector:
    iam.gke.io/gke-metadata-server-enabled: "true"
```

พิมพ์คำสั่ง k9s 
