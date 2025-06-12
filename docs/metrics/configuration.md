## Prometheus Server

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  labels:
    app: deka-metric
    observability.cloudeka.ai/release: deka-metric
  name: deka-metric
  namespace: observability
spec:
  additionalScrapeConfigs:
    key: deka-metric-additional.yaml
    name: deka-metric-scrape-configs
  alerting:
    alertmanagers:
    - apiVersion: v2
      name: deka-alert
      namespace: observability
      pathPrefix: /
      port: http-web
  automountServiceAccountToken: true
  enableAdminAPI: false
  evaluationInterval: 30s
  externalUrl: http://deka-metric.observability:9090
  hostNetwork: false
  image: quay.io/prometheus/prometheus:v2.53.1
  listenLocal: false
  logFormat: logfmt
  logLevel: info
  nodeSelector:
    node-role.kubernetes.io/metrics: "true"
  paused: false
  podMonitorNamespaceSelector: {}
  podMonitorSelector:
    matchLabels:
      observability.cloudeka.ai/release: deka-metric
  portName: http-web
  probeNamespaceSelector: {}
  probeSelector:
    matchLabels:
      observability.cloudeka.ai/release: deka-metric
  prometheusExternalLabelName: stack
  replicaExternalLabelName: replica
  replicas: 3
  retention: 30d
  routePrefix: /
  ruleNamespaceSelector: {}
  ruleSelector:
    matchLabels:
      observability.cloudeka.ai/release: deka-metric
  scrapeConfigNamespaceSelector: {}
  scrapeConfigSelector:
    matchLabels:
      observability.cloudeka.ai/release: deka-metric
  scrapeInterval: 15s
  securityContext:
    fsGroup: 2000
    runAsGroup: 2000
    runAsNonRoot: true
    runAsUser: 1000
    seccompProfile:
      type: RuntimeDefault
  serviceAccountName: deka-metric
  serviceMonitorNamespaceSelector: {}
  serviceMonitorSelector:
    matchLabels:
      observability.cloudeka.ai/release: deka-metric
  shards: 1
  storage:
    volumeClaimTemplate:
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 500Gi
        storageClassName: storage-mgmt
  tolerations:
  - effect: NoSchedule
    key: node-role.kubernetes.io/metrics
    operator: Equal
    value: "true"
  tsdb:
    outOfOrderTimeWindow: 0s
  version: v2.53.1
  walCompression: true
```

## Prometheus Additional Scrape Config

```yaml
- job_name: 'kubernetes-apiservers'
  kubernetes_sd_configs:
    - role: endpoints
  scheme: https
  tls_config:
    ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
  bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
  relabel_configs:
    - source_labels: [__meta_kubernetes_namespace]
      regex: 'loft-.*'
      action: drop
    - source_labels: [__address__]
      regex: '172\.22\.38\.10:6443'
      action: keep
    - source_labels: [__meta_kubernetes_service_label_component]
      target_label: job
      action: replace
    - source_labels: []
      target_label: cluster_org
      replacement: 'cloudeka'
    - source_labels: []
      target_label: cluster_name
      replacement: 'tbs1-dev'
  metric_relabel_configs:
    - source_labels:
      - __name__
      - le
      action: drop
      regex: apiserver_request_duration_seconds_bucket;(0.15|0.2|0.3|0.35|0.4|0.45|0.6|0.7|0.8|0.9|1.25|1.5|1.75|2|3|3.5|4|4.5|6|7|8|9|15|25|40|50)
    - source_labels: [__name__]
      regex: '^(apiserver_requested_|apiserver_request_|workqueue_|process_resident_memory_|process_cpu_|go_goroutines).*'
      action: keep
```

!!! note
    - If you encounter any bugs or documentation errors, please send an email to `hernanta.kusuma@lintasarta.co.id`.