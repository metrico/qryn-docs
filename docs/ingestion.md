# 🔻 Data Ingestion

Ingesting data with **qryn** is easy and painless. _Choose a supported integration and start shipping._

<!-- tabs:start -->

#### ** Logs **

Get started using the [Log Ingestion](logs/ingestion.md) section

[![image](https://user-images.githubusercontent.com/1423657/185749193-faa91e58-4283-479a-8196-bae5212ae141.png ':size=100')](logs/ingestion#popular)
[![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')](logs/ingestion#grafana)
[![image](https://user-images.githubusercontent.com/1423657/184496304-4f35a365-efdc-4dca-9771-6b7b1deb9ae3.png ':size=100')](logs/ingestion#elastic)
[![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')](logs/ingestion#influx)
[![image](https://avatars.githubusercontent.com/u/54801242?s=200&v=4 ':size=100')](logs/ingestion#clickhouse)


#### ** Metrics **

Get started using the [Metrics Ingestion](metrics/ingestion.md) section

[![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')](metrics/ingestion#logql)
[![image](https://user-images.githubusercontent.com/1423657/184496973-9f46e551-872d-4a25-877c-51a2e5f53e84.png ':size=100')](metrics/ingestion#prometheus)
[![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')](metrics/ingestion#influx)
[![image](https://avatars.githubusercontent.com/u/54801242?s=200&v=4 ':size=100')](metrics/ingestion#clickhouse)

#### ** Telemetry **

Get started using the [Telemetry Ingestion](telemetry/ingestion.md) section

[![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')](telemetry/ingestion#grafana)
[![image](https://user-images.githubusercontent.com/1423657/184494381-15d20f5d-3d52-411b-9064-dfd2ccea7c1c.png ':size=100')](telemetry/ingestion#zipkin)
[![image](https://user-images.githubusercontent.com/1423657/184494438-17d7ceb0-a62a-4819-9b1c-43d7f0baf802.png ':size=100')](telemetry/ingestion#zipkin)
[![image](https://avatars.githubusercontent.com/u/54801242?s=200&v=4 ':size=100')](telemetry/ingestion#clickhouse)

#### ** Profiling **

Get started using the Pyroscope [Continuous Profiling](profiling/ingestion.md) section

[![image](https://github.com/metrico/qryn-docs/assets/1423657/4525cc49-1a5e-4714-aa16-088928d245ff ':size=100')](profiling/ingestion)


#### ** Custom **

Missing an ingestion API? Open a [feature request](https://github.com/metrico/qryn/issues) or a PR with your work.

![image](https://user-images.githubusercontent.com/1423657/184502410-eacd247e-e046-4cdb-9e0c-39411e02d734.png ':size=100')

<!-- tabs:end -->

## 🔻 API Support

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



?> **qryn** APIs are compatible with [many observability ingestion formats](support.md)

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

<img src="https://user-images.githubusercontent.com/1423657/184487816-fcc86e34-0395-4927-8ceb-33c2ad3e63d4.gif" width=600 />
