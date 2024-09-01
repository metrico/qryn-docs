# 👋 Introduction
 
?> ... it's pronounced /ˈkwɪr..ɪŋ/ or just **_querying_**

**qryn** is a _fast, thin, all-in-one_ _**polyglot**_ observability stack built on top of _**ClickHouse**_

_Designed from the ground up to handle infinite Logs, Metrics, Traces, Profiles - batteries included!_ 🔋

> The project started in 2018 as "cLoki" and gradually evolved from a prototype into a mature, stable and independent observability stack able to act as a _drop-in alternative_ to Grafana Cloud/LGTM and API compatible with multiple query and ingestion formats from popular observability vendors such as _Opentelemetry, InfluxDB, Elastic, Datadog and many more_. The beauty of qryn is in its simplicity, acting as a _lightweight transpiler_ on top of OLAP SQL storage without reinventing the wheel and offering its users performance, security and full ownership and control of their data without limitations and without obscure formats. _Less is more, without giving up any feature._

#### Long Play, Here to Stay

**qryn** is an underdog actively developed, maintained, deployed and trusted by integrators and customers ⭐

<img src="https://camo.githubusercontent.com/0dd3a1ac0a2e9672c2f7b660982a2ef16de9568f6124afb78ca4f3a15ed84c50/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d6d65747269636f2f7172796e26747970653d54696d656c696e65" />

<!-- tabs:start -->

#### ** Ingestion **

🎉 _No custom formats to integrate_ 

Our Ingestion APIs are transparently compatible with [Loki](logs/ingestion#logql), [Prometheus](metrics/ingestion#prometheus), [Tempo](telemetry/ingestion#zipkin), [Pyroscope](https://qryn.metrico.in/#/profiling/ingestion), [InfluxDB](metrics/ingestion#influx), [Elastic](logs/ingestion#elastic) _and others_, ready to use with a variery of Agents such as _[Promtail, Grafana-Agent, Vector, Logstash, Telegraf](ingestion.md) and many more_

#### ** Querying **

🎉 _No custom query languages to learn_ 

Start querying logs, metrics and traces instantly using [LogQL, PromQL, TempoQL, Pyroscope](getting-started.md)

**qryn** is ready to use with [Grafana datasources](guide/datasources) or using the integrated [View UI](getting-started?id=view) and [CLI](getting-started?id=cli)


#### ** Database **

🎉 _No limits accessing your own data_ 

**qryn** was designed to be cloud native for _serverless and stateless operations_ on top of [ClickHouse](https://clickhouse.com/clickhouse).

Want more than our APIs offer? Access your data using pure ClickHouse SQL _for fun and compliance_.

### 👑 Hosting Partners
Looking for a hosted ClickHouse option? There are many great companies we work with delivering _state-of-the-art_ managed service instances and great benchmark results with predictable costs at any scale - all backed by extremely skilled technical teams, and compatible with _qryn_

💜 [Gigapipe](https://gigapipe.com) _(featuring a hosted qryn:cloud integration)_<br>
💙 [Altinity](https://altinity.com) _(supporting the community and many of our customers)_<br>
💚 [DoubleCloud](https://double.cloud/) _(very kind sponsor of our public demo resources)_<br>
🟨 [Clickhouse Cloud](https://clickhouse.com/) _(popular with users, free trials)_<br>


#### ** Hosted **

<img src="https://user-images.githubusercontent.com/1423657/219211554-8a72f1b7-b570-4da8-a550-ee2cf3c413d3.png">

<!--
![image](https://user-images.githubusercontent.com/1423657/187255795-f67e66be-bbee-4244-b291-342ca983900f.png)
-->

### 👑 Polyglot Cloud
Looking for a hosted qryn option? Gigapipe powers our fully managed, flat priced **qryn** experience.

💜 [qryn.cloud](https://qryn.cloud) _(fixed cost, managed qryn + clickhouse services)_<br>



<!-- tabs:end --> 

<br> 

<img src="https://github.com/metrico/qryn-docs/assets/1423657/a5164f98-d3ed-4638-afe5-c87d252c74af">
