# observability-platform


## (Topic) Considered Options 
ใช้ GCS object storage ในการเก็บข้อมูล logs ในระยะยาว

## (Sub Topic) Context and Problem Statement

ค่าใช้จ่ายในการเก็บรักษาข้อมูล logs ในระยะยาว (1 ถึง 3 ปี)

## Considered Options

* GCS (Google Cloud Storage)
* Google BigQuery

## Decision Outcome

ตัวเลือกที่ตัดสินใจเลือก: "GCS (Google Cloud Storage)" เพราะ GCS มีค่าใช้จ่ายในการเก็บข้อมูล logs ในระยะยาว ที่มีราคาถูกกว่า Google BigQuery และปัจจุบัน Grafana Loki ได้มีการใช้งาน GCS ในการเก็บข้อมูล logs อยู่แล้ว ทำให้ไม่ต้องทำการตั้งค่าแหล่งเก็บข้อมูล logs ตัวใหม่
