# Introduction

Meet <b>qryn</b>: <i>the open-source polyglot observability framework</i><br/><br/>
 
?> ... it's pronounced /ˈkwɪr..ɪŋ/ or just querying

#### Less is More 👍
**qryn**is a fast, thin, all-in-one polyglot API stack built on top of _ClickHouse_

_Everything is simple and works right out of the box. Batteries included!_ 🔋


<!-- tabs:start -->

#### ** Ingestion **

🎉 _No custom formats to integrate_ 

Ingestion and PUSH APIs are transparently compatible with [Loki](logs/ingestion#logql), [Prometheus](metrics/ingestion#prometheus), [Tempo](telemetry/ingestion#zipkin), [InfluxDB](metrics/ingestion#influx), [Elastic](logs/ingestion#elastic) _and others_

**qryn** is ready to use with Agents such as _[Promtail, Grafana-Agent, Vector, Logstash, Telegraf](ingestion.md) and many more_

#### ** Querying **

🎉 _No custom query languages to learn_ 

Start querying instantly using [LogQL, PromQL, Flux or Temapo](getting-started.md) to dynamically _[search, filter and extract data](getting-started.md) from logs, events, metrics and traces_

**qryn** is ready to use with [Grafana](getting-started?id=grafana) or using the integrated [View UI](getting-started?id=view) and [CLI](getting-started?id=cli)


#### ** Database **

🎉 _No limits accessing your own data_ 

Want more than our APIs offer? Access your data using pure ClickHouse SQL _for fun and compliance_.

**qryn** was designed to be cloud native for _serverless and stateless operations_ on top of [ClickHouse](https://clickhouse.com/clickhouse).


<!-- tabs:end --> 

![image](https://user-images.githubusercontent.com/1423657/187255795-f67e66be-bbee-4244-b291-342ca983900f.png)
