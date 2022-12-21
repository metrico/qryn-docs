# Supported API Features

At the time of writing this document **qryn** supports the following APIs:

## LogQL API

🟢 Supported 🟡 Partial Support 🔴 Not Supported

| **Name**             | **Type**         | **QRYN**      | **QRYN Cloud**      | **APIs.**                  |   
|----------------------|------------------|---------------|---------------------|----------------------------|
| Push                 | POST             | 🟢             | 🟢             | `/loki/api/v1/push`        |
| Query                | GET              | 🟢             | 🟢             | `/loki/api/v1/query`       |
| Query Range          | GET              | 🟢             | 🟢             | `/loki/api/v1/query_range` | 
| Labels               | GET              | 🟢             | 🟢             | `/loki/api/v1/label`       | 
| Label Values         | GET              | 🟢             | 🟢             | `/loki/api/v1/label/name/values` | 
| Tail                 | GET              | 🟢             | 🟢             | `/loki/api/v1/tail`        | 
| Ready                | GET              | 🟢             | 🟢             | `/ready`                   | 


## Tempo API

🟢 Supported 🟡 Partial Support 🔴 Not Supported

| **Name**             | **Type**           | **QRYN**            | **QRYN Cloud**| **APIs.**                   | 
|----------------------|--------------------|---------------------|---------------|-----------------------------------|
| Push                 | POST               | 🟢                  | 🟢             | `/tempo/api/push`                 | 
| Push                 | POST               | 🟢                  | 🟢             | `/api/v2/spans`                   | 
| Query Spans          | GET                | 🟢                  | 🟢             | `/api/v2/spans`                   | 
| Query Traces         | GET                | 🟢                  | 🟢             | `/api/traces/{traceId}`           | 
| Labels               | GET                | 🟢                  | 🟢             | `/api/search/tags`                | 
| Label Values         | GET                | 🟢                  | 🟢             | `/api/search/tag/{name}/values`   | 
| Special Push (cloud) | POST               | 🟡                  | 🟢             | `/tempo/spans`                    |


## Prometheus API

🟢 Supported 🟡 Partial Support 🔴 Not Supported


| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud**| **APIs.**                       |
|----------------------|--------------------|----------------|----------------|-----------------------------------|
| Remote Write         | POST               | 🟢             | 🟢             | `/api/v1/prom/remote/write`       |
| Remote Write         | POST               | 🟢             | 🟢             | `/api/prom/remote/write`          |
| Query Range          | GET/POST           | 🟡             | 🟢             | `/api/v1/query_range`             |
| Query                | GET/POST           | 🟡             | 🟢             | `/api/v1/query`                   |
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

🟢 Supported 🟡 Partial Support 🔴 Not Supported

| **Name**             | **Type**           | **QRYN**      | **QRYN Cloud**| **APIs.**                   |  
|----------------------|--------------------|---------------|---------------|-----------------------------|
| Index                | POST               | 🟢             | 🟢            |  `/:target/_doc`            |
| Index                | POST               | 🟢             | 🟢            | `/:target/_create/:id`      |
| Index                | PUT                | 🟢             | 🟢            | `/:target/_doc/`            |
| Index                | PUT                | 🟢             | 🟢            | `/:target/_create/:id`      | 
| Bulk                 | POST               | 🟢             | 🟢            | `/_bulk`                    |
| Bulk                 | POST               | 🟢             | 🟢            | `/:target/_bulk`            |


## Influx API

🟢 Supported 🟡 Partial Support 🔴 Not Supported

| **Name**             | **Type**           | **QRYN**       | **QRYN Cloud** | **APIs.**                       |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| Write                | POST               | 🟢             |🟢               | `/influx/api/v2/write`          |

