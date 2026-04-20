# recheck-monitoring

Prometheus + Grafana 모니터링 스택입니다.

## 구성

| 서비스 | 이미지 | 포트 |
|--------|--------|------|
| Prometheus | `prom/prometheus:latest` | 9090 |
| Grafana | `grafana/grafana:latest` | 3000 |

## 시작하기

### 사전 요구사항

- Docker
- Docker Compose

### 실행

```bash
# 스택 시작
docker compose -f docker/docker-compose.monitoring.yml up -d

# 스택 중지
docker compose -f docker/docker-compose.monitoring.yml down
```

### 접속

- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000

### Grafana 계정 설정

기본 계정(admin/admin) 대신 환경 변수로 설정할 수 있습니다.

```bash
GRAFANA_ADMIN_USER=admin \
GRAFANA_ADMIN_PASSWORD=your_password \
docker compose -f docker/docker-compose.monitoring.yml up -d
```

## 모니터링 대상

`prometheus/prometheus.yml`에서 스크랩 대상을 설정합니다.

| Job | 엔드포인트 | 설명 |
|-----|-----------|------|
| `node-exporter` | `172.31.9.253:9100/metrics` | 호스트 시스템 메트릭 |
| `spring-actuator` | `172.31.9.253:8080/actuator/prometheus` | Spring Boot 애플리케이션 메트릭 |

> 대상 IP(`172.31.9.253`)는 AWS EC2 프라이빗 IP입니다. Docker가 실행되는 환경에서 해당 IP에 접근 가능해야 합니다.

## 데이터 보존

- Prometheus 메트릭 보존 기간: **30일**
- 데이터는 Docker named volume(`prometheus-data`, `grafana-data`)에 저장됩니다.
