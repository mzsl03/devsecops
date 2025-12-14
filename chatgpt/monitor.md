## 1. Proxmox virtuális hálózat kialakítása

Cél: legyen egy elkülönített LAB környezet.

### Proxmox oldalon

- Hozz létre egy Linux Bridge-et (pl. vmbr1)

- Bridge mód: internal / isolated

- Nem kell külső internet (oktatási környezethez ideális)

### VM-ek

Ajánlott felosztás:

- VM1 – Monitoring VM
  - Prometheus
  - Grafana
  - Node-exporter
  - cAdvisor
- VM2 – Logging VM
  - Elasticsearch
  - Kibana
  - Filebeat

Így szépen szétválik a monitoring és a logging, ami DevSecOps szempontból is helyes.

## 2. Operációs rendszer (mindkét VM-en)

Ajánlott:
- Ubuntu Server 22.04 LTS

Telepítés után:
```
sudo apt update && sudo apt upgrade -y
```

Docker + Docker Compose:

```
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

## 3. Projektstruktúra (NE módosítsd a fájlokat)
Monitoring VM
```
/opt/monitoring
├── docker-compose.yml     # Prometheus + Grafana + exporters
└── prometheus.yml
```
```
Logging VM
/opt/logging
├── docker-compose.yml     # Elasticsearch + Kibana + setup
├── .env
└── filebeat.yml
```

A **YAML**-ok tartalmát pontosan úgy hagyd, ahogy elküldted.

## 4. Environment fájl (.env) – csak értékek

Ez nem YAML-módosítás, csak szükséges konfiguráció.

```
STACK_VERSION=8.12.0
CLUSTER_NAME=devsecops-cluster
ES_PORT=9200
KIBANA_PORT=5601
ELASTIC_PASSWORD=StrongElasticPass123
KIBANA_PASSWORD=StrongKibanaPass123
MEM_LIMIT=1g
LICENSE=basic
```

## 5. Monitoring stack indítása (VM1)

```
cd /opt/monitoring
docker-compose up -d
```

Ellenőrzés:

```
docker ps
```

Elérhetőség:
- Prometheus: http://VM1_IP:9090
- Grafana: http://VM1_IP:3000
  - user: devsecops
  - pass: password1234

## 6. Logging stack indítása (VM2)

```
cd /opt/logging
docker-compose up -d
```

Ez:
- tanúsítványokat generál
- Elasticsearch security-t állít be
- Kibana system user jelszót beállít

Ellenőrzés:

```
docker ps
```

Elérhetőség:
- Kibana: http://VM2_IP:5601

## 7. Filebeat használata (log pipeline)

A megadott `filebeat.yml`:
- NDJSON logokat olvas (/logs/*.json)
- Elasticsearch-be küldi
- Oktatáshoz jó példa:
  - strukturált logolás
  - security pipeline
  - centralizált logging

Indítás:
```
docker run -d \
  --name filebeat \
  --network host \
  -v $(pwd)/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro \
  -v /var/log:/logs:ro \
  docker.elastic.co/beats/filebeat:8.12.0
```

## 8. Grafana + Prometheus összekötése (óra közbeni demo)

Grafanában:
- Data source → Prometheus
- URL: http://prometheus:9090

Készen:
- node-exporter → OS metrikák
- cAdvisor → konténerek
- Prometheus → scraping
- Grafana → vizualizáció

## 9. DevSecOps fókusz – mit lehet bemutatni órán?

✅ Infrastructure as Code (docker-compose)

✅ Observability (metrics + logs)

✅ Security by default
- TLS az Elasticsearchben
- Authentication
- Centralizált naplózás

✅ Separation of concerns
- monitoring vs logging

✅ Production-grade stack (ELK + Prometheus)