# Supported API Features

At the time of writing this document **qryn** supports the following APIs:

🟢 Supported 🟡 Partial Support 🔴 Not Supported

## LogQL API

| **Name**             | **Type**         | **QRYN**      | **QRYN Cloud**      | **APIs.**                  |   
|----------------------|------------------|---------------|---------------------|----------------------------|
| Push                 | POST             | 🟢             | 🟢             | `/loki/api/v1/push`        |
| Query                | GET              | 🟢             | 🟢             | `/loki/api/v1/query`       |
| Query Range          | GET              | 🟢             | 🟢             | `/loki/api/v1/query_range` | 
| Labels               | GET              | 🟢             | 🟢             | `/loki/api/v1/label`       | 
| Label Values         | GET              | 🟢             | 🟢             | `/loki/api/v1/label/name/values` | 
| Tail                 | GET              | 🟢             | 🟢             | `/loki/api/v1/tail`        | 
| Ready                | GET              | 🟢             | 🟢             | `/ready`                   | 

?> You can add ```&csv=1``` to your query_range parameters to receive output in CSV format

## Tempo API

| **Name**             | **Type**           | **QRYN**            | **QRYN Cloud**| **APIs.**                   | 
|----------------------|--------------------|---------------------|---------------|-----------------------------------|
| Push (Tempo)         | POST               | 🟢                  | 🟢             | `/tempo/api/push`                 | 
| Push (Zipkin)        | POST               | 🟢                  | 🟢             | `/api/v2/spans`                   |
| Push (OTLP)          | POST               | 🟢                  | 🟢             | `/v1/traces` (OTLP)               |
| Query Traces         | GET                | 🟢                  | 🟢             | `/api/traces/{traceId}`           |
| Query Traces (JSON)  | GET                | 🟢                  | 🟢             | `/api/traces/{traceId}/json`      |
| Trace Tags           | GET                | 🟢                  | 🟢             | `/api/search/tags`                | 
| Trace Tag Values     | GET                | 🟢                  | 🟢             | `/api/search/tag/{name}/values`   |
| Push Cloud           | POST               | 🟢                  | 🟢             | `/tempo/spans`                    |


## Prometheus API

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud**| **APIs.**                       |
|----------------------|--------------------|----------------|----------------|-----------------------------------|
| Remote Write         | POST               | 🟢             | 🟢             |  `/api/v1/prom/remote/write`<br>`/api/prom/remote/write`<br>`/prom/remote/write`<br>`/api/v1/write`       |
| Query Range          | GET/POST           | 🟢             | 🟢             | `/api/v1/query_range`             |
| Query                | GET/POST           | 🟢             | 🟢             | `/api/v1/query`                   |
| Labels               | GET/POST           | 🟢             | 🟢             | `/api/v1/labels`                  |
| Label Values         | GET/POST           | 🟢             | 🟢             | `/api/v1/label/:name/values`      |
| Format Query         | GET/POST           | 🔴             | 🔴             | `/api/v1/format_query`            |
| Series Values        | GET/POST           | 🟢             | 🟢             | `/api/v1/series`                  |
| Exemplars Query      | GET/POST           | 🔴             | 🔴             | `/api/v1/query_exemplars`         |
| Targets              | GET                | 🔴             | 🔴             | `/api/v1/targets`                 |
| Rules                | GET                | 🟢             | 🟢             | `/api/v1/rules`                   |
| Alerts               | GET                | 🔴             | 🔴             | `/api/v1/alerts`                  |
| Targets Metadata     | GET                | 🔴             | 🔴             | `/api/v1/targets/metadata`        |
| Metadata             | GET                | 🟡             | 🔴             | `/api/v1/metadata`                |
| Alert Managers       | GET                | 🔴             | 🔴             | `/api/v1/alertmanagers`           |
| Config               | GET                | 🔴             | 🔴             | `/api/v1/status/config`           |
| Flags                | GET                | 🔴             | 🔴             | `/api/v1/status/flags`            |
| Runtime Info         | GET                | 🔴             | 🔴             | `/api/v1/status/runtimeinfo`      |
| Build Info           | GET                | 🟡             | 🔴             | `/api/v1/status/buildinfo`        |
| TSDB Info            | GET                | 🔴             | 🔴             | `/api/v1/status/tsdb`             |
| Wal Replay           | GET                | 🔴             | 🔴             | `/api/v1/status/walreplay`        |


## Elastic API

| **Name**             | **Type**           | **QRYN**      | **QRYN Cloud**| **APIs.**                   |  
|----------------------|--------------------|---------------|---------------|-----------------------------|
| Index                | POST               | 🟢             | 🟢          |  `/:target/_doc`            |
| Index                | POST               | 🟢             | 🟢          | `/:target/_create/:id`      |
| Index                | PUT                | 🟢             | 🟢          | `/:target/_doc/`            |
| Index                | PUT                | 🟢             | 🟢          | `/:target/_create/:id`      | 
| Bulk                 | POST               | 🟢             | 🟢          | `/_bulk`                    |
| Bulk                 | POST               | 🟢             | 🟢          | `/:target/_bulk`            |


## Influx API

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| Write                | POST               | 🟢             |🟢             | `/influx/api/v2/write`<br>`/write`          |
| Health               | GET                | 🟢             |🟢             | `/influx/health`<br>`/health`          |



## Datadog API

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| Logs                 | POST               | 🟢             |🟢             | `/api/v2/logs` |
| Series               | POST               | 🟢             |🟢             | `/api/v2/series`  |
| Insert (Cloudflare Format) | POST         | 🟡             |🟢             | `/cf/api/v1/insert` |

## NewRelic API

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| [Logs](https://docs.newrelic.com/docs/logs/log-api/introduction-log-api/)| POST               | 🟢             |🟡             | `/log/v1` |

## Pyroscope API

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| [ProfileTypes](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L10)| POST               | 🟢             |🟡             | `/querier.v1.QuerierService/ProfileTypes` |
| [LabelValues](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L12) | POST               | 🟢             |🟡             | `/querier.v1.QuerierService/LabelValues` |
| [LabelNames](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L14) | POST               | 🟢             |🟡             | `/querier.v1.QuerierService/LabelNames` |
| [SelectSeries](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L24) | POST               | 🟢             |🟡             | `/querier.v1.QuerierService/SelectSeries` |
| [SelectMergeStacktraces](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L18) | POST               | 🟢             |🟡             | `/querier.v1.QuerierService/SelectMergeStacktraces` |

## OTEL Collector

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| OTLPPush             | POST               | 🟢             |🟢              | `/v1/traces`         |
| OTLPLogPush          | POST               | 🟢             |🟢              | `/v1/logs`         |

* Any Log, Metric or Trace format supported via [qryn-collector](https://github.com/metrico/otel-collector)
