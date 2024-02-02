# 🔻 Log Ingestion

The following protocols and APIs integrations are supported for ingesting Logs:

<!-- tabs:start -->

## ** Loki **

?> **qryn** implements a full **Loki/LogQL** API to ingest, query and manage Logs

##### API Endpoints

| **Name**             | **Type**         | **QRYN**      | **QRYN Cloud**      | **APIs.**                  |   
|----------------------|------------------|---------------|---------------------|----------------------------|
| Push                 | POST             | 🟢             | 🟢             | `/loki/api/v1/push`        |
| Query                | GET              | 🟢             | 🟢             | `/loki/api/v1/query`       |
| Query Range          | GET              | 🟢             | 🟢             | `/loki/api/v1/query_range` | 
| Labels               | GET              | 🟢             | 🟢             | `/loki/api/v1/label`       | 
| Label Values         | GET              | 🟢             | 🟢             | `/loki/api/v1/label/name/values` | 
| Tail                 | GET              | 🟢             | 🟢             | `/loki/api/v1/tail`        | 
| Ready                | GET              | 🟢             | 🟢             | `/ready`                   | 


##### Compatible Agents

Any Loki compatible client can be used with qryn without modifications

* Grafana Agent
* Vector
* Opentelemetry Collector
* Logtail
* Telegraf
* Fluentd
* Logstash
* _others_


## ** Prometheus **

?> **qryn** implements a full **Prometheus/PromQL** API to ingest, query and manage Metrics

##### API Endpoints

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud**| **APIs.**                       |
|----------------------|--------------------|----------------|----------------|-----------------------------------|
| Remote Write         | POST               | 🟢             | 🟢             |  `/api/v1/prom/remote/write`<br>`/api/prom/remote/write`<br>`/prom/remote/write`<br>`/api/v1/write`       |
| Query Range          | GET/POST           | 🟢             | 🟢             | `/api/v1/query_range`             |
| Query                | GET/POST           | 🟢             | 🟢             | `/api/v1/query`                   |
| Labels               | GET/POST           | 🟢             | 🟢             | `/api/v1/labels`                  |
| Label Values         | GET/POST           | 🟢             | 🟢             | `/api/v1/label/:name/values`      |
| Series Values        | GET/POST           | 🟢             | 🟢             | `/api/v1/series`                  |
| Rules                | GET                | 🟢             | 🟢             | `/api/v1/rules`                   |

##### Compatible Agents

Any Prometheus compatible client can be used with qryn without modifications

* Grafana Agent
* Vector
* Opentelemetry
* Telegraf
* _others_


## ** Tempo **

?> **qryn** implements the **Tempo/TempoQL** API to ingest, query and manage Traces

##### API Endpoints

| **Name**             | **Type**           | **QRYN**            | **QRYN Cloud**| **APIs.**                   | 
|----------------------|--------------------|---------------------|---------------|-----------------------------------|
| Push (Tempo)         | POST               | 🟢                  | 🟢             | `/tempo/api/push`                 | 
| Push (Zipkin)        | POST               | 🟢                  | 🟢             | `/api/v2/spans`                   |
| Push (OTLP)          | POST               | 🟢                  | 🟢             | `/v1/traces` (OTLP)               |
| Query Traces         | GET                | 🟢                  | 🟢             | `/api/traces/{traceId}`           |
| Query Traces (JSON)  | GET                | 🟢                  | 🟢             | `/api/traces/{traceId}/json`      |
| Trace Tags           | GET                | 🟢                  | 🟢             | `/api/search/tags`                | 
| Trace Tag Values     | GET                | 🟢                  | 🟢             | `/api/search/tag/{name}/values`   |
| Push Cloud.          | POST               | 🟡                  | 🟢             | `/tempo/spans`                    |

##### Compatible Agents

Any Tempo/Jaeger compatible client can be used with qryn without modifications

* Grafana Agent
* Opentelemetry Collector
* _others_


## ** OTel **

?> **qryn** implements the **Opentelemetry/OTLPPush** API to ingest Traces, Logs and Metrics

⚡ qryn is officially integrated with opentelemetry supports any log, trace or metric format
Ingested data can be queried using any of the avialable qryn APIs (LogQL, PromQL, TraceQL)

##### API Endpoints

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| OTLPPush             | POST               | 🟢             |🟢             | `/v1/traces`         |

#### OpenTelemetry Collector for qryn

The [qryn otel-collector](https://github.com/metrico/otel-collector) is designed to store observability data _(Traces, Logs, Metrics)_ from multiple vendors/platforms into ClickHouse using qryn fingerprinting and table formats transparently accessible through qryn via _LogQL, PromQL, Tempo and Pyroscope_ queries.

##### Compatible Agents

Any Opentelemetry compatible collector can be used with qryn without modifications

* Grafana Agent
* Opentelemetry Collector
* _others_


## ** Influx **

?> **qryn** implements the **Influx v2** Write API subset for ingestion of Logs _(and [metrics](metrics/ingestion#influx))_

##### API Endpoints

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| Write                | POST               | 🟢             |🟢             | `/influx/api/v2/write`<br>`/write`          |
| Health               | GET                | 🟢             |🟢             | `/influx/health`<br>`/health`          |


##### Compatible Agents

Any InfluxDB v2 compatible client can be used with qryn without modifications

* Telegraf
* _others_

#### CURL Examples

##### Logs
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'syslog,appname=myapp,facility=console,host=myhost,hostname=myhost,severity=warning facility_code=14i,message="warning message here",severity_code=4i,procid="12345",timestamp=1434055562000000000,version=1'
```  
##### Metrics
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```  

!> Replace the **qryn** URL from the example to match your actual deployment!

?> That's it! You're now shipping logs straight off your bash scripts!

## ** Datadog **

?> **qryn** implements a **Datadog v2 Write** API subset for ingestion of Logs and Metrics

##### API Endpoints

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| Logs                 | POST               | 🟢             |🟢             | `/api/v2/logs` |
| Series               | POST               | 🟢             |🟢             | `/api/v2/series`  |
| Insert (Cloudflare Format) | POST         | 🟡             |🟢             | `/cf/api/v1/insert` |


## ** Elastic **

?> **qryn** implements an basic **Elastic API** subset for ingestion of JSON objects as tagged logs.

##### API Endpoints

| **Name**             | **Type**           | **QRYN**      | **QRYN Cloud**| **APIs.**                   |  
|----------------------|--------------------|---------------|---------------|-----------------------------|
| Index                | POST               | 🟢             | 🟢          |  `/:target/_doc`            |
| Index                | POST               | 🟢             | 🟢          | `/:target/_create/:id`      |
| Index                | PUT                | 🟢             | 🟢          | `/:target/_doc/`            |
| Index                | PUT                | 🟢             | 🟢          | `/:target/_create/:id`      | 
| Bulk                 | POST               | 🟢             | 🟢          | `/_bulk`                    |
| Bulk                 | POST               | 🟢             | 🟢          | `/:target/_bulk`            |

?> The `_index` and `_id` tags are automatically added to each insert based on the API request

#### Index API
```bash
curl -X POST "qryn:3100/test-index/_doc/1234" -H 'Content-Type: application/json' -d'
```
```json
{
  "message": "hello",
  "user": "cloki"
}
```

#### Bulk API
```json
{ "index" : { "_index" : "test-index", "_id" : "1234" } }
{ "message" : "hello", "user": "qryn" }
```
```bash
curl -s -H "Content-Type: application/x-ndjson" -XPOST http://qryn/_bulk --data-binary "@bulkreq"
```

!> Replace the **qryn** URL from the example to match your actual deployment!


Either type will get converted to the following `LogQL` insert
```json
{
      "stream": {
        "_index": "test-index",
        "_id": "1234",
        "type": "elastic"
      },
      "values": [
          [ "<unix epoch in nanoseconds>", "{\"message\": \"hello\", \"user\":\"qryn'"}" ]
      ]
    }
```

?> That's it! You're now shipping logs straight off your Elastic agents!


##### Notes
- _The implementation is not focused on speed. Bulking capacity depends on fastify settings._
- _A static type tag is also attached to events ingested through the elastic compatible APIs_
- _delete, update bulk actions and other APIs are not supported_


## ** NewRelic **

?> **qryn** implements an basic **NewRelic API** subset for ingestion of Logs

##### API Endpoints

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| [Logs](https://docs.newrelic.com/docs/logs/log-api/introduction-log-api/)| POST               | 🟢             |🟡             | `/log/v1` |

<!-- tabs:end -->


<!-- tabs:start -->

## ** Popular **

![image](https://user-images.githubusercontent.com/1423657/184487816-fcc86e34-0395-4927-8ceb-33c2ad3e63d4.gif)

?> **qryn** APIs are compatible with [many observability ingestion formats](support.md)

This means _any compatible client_ can be used with **qryn** without plugins or modifications! 🏁

| Client  | Logs  | Metrics | Traces  |
|---|:-:|:-:|:-:|
| [Grafana Agent](https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-agent/)  | 🟢 | 🟢 | 🟢 |
| [OpenTelemetry](https://github.com/metrico/otel-collector)  | 🟢 | 🟢 | 🟢 |
| [Vector/Datadog](https://vector.dev/docs/reference/configuration/sinks/loki/)  | 🟢 | 🟢 |   |
| [Telegraf/Influx](https://docs.influxdata.com/telegraf/v1.21/introduction/getting-started/)  | 🟢 | 🟢 |   |
| [Logstash/Elastic](https://grafana.com/docs/loki/latest/clients/logstash/)  | 🟢 | 🟢 |   |
| [paStash](https://github.com/sipcapture/paStash/wiki/Example:-Loki)  | 🟢 | 🟢 |   |
| [Promtail](https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-promtail/)  | 🟢 |   |   |
| [Promtail Lambda](https://grafana.com/docs/loki/latest/clients/lambda-promtail/) | 🟢 |   |   |
| [Fluentd](https://grafana.com/docs/loki/latest/clients/fluentd/)  | 🟢 |   |   |
| [Docker](https://grafana.com/docs/loki/latest/clients/docker-driver/)  | 🟢 |   |   |
| [Cloudflare Logpush](https://github.com/metrico/cloudflare-worker-logpush-loki) | 🟢  |   |   |
| [Curl](https://github.com/metrico/qryn/wiki/HTTP-API#api-examples) | 🟢 | 🟢 | 🟢 |


!> Is your favourite client not listed? please submit a suggestion and we'll add it 👍

## ** LogQL **

<a id=logql name=logql></a>

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

### Grafana Agent

<a id=grafana name=grafana></a>

Install [Grafana Agent](https://grafana.com/docs/grafana-cloud/data-configuration/agent/) using the official documentation.

The agent configuration is stored in `/etc/grafana-agent.yaml`

This example will scrape and send info from all log files in `/var/log` that end in log. They are labeled with varlogs as the `job` and `job_name`
```
loki:
  configs:
  - name: default
    positions:
      filename: /tmp/positions.yaml
    scrape_configs:
      - job_name: varlogs
        static_configs:
          - targets: [localhost]
            labels:
              job: varlogs
              __path__: /var/log/*log
    clients:
      - url: http://qryn:3100/loki/api/v1/push
        basic_auth:
          username: <User>
          password: <Password>
```

!> Replace the **qryn** URL from the example to match your actual deployment!

Here is another example, scraping logs for a minecraft server with logs stored in a subdirectory of the `/home` directory of a special minecraft user.
```
      - job_name: minecraftlog
        static_configs:
          - targets: [localhost]
            labels:
              job: minecraft
              __path__: /home/MCuser/minecraft/logs/latest.log
```

!> Anytime you change the agent configuration, you must restart the agent for the new configuration to take effect.

```
sudo systemctl restart grafana-agent.service
``` 

## ** Elastic **

<a id=elastic name=elastic></a>

![image](https://user-images.githubusercontent.com/1423657/184496304-4f35a365-efdc-4dca-9771-6b7b1deb9ae3.png ':size=100')

?> **qryn** implements an experimental Elastic API for ingestion of JSON objects as tagged logs.

#### API Endpoints
- [x] `/:target/_doc`
- [x] `/:target/_create/:id`
- [x] `/:target/_doc/:id`
- [x] `/:target/_create/:id`
- [x] `/_bulk`
- [x] `/:target/_bulk`

?> The `_index` and `_id` tags are automatically added to each insert based on the API request

#### Index API
```bash
curl -X POST "qryn:3100/test-index/_doc/1234" -H 'Content-Type: application/json' -d'
```
```json
{
  "message": "hello",
  "user": "cloki"
}
```

#### Bulk API
```json
{ "index" : { "_index" : "test-index", "_id" : "1234" } }
{ "message" : "hello", "user": "qryn" }
```
```bash
curl -s -H "Content-Type: application/x-ndjson" -XPOST http://qryn/_bulk --data-binary "@bulkreq"
```

!> Replace the **qryn** URL from the example to match your actual deployment!


Either type will get converted to the following `LogQL` insert
```json
{
      "stream": {
        "_index": "test-index",
        "_id": "1234",
        "type": "elastic"
      },
      "values": [
          [ "<unix epoch in nanoseconds>", "{\"message\": \"hello\", \"user\":\"qryn'"}" ]
      ]
    }
```

?> That's it! You're now shipping logs straight off your Elastic agents!


##### Notes
- _The implementation is not focused on speed. Bulking capacity depends on fastify settings._
- _A static type tag is also attached to events ingested through the elastic compatible APIs_
- _delete, update bulk actions and other APIs are not supported_

## ** Influx **

<a id=influx name=influx></a>

![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')

?> **qryn** implements the Influx v2 Write API subset for ingestion of Logs _(and [metrics](metrics/ingestion#influx))_

##### API Endpoints
- [x] `/influx/api/v2/write`

#### CURL Examples

##### Logs
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'syslog,appname=myapp,facility=console,host=myhost,hostname=myhost,severity=warning facility_code=14i,message="warning message here",severity_code=4i,procid="12345",timestamp=1434055562000000000,version=1'
```  
##### Metrics
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```  

!> Replace the **qryn** URL from the example to match your actual deployment!

?> That's it! You're now shipping logs straight off your bash scripts!

## ** OTEL **

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

###### qryn collector `config.taml`
The following example enables all the supported formats at once. Filter the required sections to build your configuration.

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
        - job_name: 'otel-collector'
          scrape_interval: 5s
          static_configs:
            - targets: ['exporter:8080']
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
  memory_ballast:
    size_mib: 1000

service:
  extensions: [pprof, zpages, health_check]
  pipelines:
    logs:
      receivers: [fluentforward, otlp]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
    traces:
      receivers: [otlp, jaeger, zipkin]
      processors: [memory_limiter, resourcedetection/system, resource, spanmetrics, servicegraph, batch]
      exporters: [qryn]
    # for align with https://grafana.com/docs/tempo/latest/metrics-generator/span_metrics/#how-to-run
    metrics/spanmetrics:
      receivers: [otlp]
      processors: [metricstransform]
      exporters: [qryn]
    metrics:
      receivers: [prometheus]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
```

?> _That's it!_ You're now _tracing spans to **qryn** using OTLP Collector! 


## ** ClickHouse **

<a id=clickhouse name=clickhouse></a>

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=100 />

ClickHouse populates the `system.text_log` table with its internal logs. Without using any client or file reading application, we can use ClickHouse Materialized View + URL Engine to ship our logs to **qryn** as soon as they are generated using the InfluxDB line format protocol

### Requirements
- Enable [opentelemetry](https://clickhouse.com/docs/en/operations/opentelemetry/) settings in your ClickHouse `config.xml`


### Create Materialized View

The following MV will push logs to the **qryn** `/influx/api/v2/write` API using the ClickHouse **URL engine**
```sql
CREATE MATERIALIZED VIEW default.influx_log_send
ENGINE = URL('http://qryn:3100/influx/api/v2/write', 'LineAsString')
AS SELECT format('syslog,level={0},logger_name={1},type=clickhouse message="{2}" {3}',
   toString(level),
   replaceRegexpAll(toString(logger_name), '[^a-zA-Z0-9_]', '_'),
   replaceAll(replaceRegexpAll(message, '["\\\\]', '\x00\0'), '\x00', '\\'),
   toString(toUnixTimestamp64Nano(event_time_microseconds)))
FROM system.text_log;
```

!> Replace the **qryn** URL from the example to match your actual deployment!

_Done?_ Just wait a few seconds for the logs to flush and they will appear in **qryn**


?> That's it! You're now shipping logs straight off your ClickHouse core!

## ** Curl **

No API guide would be complete without a Curl example - _and we're no exception!_

#### POST Logs
Insert labeled  using the `line` parameter in a _LogQL stream_:

```bash
curl -i -XPOST -H "Content-Type: application/json" http://qryn:3100/loki/api/v1/push \
     --data '{"streams":[{"stream":{"type":"test"},"values":[['$(date +"%s%N")', "hello qryn"]]}]}'
```
```json
{
    "streams": [
        {
            "stream": {"type":"test"},
            "values": [ 
                [
                 "1665767942870000000",
                 "hello qryn"
                ]
            ]
        }
    ]
}
```

#### POST Metrics
We can also insert labeled  using the `value` parameter in a _LogQL stream_

```bash
curl -i -XPOST -H "Content-Type: application/json" http://127.0.0.1:3104/loki/api/v1/push --data '{"streams":[{"stream":{"__name__":"test_metric"},"values":[['$(date +"%s%N")', "metric value", 100]]}]}'
```
```json
{
    "streams": [
        {
            "stream": {"__name__":"test_metric"},
            "values": [
                [
                    "1665767942870000000",
                    "metric value",
                    100
                ]
            ]
        }
    ]
}
```

!> Replace the **qryn** URL from the example to match your actual deployment!

?> _That's it!_ You are now inserting metrics throug Curl in **qryn**

## ** Docker **

<img src="https://user-images.githubusercontent.com/1423657/184507884-624b9598-62e1-413f-854e-8210ecac4e75.png" width=300 />

Integrate Docker with **qryn** using the _native logging driver_

```
version: '3.9'

services:
  some-service:
    image: 'some-service/image:latest'
    ports:
      - '80:80'
    logging:
      driver: loki
      options:
        loki-url: 'http://localhost:3100/loki/api/v1/push'
```

## ** DEV **

![image](https://user-images.githubusercontent.com/1423657/187414382-64a9203f-52a6-4c58-af75-ad4fee3d5a31.png ':size=600')

Integrate **qryn** features in your native application using native libraries

#### 👇 Libraries & Examples
* NodeJS
  * [pino-loki](https://www.npmjs.com/package/pino-loki)
  * [winston-loki](https://github.com/JaniAnttonen/winston-loki)
  * [workers](https://github.com/SirCremefresh/workers-loki-logger)
  * [logpush](https://github.com/DavidJKTofan/logpush-grafana)
* [RUST](https://docs.rs/loki-logger/latest/loki_logger/)
* [Python](https://github.com/GreyZmeem/python-logging-loki)
* [Arduino](https://github.com/grafana/loki-arduino)
* [C#](https://github.com/serilog-contrib/serilog-sinks-grafana-loki)
* [GO](https://github.com/grafana/loki-client-go)
* [.NET](https://github.com/hannahchan/LokiLoggingProvider)
* [Java](https://github.com/mjfryc/mjaron-tinyloki-java)
* [Lambda/GO](https://github.com/Sorarinu/cloudwatch-logs-to-loki)
* [Julia](https://github.com/JuliaLogging/LokiLogger.jl)

?> Got an interesting link or resource to add? Please submit a `contribution`

<!-- tabs:end -->
