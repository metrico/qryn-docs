# 🔻 Telemetry Ingestion

The following protocol APIs are supported for ingesting telemetry events:

<!-- tabs:start -->
## ** Grafana Agent **

<a id=grafana name=grafana></a>

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

Grafana Agent can act as a telemetry collector and receive spans from Jaeger, Kafka, OpenCensus, OTLP, and Zipkin to process and forward traces to **qryn** using the _Tempo API_

##### Example
In this example an `OTLP` collector is started on port `:4318` forwarding traces to **qryn** on port `:3100`

###### OTLP config.yaml _(cloud only)_
```
server:
  log_level: debug

traces:
  configs:
  - name: server_traces
    receivers:
      otlp:
        protocols:
          http:
            endpoint: "0.0.0.0:4318"

    remote_write:
      - endpoint: qryn:3100
        insecure: true
```

###### ZIPKIN config.yaml _(js)_
```
config.yaml: |
    receivers:
      jaeger:
        protocols:
          grpc:
            endpoint: 0.0.0.0:14250
          thrift_http:
            endpoint: 0.0.0.0:14268
          thrift_compact:
            endpoint: 0.0.0.0:6831
      otlp:
        protocols:
          grpc:
            endpoint: 0.0.0.0:4317
          http:
            endpoint: 0.0.0.0:4318

    processors:
      memory_limiter:
        check_interval: 1s
        limit_percentage: 70
        spike_limit_percentage: 30
      batch:
        send_batch_size: 100
        send_batch_max_size: 1000
        timeout: 10s
      resourcedetection:
        detectors: [env]
        timeout: 5s
        override: false
      k8sattributes:
        auth_type: 'serviceAccount'
      resource:
        attributes:
          - key: app
            action: insert
            from_attribute: k8s.deployment.name
          - key: pod_name
            action: insert
            from_attribute: k8s.pod.name
    exporters:
      zipkin:
        endpoint: http://qryn:3100/api/v2/spans
        tls:
          insecure: true
    service:
      pipelines:
        traces:
          receivers: [jaeger, otlp]
          processors: [memory_limiter, batch, resourcedetection, k8sattributes, resource]
          exporters: [zipkin]
```

The following articles provide great insight and examples on the subject:

- [Tracing with Grafana Agent](https://grafana.com/blog/2020/11/17/tracing-with-the-grafana-cloud-agent-and-grafana-tempo/)
- [Tracing K8s with Grafana Agent](https://grafana.com/docs/grafana-cloud/kubernetes-monitoring/agent-k8s/k8s_agent_traces/)

?> _That's it!_ You're now _tracing spans to **qryn** using Grafana Agent_! 



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
