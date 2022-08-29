# Introduction

 Meet <b>qryn</b>: <i>the open-source polyglot observability stack</i><br/><br/>
    
<img src="https://github.com/metrico/qryn-docs/blob/main/docs/resources/images/qryn_logo_trans.png?raw=true" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184496304-4f35a365-efdc-4dca-9771-6b7b1deb9ae3.png" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184496973-9f46e551-872d-4a25-877c-51a2e5f53e84.png" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184494381-15d20f5d-3d52-411b-9064-dfd2ccea7c1c.png" width=50 />&nbsp;
<img src="https://user-images.githubusercontent.com/1423657/184494438-17d7ceb0-a62a-4819-9b1c-43d7f0baf802.png" width=50 />&nbsp;
<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=50 /><br/>
    
<img src="https://user-images.githubusercontent.com/1423657/184538094-13c11500-24ef-4468-9f33-dc9d564238e3.gif" width=500 class=border />

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

<!-- tabs:end --> 


