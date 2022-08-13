![qryn_integrations](https://user-images.githubusercontent.com/1423657/184487816-fcc86e34-0395-4927-8ceb-33c2ad3e63d4.gif)

# Ingestion

Ingesting data with **qryn** is easy and painless. Just pick an integration and you're set!


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
