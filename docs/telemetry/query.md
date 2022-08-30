# 🔎 Querying Telemetry

In this section, we'll learn how to query and filter ingested traces using **Tempo**. Let's get __qryn__!

<!-- tabs:start -->
### ** ⭐ Grafana **
<a id=grafana></a>

Let's **explore** our data using [Grafana](guide/datasources.md) and its _amazing visualizations_

![grafanaexplore](https://user-images.githubusercontent.com/1423657/184538094-13c11500-24ef-4468-9f33-dc9d564238e3.gif)

### Tempo <!-- {docsify-ignore-all} -->
#### Find a Trace using Tempo 🧲

Tempo is _Grafana's take on the Zipkin API_, extended to provide Prometheus-like filtering capabilities.

#### Beta Search
Use the **qryn** `Tempo` datasource to find traces using _Beta Search_

![image](https://user-images.githubusercontent.com/1423657/186616553-ca34e26a-6a59-4a04-98e8-c27fdf8f6159.png)

#### Loki Search
Use the **qryn** `Tempo` datasource to find traces using _Loki Search_

![image](https://user-images.githubusercontent.com/1423657/184548814-b5a4c641-010e-47b1-91c8-5aa402a85908.png)

#### TraceID
Use the **qryn** `Tempo` datasource to find traces using a known _TraceID_

![image](https://user-images.githubusercontent.com/1423657/187453673-a4af8ca8-8288-45fd-81e9-ef2cace91808.png)


#### Service Graph
Use the **qryn** `Tempo` datasource to navigate Service relations using _Service Graph_
![image](https://user-images.githubusercontent.com/1423657/187453193-bd3936b8-fb6e-464f-ac65-413233ca66c2.png)


### Go Pro 🥊

That was easy, _wasn't it?_

Progress your knowledge using the [advanced Tempo articles]([guide/logql.md](https://qryn.metrico.in/#/examples))

<!-- tabs:end -->
