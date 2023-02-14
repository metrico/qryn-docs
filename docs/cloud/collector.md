<!-- ![image](https://user-images.githubusercontent.com/1423657/197589119-bb790fba-dd50-412c-92c1-033675fa980a.png ':size=200') -->
<a href="https://app.gigapipe.com/signup?ref=qxip" target="_blank">
  <!-- <img src="https://user-images.githubusercontent.com/1423657/200078144-5d0b0960-2ad8-4b0c-9cdd-b7f8f9f516ae.png" width=500 /> -->
  <img src="https://user-images.githubusercontent.com/1423657/200078554-f8352174-9a6b-4f4a-90fc-1c6521d46c5b.png" width=600 />  
</a>

# ☁️  [qryn-cloud-collector](/cloud)

!> qryn-cloud-collector requires activation & pull tokens! Please [contact us](mailto:info@qxip.net) to obtain a license.

## 📦 Installation

Let's install **qryn-cloud-collector** in a snap. All you need is a supported OS or `Docker` installed.

Stack configuration passed to the process or container with a yml configuration file, detailed below.

<!-- tabs:start -->

#### ** Docker **

![image](https://user-images.githubusercontent.com/1423657/184507884-624b9598-62e1-413f-854e-8210ecac4e75.png ':size=300x100')

Install `qryn-cloud-collector` on your system using `docker` and `docker-compose`

##### Log into the Container Hub

```bash
docker login
```

Use the provided _remote token_ to access to the container repository instead of your password

```bash
echo {providedPullToken} | docker login -u Username --password-stdin
```

##### Pull the containers
```bash
docker pull qxip/qryn-cloud-collector
```

##### Create Docker Compose File

Define a docker compose file to configure each element.

```yml
version: '3.1'

services:
   qryn-collector:
    container_name: qryn-collector
    image: qxip/qryn-cloud-collector:latest
    volumes:
      - ./qryn-collector-config.yaml:/etc/otel/config.yaml
    ports:
      - "4317:4317"     # OTLP gRPC receiver
      - "4318:4318"     # OTLP HTTP receiver
      - "14250:14250"   # Jaeger gRPC
      - "14268:14268"   # Jaeger thrift HTTP
      - "9411:9411"     # Zipkin port
      - "24224:24224"   # Fluent Forward
      - "8889:8889"     # Prometheus Exporter
    restart: on-failure
```

Create the qryn-collector-config.yaml file

?> Adjust the clickhouse server and authentication details to match your setup

```yml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
  jaeger:
    protocols:
      grpc:
        endpoint: 0.0.0.0:14250
      thrift_http:
        endpoint: 0.0.0.0:14268
  zipkin:
    endpoint: 0.0.0.0:9411
  fluentforward:
    endpoint: 0.0.0.0:24224
  prometheus:
    config:
      scrape_configs:
        - job_name: 'qryn-collector'
          scrape_interval: 5s
          static_configs:
            - targets: ['localhost:8080']
processors:
  batch:
    send_batch_size: 10000
    timeout: 5s
  memory_limiter:
    check_interval: 2s
    limit_mib: 1800
    spike_limit_mib: 500
  resourcedetection/system:
    detectors: ['system']
    system:
      hostname_sources: ['os']
  resource:
    attributes:
      - key: service.name
        value: "serviceName"
        action: upsert
  spanmetrics:
    metrics_exporter: otlp/spanmetrics
    latency_histogram_buckets: [100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms]
    dimensions_cache_size: 1500
  servicegraph:
    metrics_exporter: otlp/spanmetrics
    latency_histogram_buckets: [100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms]
    dimensions: [cluster, namespace]
    store:
      ttl: 2s
      max_items: 200
  metricstransform:
    transforms:
      - include: calls_total
        match_type: strict
        action: update
        new_name: traces_spanmetrics_calls_total
        operations:
          - action: update_label
            label: operation
            new_label: span.name
          - action: update_label
            label: service.name
            new_label: service
      - include: latency
        match_type: strict
        action: update
        new_name: traces_spanmetrics_latency
exporters:
  qryn:
    dsn: n-tcp://clickhouse.ip:9000/qryn?username=default&password=PASSW0RD
    timeout: 10s
    sending_queue:
      queue_size: 100
    retry_on_failure:
      enabled: true
      initial_interval: 5s
      max_interval: 30s
      max_elapsed_time: 300s
  otlp/spanmetrics:
    endpoint: "http://0.0.0.0:4317"
    tls:
      insecure: true
  prometheus:
    endpoint: "0.0.0.0:8889"
extensions:
  health_check:
  pprof:
  zpages:
  memory_ballast:
    size_mib: 1000
service:
  extensions: [pprof, zpages, health_check]
  pipelines:
    logs:
      receivers: [fluentforward, otlp]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn, logging]
    traces:
      receivers: [otlp, jaeger, zipkin]
      processors: [memory_limiter, resourcedetection/system, resource, spanmetrics, servicegraph, batch]
      exporters: [qryn]
    metrics:
      receivers: [otlp]
      processors: [memory_limiter, resourcedetection/system, resource, metricstransform, batch]
      exporters: [prometheus]
    metrics/spanmetrics:
      receivers: [otlp]
      exporters: [otlp/spanmetrics]
```

##### Run Compose
```bash
docker compose up -d
docker compose ps
```

?> That's it! You are ready to access the stack using [qryn-view or Grafana](getting-started.md)

?> For an overview of all enviroment settings go [here](/cloud/env.md) and for configurations go [here](/cloud/config.md)

<!-- tabs:end -->
