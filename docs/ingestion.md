# Ingestion

Ingesting data with **qryn** is easy and painless. Just pick an integration and you're set!

<br />

<img src="https://user-images.githubusercontent.com/1423657/184487816-fcc86e34-0395-4927-8ceb-33c2ad3e63d4.gif" width=600 />



### Logs
The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->
#### ** LogQL **
* LogQL PUSH
#### ** Elastic **
* Elastic _(_index, _bulk)_
#### ** Influx **
* Influx _(line protocol syslog)_
#### ** ClickHouse MV **
* ClickHouse Materialized View
<!-- tabs:end -->


### Metrics
The following protocol APIs are supported for ingesting metrics:
<!-- tabs:start -->
#### ** Prometheus **
* Prometheus _(remote_write)_
#### ** LogQL **
* LogQL PUSH _(unwrap_metric)_
#### ** Influx **
* Influx _(line protocol)_
<!-- tabs:end -->

### Traces
The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->
#### ** Zipkin **
* Zipkin _(json/protobuf)_
#### ** OTLP **
* OTLP _(grafana-agent)_
#### ** ClickHouse MV **
* ClickHouse Materialized View
<!-- tabs:end -->
