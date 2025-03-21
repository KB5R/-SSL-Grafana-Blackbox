# SSL-Grafana-Blackbox
SSL Monitoring with Grafana, Prometheus, Blacbox Exporter

## Quick start

```
git clone https://github.com/KB5R/SSL-Grafana-Blackbox
docker compose up -d
```
**Проверить работу**
```
0.0.0.0:3000 Grafana
0.0.0.0:9090 Prometheus
0.0.0.0:9115 Blackbox-exporter
```
**Next, go to Prometheus and edit prometheus.yml**
```
scrape_configs:
  - job_name: 'blackbox_http'
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
          - https://www.google.com/
          - ...
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: blackbox:9115

  - job_name: 'blackbox-tcp'
    scrape_timeout: 15s
    scrape_interval: 15s
    metrics_path: /probe
    params:
      module: [tls_connect]
    static_configs:
      - targets:
          - www.kernel.org:443
          - ...
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: blackbox:9115

```
```
docker compose restart
```

**Grafana dashboards**
```
7587
```

### Works
<image src="./image/grafana_git.png">

