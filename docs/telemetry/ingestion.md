# Trace Ingestion

Ingesting telemetry with **qryn** is easy and painless. Just pick an integration and you're set!


### Traces
The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->
## ** Zipkin **

**qryn** natively implements a json/http [Zipkin](https://zipkin.io/) receiver API endpoint to ingest tracing data

#### Send Spans using OpenTelemetry libraries

Trace from any application supported by [opentelemetry](https://github.com/open-telemetry) using standard libraries

- [zipkin sender](https://github.com/open-telemetry/opentelemetry-js/blob/main/examples/tracer-web/examples/zipkin/index.js)

#### Send Spans using CURL

Trace from your console for testing and to support telemetry withing your bash scripts

```bash
curl -X POST http://localhost:3100/tempo/api/push -H 'Content-Type: application/json' -d '[{
 "id": "1234",
 "traceId": "d6e9329d67b6146b",
 "timestamp": '$(date +%s%N | cut -b1-16)',
 "duration": 1000,
 "name": "span from bash!",
 "tags": {
    "http.method": "GET",
    "http.path": "/api"
  },
  "localEndpoint": {
    "serviceName": "shell script"
  }
}]'
```
```bash
curl -X POST http://localhost:3100/tempo/api/push  -H 'Content-Type: application/json' -d '[{
 "id": "5678",
 "traceId": "d6e9329d67b6146b",
 "parentId": "1234",
 "timestamp": '$(date +%s%N | cut -b1-16)',
 "duration": 1000,
 "name": "child span from bash!",
  "localEndpoint": {
    "serviceName": "shell script"
  }
}]'
```


### Screenshot
##### Tempo Spans
![image](https://user-images.githubusercontent.com/1423657/147878090-a7630467-433e-4912-a439-602ce719c21d.png)
##### Tempo Logs
![image](https://user-images.githubusercontent.com/1423657/147878505-4136912b-e8b4-492d-b98f-7137dfeee015.png)

That's it! You're now _tracing spans using ClickHouse_! 


## ** ClickHouse MV **

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=200 />

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

That's it! You're now tracing _ClickHouse using ClickHouse_! 


<!-- tabs:end -->
