# Ingestion

Ingesting data with **qryn** is painless and effortless. Just pick an integration and you're set.


<!-- tabs:start -->

#### ** Logs **
The following protocol APIs are supported for ingesting logs:
* LogQL PUSH
* Influx _(line protocol syslog)_

#### ** Metrics **
The following protocol APIs are supported for ingesting metrics:
* Prometheus _(remote_write)_
* LogQL PUSH _(unwrap_metric)_
* Influx _(line protocol)_

#### ** Traces **
The following protocol APIs are supported for ingesting logs:
* Zipkin _(json/protobuf)_
* OTLP _(grafana-agent)_

<!-- tabs:end -->

<img src="https://user-images.githubusercontent.com/1423657/143876342-85531041-aca5-4892-a218-e8775674867d.gif" />
