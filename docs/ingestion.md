# Ingestion

Ingesting data with **qryn** is easy and painless. Just pick an integration and you're set!

<br />

<img src="https://user-images.githubusercontent.com/1423657/184487816-fcc86e34-0395-4927-8ceb-33c2ad3e63d4.gif" width=600 />


<!-- tabs:start -->

#### ** Logs **
The following protocol APIs are supported for ingesting logs:
* LogQL PUSH
* Influx _(line protocol syslog)_

  <!-- tabs:start -->
  ##### ** Sub 1 **
  aaa
  ##### ** Sub 1 **
  bbb
  <!-- tabs:end -->


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
