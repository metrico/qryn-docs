# ð Introduction
 
?> ... it's pronounced /ËkwÉŠr..ÉŠÅ/ or just _querying_ 

**qryn** is a _fast, thin, all-in-one_ _**polyglot**_ observability stack built on top of _**ClickHouse**_

_Logs, Metrics and Traces made simple, right out of the box - batteries included!_ ð


<!-- tabs:start -->

#### ** Ingestion **

ð _No custom formats to integrate_ 

Ingestion APIs are transparently compatible with [Loki](logs/ingestion#logql), [Prometheus](metrics/ingestion#prometheus), [Tempo](telemetry/ingestion#zipkin), [InfluxDB](metrics/ingestion#influx), [Elastic](logs/ingestion#elastic) _and others_

**qryn** is ready to use with Agents such as _[Promtail, Grafana-Agent, Vector, Logstash, Telegraf](ingestion.md) and many more_

#### ** Querying **

ð _No custom query languages to learn_ 

Start querying logs, metrics and traces instantly using [LogQL, PromQL, Flux or Tempo/Zipkin](getting-started.md)

**qryn** is ready to use with [Grafana datasources](guide/datasources) or using the integrated [View UI](getting-started?id=view) and [CLI](getting-started?id=cli)


#### ** Database **

ð _No limits accessing your own data_ 

**qryn** was designed to be cloud native for _serverless and stateless operations_ on top of [ClickHouse](https://clickhouse.com/clickhouse).

Want more than our APIs offer? Access your data using pure ClickHouse SQL _for fun and compliance_.

### ð Hosting Partners
Looking for a hosted ClickHouse option? There are many great companies we work with delivering _state-of-the-art_ managed service instances and great benchmark results with predictable costs at any scale - all backed by extremely skilled technical teams, and compatible with _qryn_

ð [Gigapipe](https://gigapipe.com) _(featuring a hosted qryn:cloud integration)_<br>
ð [Altinity](https://altinity.com) _(supporting the community and many of our customers)_<br>
ð [DoubleCloud](https://double.cloud/) _(very kind sponsor of our public demo resources)_<br>
ð [ClickHouse](https://clickhouse.com) _(early alpha stage, not yet tested for production)_<br>


#### ** Hosted **

<img src="https://user-images.githubusercontent.com/1423657/200078554-f8352174-9a6b-4f4a-90fc-1c6521d46c5b.png">

<!--
![image](https://user-images.githubusercontent.com/1423657/187255795-f67e66be-bbee-4244-b291-342ca983900f.png)
-->

### ð Polyglot Cloud
Looking for a hosted qryn option? Gigapipe and qxip offer a fully managed, pay as you go **qryn** experience.

ð [Gigapipe](https://qryn.cloud) _(pay as you go, managed qryn and clickhouse services)_<br>



<!-- tabs:end --> 

<br> 

<img src="https://user-images.githubusercontent.com/1423657/191464140-48851b2c-ada0-4cfb-b27d-315edc0ba890.gif">
