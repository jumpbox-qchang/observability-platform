# Architecture Decidion Diagram

## (Topic) กำหนด query command ของ config Tempo datasource ใน  Grafana Dashboard 

## (Sub Topic) Context and Problem Statement

### เงื่อนไขการ query command สำหรับการ navigate trace to log 

## Considered Options

### * query command เริ่มด้วย 
### ( {instance="${__span.instance}"} |= `traceID` | json | traceID = "${__trace.traceId}" )

### * query command เริ่มด้วย 
### ( {job="loki.source.kubernetes.pods"} |= `traceID` | json | traceID = "${__trace.traceId}" )

## Decision Outcome

### ตัวเลือกที่ตัดสินใจเลือก: คือ query command เริ่มด้วย 
### ( {job="loki.source.kubernetes.pods"} |= `traceID` | json | traceID = "${__trace.traceId}" )
### เนื่องจาก trace ไม่ได้มีการส่งข้อมูล instance name มากับ attributes ดังนั้นจึงจำเป็นต้อง query จาก job ของ log แทน