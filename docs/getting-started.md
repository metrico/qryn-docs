# Querying

In this section, we'll learn how to find and query our ingested data. Let's get __qryn__!


### Integrations

Let's connect our **qryn** instance to Grafana

!> The [qryn-view](view.md) embedded UI can be used to explore data on port `:3100`

<!-- tabs:start -->

#### ** Loki **
Connect Grafana to **qryn** using the _built-in_ `Loki` datasource

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
