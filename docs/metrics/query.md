# 🔎 Querying Metrics

In this section, we'll learn how to query and filter metrics using **PromQL**. Let's get __qryn__!

!> **qryn:oss** emulates a subset of promql queries using logql metics

?> **qryn:cloud** fully supports prometheus and all promql functions

<!-- tabs:start -->
### ** ⭐ Grafana **
<a id=grafana></a>

Let's **explore** our data using [Grafana](guide/datasources.md) and its _amazing visualizations_

![grafanaexplore](https://user-images.githubusercontent.com/1423657/184538094-13c11500-24ef-4468-9f33-dc9d564238e3.gif)

### PromQL <!-- {docsify-ignore-all} -->
#### Visualize metrics using PromQL 📈

<details>
  <summary>Query Builder</summary>
  <br>
  Use the Grafana <a href="https://grafana.com/blog/2022/07/18/new-in-grafana-9-the-prometheus-query-builder-makes-writing-promql-queries-easier/" target="_blank">Prometheus query builder</a> available in Grafana 9.x to browse qryn metrics
  <br>
  <img src="https://user-images.githubusercontent.com/1423657/187435182-8d739f3d-53b0-4561-8ca6-86c6736411b2.png">
  <br>
</details>

#### Find a Label 🏷️

Let's use our **qryn** `Prometheus` datasource find metrics for label `cpu_percent_value`

![image](https://user-images.githubusercontent.com/1423657/185901793-37529481-5201-474f-b01e-b30386a8ef89.png)

##### Visualize metrics 📊

Let's calculate the `rate` for our matching metrics 

![image](https://user-images.githubusercontent.com/1423657/185901314-b94082cf-b2ea-4fc2-93df-5d11e9fe055c.png)

### Go Pro 🥊

That was easy, _wasn't it?_

Progress your knowledge using the [advanced Promql articles]([guide/logql.md](https://qryn.metrico.in/#/examples))

<!-- tabs:end -->
