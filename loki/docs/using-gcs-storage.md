## In case using external storage

# set up: bucket 

```bash
gcloud storage buckets create gs://grafana-loki-prd --project=qchang-prod --default-storage-class=STANDARD --location=ASIA-SOUTHEAST1 --uniform-bucket-level-access --public-access-prevention
```

```bash
gcloud storage buckets update gs://grafana-loki-prd --project=qchang-prod --update-labels=env=prod
```


# set up: services account

```bash
gcloud iam service-accounts create grafana-loki-prd --project=qchang-prod
```

```bash
gcloud projects add-iam-policy-binding qchang-prod \
    --member "serviceAccount:grafana-loki-prd@qchang-prod.iam.gserviceaccount.com" \
    --role "roles/storage.admin" --condition 'expression=resource.name.startsWith("projects/_/buckets/grafana-loki-prd"),title=only-loki,description=Reduce the binding scope to affect only buckets used by Loki'
```

```bash
gcloud iam service-accounts add-iam-policy-binding grafana-loki-prd@qchang-prod.iam.gserviceaccount.com --role roles/iam.workloadIdentityUser --member "serviceAccount:qchang-prod.svc.id.goog[monitoring/loki]"
```

# for connection and permission testing (ในกรณี test ต้อง create service account แล้วแปะ annotation เอง)
```bash
gcloud compute ssh --zone "asia-southeast1-a" "qchang-prod-bastion-asia-southeast1-public-a" --project "qchang-prod" 
```

```bash
(คำสั่ง create service account) kubectl create sa test-wait-delete -n monitoring
```

```bash
(คำสั่งแปะ annotation) kubectl annotate serviceaccount test-wait-delete --namespace monitoring iam.gke.io/gcp-service-account=grafana-prd@qchang-prod.iam.gserviceaccount.com
```

```bash
vi workload-test.yaml (เข้าไปสร้างไฟล์ declaretive ไฟล์ที่ qchang prod )
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
  serviceAccountName: loki
  nodeSelector:
    iam.gke.io/gke-metadata-server-enabled: "true"
```

พิมพ์คำสั่ง k9s 
