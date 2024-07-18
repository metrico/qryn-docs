# 🔻 Telemetry Ingestion

The following protocol APIs are supported for ingesting telemetry events:

<!-- tabs:start -->
## ** Grafana Agent **

<a id=grafana name=grafana></a>

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

Grafana Agent can act as a telemetry collector and receive spans from [Jaeger, Kafka, OpenCensus, OTLP, and Zipkin](https://grafana.com/docs/agent/latest/configuration/traces-config/) to process and forward traces to **qryn** using the _Tempo API_

##### Example
In this example an `OTLP` collector is started on port `:4318` forwarding traces to **qryn** on port `:3100`

###### OTLP Exporter
```yml
server:
  log_level: debug

traces:
  configs:
  - name: server_traces
    receivers:
      zipkin:
      otlp:
        protocols:
          http:
            endpoint: '0.0.0.0:4318'
          grpc:
            endpoint: '0.0.0.0:4317'
    remote_write:
      - endpoint: qryn:3100
        insecure: true
        basic_auth:
         username: <qryn cloud Username>
         password: <qryn cloud API Key>
```

The following articles provide great insight and examples on the subject:

- [Tracing with Grafana Agent](https://grafana.com/blog/2020/11/17/tracing-with-the-grafana-cloud-agent-and-grafana-tempo/)
- [Tracing K8s with Grafana Agent](https://grafana.com/docs/grafana-cloud/kubernetes-monitoring/agent-k8s/k8s_agent_traces/)

###### Service Graphs

Service graphs are generated in Grafana Agent and pushed to qryn's prometheus compatible backend.

```yml
server:
  log_level: debug

traces:
  configs:
  - name: server_traces
    receivers:
      zipkin:
      otlp:
        protocols:
          http:
            endpoint: '0.0.0.0:4318'
          grpc:
            endpoint: '0.0.0.0:4317'
    remote_write:
      - endpoint: qryn:3100
        insecure: true
        basic_auth:
         username: <qryn cloud Username>
         password: <qryn cloud API Key>
    service_graphs:
        enabled: true
    scrape_configs:
      - job_name: local_scrape
        static_configs:
          - targets: ['127.0.0.1:12345']
        remote_write:
        - endpoint: qryn:3100
          insecure: true
          basic_auth:
            username: <qryn cloud Username>
            password: <qryn cloud API Key>
```

The following article provide great insight and examples on the subject:

- [Grafana APM Dashboard](https://grafana.com/docs/tempo/latest/metrics-generator/app-performance-mgmt/)

?> _That's it!_ You're now _tracing spans to **qryn** using Grafana Agent_! 


## ** OTEL Collector **

<a id=grafana name=grafana></a>

![image](https://user-images.githubusercontent.com/1423657/196469086-3d85efd5-7ef9-4d42-a677-5591470b7cae.png ':size=200')

The [OTEL Collector]([https://opentelemetry.io/docs/collector/](https://github.com/metrico/otel-collector)) offers a vendor-agnostic implementation of how to receive, process and export telemetry data. It removes the need to run, operate, and maintain multiple agents/collectors. 

This works with improved scalability and supports a large variety of open-source observability data formats _(e.g. Jaeger, Prometheus, Fluent Bit, etc.)_ and allows aggregating and sending traces to **qryn** using the _Tempo API_

For large scale ingestion, we suggest using the dedicated [qryn opentelemetry distribution](https://github.com/metrico/otel-collector) for ClickHouse.

### Examples

![image](https://user-images.githubusercontent.com/1423657/217615085-cb5c7858-798e-4896-8ad6-3b88cfb16c6c.png)


#### qryn collector
```
otel-collector:
    container_name: otel-collector
    image: ghcr.io/metrico/qryn-otel-collector:latest
    volumes:
      - ./otel-collector-config.yaml:/etc/otel/config.yaml
    ports:
      - "4317:4317"     # OTLP gRPC receiver
      - "4318:4318"     # OTLP HTTP receiver
      - "14250:14250"   # Jaeger gRPC
      - "14268:14268"   # Jaeger thrift HTTP
      - "9411:9411"     # Zipkin port
      - "24224:24224".  # Fluent Forward
    restart: on-failure
```
#### Inserts to ClickHouse

Insert logs, metrics, traces and profiling data directly to ClickHouse bypassing the qryn ingestion APIs.

###### qryn collector `config.taml`
The following example enables all the supported formats at once. Filter the required sections to build your configuration.

```yml
receivers:
  fluentforward:
    endpoint: 0.0.0.0:24224
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
  prometheus:
    config:
      scrape_configs:
        - job_name: 'otel-collector'
          scrape_interval: 5s
          static_configs:
            - targets: ['exporter:8080']
connectors:
  servicegraph:
    latency_histogram_buckets: [ 100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms ]
    dimensions: [ cluster, namespace ]
    store:
      ttl: 2s
      max_items: 1000
    cache_loop: 2m
    store_expiration_loop: 2s
    virtual_node_peer_attributes:
      - db.name
      - rpc.service
  spanmetrics:
    namespace: span.metrics
    exemplars:
      enabled: false
    dimensions_cache_size: 1000
    aggregation_temporality: 'AGGREGATION_TEMPORALITY_CUMULATIVE'
    metrics_flush_interval: 30s
    metrics_expiration: 5m
    events:
      enabled: false
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
  metricstransform:
    transforms:
      - include: calls_total
        action: update
        new_name: traces_spanmetrics_calls_total
      - include: latency
        action: update
        new_name: traces_spanmetrics_latency
exporters:
  qryn:
    dsn: tcp://clickhouse-server:9000/cloki?username=default&password=*************
    timeout: 10s
    sending_queue:
      queue_size: 100
    retry_on_failure:
      enabled: true
      initial_interval: 5s
      max_interval: 30s
      max_elapsed_time: 300s
    logs:
      format: raw
  otlp/spanmetrics:
    endpoint: localhost:4317
    tls:
      insecure: true
extensions:
  health_check:
  pprof:
  zpages:

service:
  extensions: [pprof, zpages, health_check]
  pipelines:
    logs:
      receivers: [fluentforward, otlp]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
    traces:
      receivers: [otlp, jaeger, zipkin]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn, spanmetrics, servicegraph]
    metrics:
      receivers: [prometheus, spanmetrics, servicegraph]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
```

#### Inserts to API

Insert logs, metrics, traces and profiling data in ClickHouse through the qryn ingestion APIs.

###### qryn collector `config.yaml`
The following example outlines the usage of exporters with the qryn or qryn.cloud endpoints.
```
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
        - job_name: 'otel-collector'
          scrape_interval: 5s
          static_configs:
            - targets: ['exporter:8080']
connectors:
  servicegraph:
    latency_histogram_buckets: [ 100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms, 500ms, 1000ms, 1500ms, 2000ms, 5s, 10s, 30s, 60s ]
    dimensions: [ cluster, namespace ]
    store:
      ttl: 2s
      max_items: 1000
    cache_loop: 2m
    store_expiration_loop: 2s
    virtual_node_peer_attributes:
      - db.name
      - rpc.service
  spanmetrics:
    namespace: span.metrics
    exemplars:
      enabled: false
    dimensions_cache_size: 1000
    aggregation_temporality: 'AGGREGATION_TEMPORALITY_CUMULATIVE'
    metrics_flush_interval: 30s
    metrics_expiration: 5m
    events:
      enabled: false
processors:
  attributes:
    actions:
      - action: insert
        key: loki.attribute.labels
        value: sender
extensions:
  health_check:
  pprof:
  zpages:
  memory_ballast:
    size_mib: 1000
exporters:
  otlphttp:
    endpoint: https://<API_KEY>:<API_SECRET>@qryn.gigapipe.com
    timeout: 30s
  loki:
    endpoint: https://<API_KEY>:<API_SECRET>@qryn.gigapipe.com/loki/api/v1/push
    timeout: 30s
  prometheusremotewrite:
    endpoint: https://<API_KEY>:<API_SECRET>@qryn.gigapipe.com/prom/remote/write
    timeout: 30s
service:
  extensions: [pprof, zpages, health_check]
  pipelines:
    traces:
      receivers: [zipkin]
      exporters: [otlphttp, spanmetrics, servicegraph]
    logs:
      receivers: [fluentforward, otlp]
      processors: [attributes]
      exporters: [loki]
    metrics:
      receivers: [prometheus, spanmetrics, servicegraph]
      exporters: [prometheusremotewrite]
```


?> _That's it!_ You're now _tracing spans to **qryn** using OTLP Collector! 


## ** Zipkin **

<a id=zipkin name=zipkin></a>

![image](https://user-images.githubusercontent.com/1423657/184494381-15d20f5d-3d52-411b-9064-dfd2ccea7c1c.png ':size=100')![image](https://user-images.githubusercontent.com/1423657/184494438-17d7ceb0-a62a-4819-9b1c-43d7f0baf802.png ':size=100')


?> **qryn** natively implements a json/http [Zipkin](https://zipkin.io/) API endpoint to ingest tracing spans.

Tracing is possible from any application supported by the [opentelemetry](https://github.com/open-telemetry) libraries.

- [Opentelemetry Zipkin example](https://github.com/open-telemetry/opentelemetry-js/tree/main/examples/opentelemetry-web/examples/zipkin)
- [Zipkin Tracing Instrumentations](https://zipkin.io/pages/tracers_instrumentation.html)
- [Instrumenting a Library](https://zipkin.io/pages/instrumenting.html)

?> _That's it!_ You're now _tracing spans to **qryn** using Zipkin! 


## ** ClickHouse MV **

<a id=clickhouse name=clickhouse></a>

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=100 />

ClickHouse creates trace spans for each query and some of the query execution stages, such as query planning or distributed queries.
Using MVs the internal spans can be translated to ZipkinV2 JSON and sent to an OTLP collector such as qryn, jaeger or prometheus.

### Requirements
- Enable [opentelemetry](https://clickhouse.com/docs/en/operations/opentelemetry/) settings in your ClickHouse `config.xml`


### Create Materialized View

The following MV will push traces to the qryn /tempo API using the ClickHouse **URL engine**
```sql
CREATE MATERIALIZED VIEW default.zipkin_spans
ENGINE = URL('http://qryn:3100/tempo/api/push', 'JSONEachRow')
SETTINGS output_format_json_named_tuples_as_objects = 1,
    output_format_json_array_of_rows = 1 AS
SELECT
    lower(hex(reinterpretAsFixedString(trace_id))) AS traceId,
    lower(hex(parent_span_id)) AS parentId,
    lower(hex(span_id)) AS id,
    operation_name AS name,
    start_time_us AS timestamp,
    finish_time_us - start_time_us AS duration,
    cast(tuple('clickhouse'), 'Tuple(serviceName text)') AS localEndpoint,
    cast(tuple(
        attribute.values[indexOf(attribute.names, 'db.statement')]),
        'Tuple("db.statement" text)') AS tags
FROM system.opentelemetry_span_log
```

!> Replace the **qryn** URL from the example to match your actual deployment!


### Send Spans from ClickHouse

Enable tracing as you login with the clickhouse-client (Value between 0 (turned off) and 1 (100% of queries create trace))
```bash
clickhouse-client -h 127.0.0.1 --opentelemetry_start_trace_probability=0.1 --query_id "a0c9f73a1c684e0ce66333477a3841bf-200" --query "select 1"
clickhouse-client -h 127.0.0.1 --query "system flush logs"

```

The following type events will be pushed:
```json
[{
 traceID: '97a156e95095e0e4c0f0f4dbb921244d',
 spanID: '19ce8d73f029e649',
 operationName: 'TCPHandler',
 references: [],
 startTime: '1641319910142576',
 startTimeUnixNano: 1641319910142576000,
 endTimeUnixNano: 1,
 duration: 12,
 tags: [ { key: 'db.statement', value: '', type: 'string' } ],
 logs: [],
 processID: 'p1',
 warnings: null,
 localEndpoint: { serviceName: 'clickhouse' },
 parentSpanID: '9654a4d5336d4c3e'
}]
```

?> _That's it!_ You're now tracing _ClickHouse using ClickHouse_! 


## ** Curl **

No API guide would be complete without a Curl example - _and we're no exception!_


#### Insert Traces

Trace from your console for testing and to support telemetry within your bash scripts


Send a root span with a `parentId`
```
curl -X POST http://localhost:3100/tempo/api/push -H 'Content-Type: application/json' -d '[{
 "id": "1234",
 "traceId": "d6e9329d67b6146b",
 "timestamp": '$(date +%s%N | cut -b1-16)',
 "duration": 1000,
 "name": "1st span from bash!",
 "tags": {
    "http.method": "POST",
    "http.path": "/parent"
  },
  "localEndpoint": {
    "serviceName": "shell script child 1"
  }
}]'
```

Send a child span pointing to the `parentId` span
```
curl -X POST http://localhost:3100/tempo/api/push -H 'Content-Type: application/json' -d '[{
 "id": "9101DEF",
 "traceId": "d6e9329d67b6146b",
 "parentId": "1234",
 "timestamp": '$(date +%s%N | cut -b1-16)',
 "duration": 500,
 "name": "2nd child span from bash!",
 "tags": {
    "http.method": "POST",
    "http.path": "/child"
  },
  "localEndpoint": {
    "serviceName": "shell script child 2"
  }
}]'
```

!> Replace the **qryn** URL from the example to match your actual deployment!

### Screenshot
##### Tempo Spans
![image](https://user-images.githubusercontent.com/1423657/147878090-a7630467-433e-4912-a439-602ce719c21d.png)
##### Tempo Logs
![image](https://user-images.githubusercontent.com/1423657/147878505-4136912b-e8b4-492d-b98f-7137dfeee015.png)



?> _That's it!_ You're now _tracing spans to **qryn** using just curl_! 


<!-- tabs:end -->
