## In case using external storage

# set up: services account

- gcloud iam service-accounts create grafana-tempo-prd --project=qchang-prod

- gcloud projects add-iam-policy-binding qchang-prod \
    --member ""serviceAccount:grafana-tempo-prd@qchang-prod.iam.gserviceaccount.com"" \
    --role ""roles/storage.objectAdmin"" --condition 'expression=resource.name.startsWith(""projects/_/buckets/grafana-tempo-prd""),title=only-tempo,description=Reduce the binding scope to affect only buckets used by Tempo'"

- gcloud iam service-accounts add-iam-policy-binding grafana-tempo-prd@qchang-prod.iam.gserviceaccount.com --role roles/iam.workloadIdentityUser --member "serviceAccount:qchang-prod.svc.id.goog[monitoring/tempo]"

# for connection and permission testing
- gcloud compute ssh --zone "asia-southeast1-a" "qchang-prod-bastion-asia-southeast1-public-a" --project "qchang-prod" 

- vi workload-test.yaml (เข้าไปสร้างไฟล์ declaretive ไฟล์ที่ qchang non prod )

- ใส่ค่าข้างล่างนี้เข้าไปใน workload-test.yaml

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

- พิมพ์คำสั่ง k9s 