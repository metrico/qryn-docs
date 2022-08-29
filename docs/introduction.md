# Introduction

 Meet <b>qryn</b>: <i>the open-source polyglot observability framework</i><br/><br/>
 
Ingest and analyze logs, metrics and traces from any agent supporting Loki, Elastic, Influx, Prometheus, Zipkin _and more!_

?> ... it's pronounced /ˈkwɪr..ɪŋ/ or just querying

 
 **qryn** was designed as a fast, thin,  polyglot API leveraging the the many strenghts of _ClickHouse_
    
![image](https://user-images.githubusercontent.com/1423657/187255795-f67e66be-bbee-4244-b291-342ca983900f.png)


This section describes some of the general design concepts and details the interaction between stack components.

<!-- tabs:start -->

#### ** Ingestion **

_No custom formats._ 

Just use one of the many Agents supporting [Loki, Prometheus, Influx, Elastic, Zipkin](ingestion.md) and you're shipping.

#### ** Querying **

_No custom query languages to learn._ 

Start querying instantly using [LogQL, PromQL, Flux or Temapo](getting-started.md)


#### ** Database **

_No limits._ 

All of your data is instantly accessible through ClickHouse SQL for fun and compliance.

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=50 /><br/>

<!-- tabs:end --> 


