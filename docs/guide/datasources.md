## Integrations

<a name="grafana" id="grafana"></a>
#### Grafana

Let's connect our **qryn** instance to Grafana in seconds using _native datasources_

?> Using Grafana `8.2+` or `9.0+` is required to enjoy all available features

<!-- tabs:start -->

#### ** Loki **
Connect Grafana to **qryn** using the _built-in_ `Loki` datasource

* Log into your Grafana instance
* Browse to `Configuration` > `Data Sources` via the cog icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Loki` from the list
* Use your **qryn** URL and `save`

![image](https://user-images.githubusercontent.com/1423657/184536845-31638c7f-30bd-4416-ae99-4486024367f8.png)


#### Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Loki-qryn
    type: loki
    access: proxy
    uid: loki
    url: http://qryn:3100
    editable: true
    jsonData:
      derivedFields:
        - datasourceUid: tempo
          matcherRegex: ".*/traces/(?\u003ctraceId\u003e.*)"
          name: "traceId"
          url: "${__value.raw}"
        - datasourceUid: tempo
          matcherRegex: "^.*?traceI[d|D]=(\w+).*$"
          name: TraceID
          url: "${__value.raw}"
```

#### ** Prometheus **
Connect Grafana to **qryn** using the _built-in_ `Prometheus` datasource

* Log into your Grafana instance
* Browse to `Configuration` > `Data Sources` via the cog icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Prometheus` from the list
* Use your **qryn** URL and `save`

![image](https://user-images.githubusercontent.com/1423657/184536921-b3ff7fab-f71f-4f07-93c3-40a5b9915c64.png)

#### Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Prom-qryn
    type: prometheus
    access: proxy
    uid: prometheus
    url: http://qryn:3100
    basicAuth: false
    jsonData:
     graphiteVersion: "1.1"
     tlsAuth: false
     tlsAuthWithCACert: false
```

#### ** Tempo **
Connect Grafana to **qryn** using the _built-in_ `Tempo` datasource

* Log into your Grafana instance
* Browse to `Configuration` > `Data Sources` via the cog icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Tempo` from the list
* Use your **qryn** URL and `save`

![image](https://user-images.githubusercontent.com/1423657/184536886-7a1eb428-9671-4a36-a93b-173fcfcd775d.png)

#### Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Tempo-qryn
    type: tempo
    access: proxy
    uid: tempo
    url: http://qryn:3100
    jsonData:
      nodeGraph:
        enabled: true
      tracesToLogs:
        datasourceUid: loki
        filterByTraceID: false
        spanEndTimeShift: "2000ms"
        spanStartTimeShift: "-2000ms"
        tags: ['job']
```

<!-- tabs:end -->
