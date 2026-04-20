# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

Prometheus + Grafana monitoring stack for the ReCheck Spring Boot application. This is a pure infrastructure config repo — no application code.

## Running the Stack

```bash
# Start from repo root
docker compose -f docker/docker-compose.monitoring.yml up -d

# Stop
docker compose -f docker/docker-compose.monitoring.yml down
```

- Prometheus UI: `http://localhost:9090`
- Grafana UI: `http://localhost:3000` (default: admin/admin — set via `GRAFANA_ADMIN_USER` / `GRAFANA_ADMIN_PASSWORD` env vars)

## Architecture

- **`docker/docker-compose.monitoring.yml`** — defines Prometheus and Grafana containers with named volumes (`prometheus-data`, `grafana-data`) and 30-day metric retention
- **`prometheus/prometheus.yml`** — scrape config targeting `172.31.9.253` (AWS private IP) for two jobs:
  - `node-exporter` on port `9100` — host-level system metrics
  - `spring-actuator` on port `8080/actuator/prometheus` — Spring Boot app metrics

## Known Issue

The `docker-compose.monitoring.yml` volume mount references `../monitor/prometheus.yml` but the actual file is at `../prometheus/prometheus.yml`. This path mismatch will cause Prometheus to start without the correct scrape config.
