# Observability Stack

A local observability stack using Docker Compose, wiring together logs, metrics, and traces into a single Grafana UI.

## Components

| Service | Role | Port |
|---|---|---|
| **Grafana** | Visualization UI | `3001` |
| **Prometheus** | Metrics scraping & storage | `9090` |
| **Loki** | Log aggregation | `3100` |
| **Promtail** | Log shipper (tail → Loki) | `9080` |
| **Tempo** | Distributed tracing (OTLP) | `4317` (gRPC), `3200` (HTTP) |

## Prerequisites

- Docker & Docker Compose

## Getting Started

```bash
docker compose up -d
```

Open Grafana at [http://localhost:3001](http://localhost:3001). All datasources (Tempo, Loki, Prometheus) are provisioned automatically.

## Configuration

| Path | Description |
|---|---|
| `prometheus/prometheus.yml` | Scrape targets (default: `host.docker.internal:3000/metrics`) |
| `promtail/config.yaml` | Log paths and Loki push target |
| `tempo/config.yaml` | OTLP receiver and local trace storage |
| `grafana/provisioning/datasources/` | Auto-provisioned Grafana datasources |

## Sending Data

**Traces** — send OTLP over gRPC to `localhost:4317`.

**Logs** — Promtail tails `../../goilerplate/storage/logs/*.log` and parses `level` and `label` fields from JSON log lines before shipping to Loki.

**Metrics** — Prometheus scrapes `/metrics` from `host.docker.internal:3000` every 15 seconds.

## Stopping

```bash
docker compose down
```
