# Webex Room Kit Mini: Grafana + Prometheus

Dieses Paket ist für den Teilnehmer gedacht, der Grafana und Prometheus betreibt.

## Inhalt

```text
grafana-prometheus-roomkit/
├── docker-compose.yml
├── .env.example
├── README.md
├── prometheus/
│   ├── prometheus.yml
│   └── alert.rules.yml
└── grafana/
    ├── provisioning/
    │   ├── datasources/
    │   │   └── prometheus.yml
    │   └── dashboards/
    │       └── dashboards.yml
    └── dashboards/
        └── roomkit-health.json
```

## Start

```bash
cp .env.example .env
docker compose up -d
```

## Wichtige Anpassung

In `prometheus/prometheus.yml` muss die IP des Python-Rechners angepasst werden:

```yaml
targets:
  - "192.168.3.100:8000"
```

Der Python-Teilnehmer stellt den Metrics-Endpunkt bereit:

```text
http://<python-rechner-ip>:8000/metrics
```

## Zugriff

Grafana:

```text
http://<grafana-rechner-ip>:3000
```

Standard-Login:

```text
admin / admin
```

Prometheus:

```text
http://<grafana-rechner-ip>:9090
```

## Erwartete Metriken

Das Dashboard erwartet beispielhaft diese Prometheus-Metriken:

```text
roomos_device_online
roomos_health_score
roomos_call_connected
roomos_microphones_muted
roomos_ethernet_speed_mbps
roomos_packet_loss
roomos_jitter_ms
roomos_temperature_alarm
roomos_standby_state
```

## Test

Auf dem Prometheus/Grafana-Rechner:

```bash
curl http://<python-rechner-ip>:8000/metrics
```

In Prometheus prüfen:

```text
Status -> Targets
```

Der Job `roomkit-monitor` sollte `UP` sein.
