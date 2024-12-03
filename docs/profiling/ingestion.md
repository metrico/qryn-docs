# 🔻 Profiling Ingestion

The following protocol APIs are supported for ingesting continuous profiling events with **qryn**:

<!-- tabs:start -->

## Pyroscope API

| **Name** | **Type** | **QRYN** | **Cloud** | **APIs** |   
|----------------------|--------------------|----------------|----------------|---------------------------------|
| [ProfileTypes](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L10)| POST | 🟢 |🟡 | `/querier.v1.QuerierService/ProfileTypes` |
| [LabelValues](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L12) | POST | 🟢 |🟡 | `/querier.v1.QuerierService/LabelValues` |
| [LabelNames](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L14) | POST | 🟢 |🟡 | `/querier.v1.QuerierService/LabelNames` |
| [SelectSeries](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L24) | POST | 🟢 |🟡 | `/querier.v1.QuerierService/SelectSeries` |
| [SelectMergeStacktraces](https://github.com/metrico/qryn/blob/master/pyroscope/proto/querier.proto#L18) | POST | 🟢 |🟡 | `/querier.v1.QuerierService/SelectMergeStacktraces` |

<!-- tabs:end -->

## 🔻 Profiling Agents

<!-- tabs:start -->
## ** Continuous Profiling **

### Application Performance Profiling
Pyroscope is a continuous profiling format that allows you to analyze the performance of your applications.<br>
Ingestion through the **qryn otel-collector** offers two primary methods: 

#### SDK Instrumentation
The Pyroscope SDKs provide you with the capability to directly instrument your application, enabling more accurate profiling. This feature is particularly useful when you desire complete control over the profiling process or when your application is written in a language supported by the SDKs, such as _Java, Python, .NET, and more_. 

#### Auto-Instrumentation
[Grafana Agent](https://grafana.com/docs/pyroscope/latest/configure-client/grafana-alloy/go_pull/) can run alongside your application to collect profiling data at regular intervals. With the **eBPF** profiling option, things get even easier as there's no need for pull or push mechanisms.

#### Ingestion
Both methods use the qryn [otel-collector](https://github.com/metrico/otel-collector) to ingest data to the database

![qryn_pyroscope_flow](https://github.com/metrico/qryn-docs/assets/1423657/e1d4232a-d2c4-467d-8b34-fae0ca95b42e)



## ** OTEL Collector **

<a id=otel name=otel></a>

![qryn_pyroscope_flow](https://github.com/metrico/qryn-docs/assets/1423657/e1d4232a-d2c4-467d-8b34-fae0ca95b42e)

The qryn [otel-collector](https://github.com/metrico/otel-collector) supports ingestion of Pyroscope events alongside logs, metrics and traces.
Here is the minimalistic docker-compose file for the profiles ingestion
```yml
version: '2.23'

services:
  otel-collector:
    container_name: otel-collector
    image: ghcr.io/metrico/qryn-otel-collector:latest
    volumes:
      - ./otel-collector-config.yaml:/etc/otel/config.yaml
    ports:
      - "8062:8062"
    restart: on-failure
  clickhouse:
    container_name: clickhouse
    hostname: clickhouse
    image: clickhouse/clickhouse-server:latest
    restart: on-failure
    ports:
      - "8123:8123"
      - "9000:9000"
  qryn:
    container_name: qryn
    hostname: qryn
    image: ghcr.io/metrico/qryn:latest
    restart: on-failure
    ports:
      - "3100:3100"
    environment:
      - CLICKHOUSE_SERVER=clickhouse
      - DEBUG=true
      - CLICKHOUSE_DB=qryn
    depends_on:
      - clickhouse
  grafana:
    container_name: grafana
    image: grafana/grafana:latest
    restart: on-failure
    hostname: grafana
    volumes:
      - ./datasource.yml:/etc/grafana/provisioning/datasources/datasource.yaml
    ports:
      - "3000:3000"
```

There should be two extra files in the folder with the docker-compose file. The first one is the `datasource.yml` for grafana:
```yml
# config file version
apiVersion: 1

datasources:
  - name: Qryn-Prof
    access: proxy
    url: http://qryn:3100
    editable: true
    type: phlare
```

The second one is the `otel-collector-config.yaml`:

##### pyroscope `otel-collector-config.yaml`
The following example enables Pyroscope ingestion using the qryn [otel-collector](Pyroscope). Integrate in your existing configuration.

```yml
receivers:
  pyroscopereceiver:
exporters:
  clickhouseprofileexporter:
    dsn: clickhouse://clickhouse:9000/qryn
service:
  pipelines:
    logs:
      receivers: [pyroscopereceiver]
      exporters: [clickhouseprofileexporter]
```

?> _That's it!_ You're now ready to ingest _continuous profiling_ into **qryn** using OTLP Collector! 

## ** SDK Clients **

<!-- tabs:start -->

### ** Java **
<a id=java name=java></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/b65d9767-040f-423d-9bde-0a21c4bebb9c ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/java/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/java/rideshare)

### ** Golang **
<a id=golang name=golang></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/49df8fba-3f6e-4e3f-8790-307e5eaefcaf ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/go_push/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/golang-push)

### ** Python **
<a id=python name=python></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/488d0220-1bc9-479d-94c5-4a11668196f2 ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/python/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/python)

### ** NodeJS **
<a id=nodejs name=nodejs></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/dddb65eb-65a9-4b98-bfd9-d005e1e54fa8 ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/nodejs/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/nodejs/express)

### ** Rust **
<a id=rust name=rust></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/d7c29289-91a9-415a-9aa4-cf787868507b ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/rust/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/rust/rideshare)

### ** eBPF **
<a id=eBPF name=eBPF></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/56d73c88-2d1b-4551-9f0c-d88951b3789e ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/grafana-alloy/ebpf/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/grafana-agent-auto-instrumentation/ebpf)

### ** Ruby **
<a id=ruby name=ruby></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/124fac51-7f17-4385-a8c4-f5711e634804 ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/ruby/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/ruby)

### ** Dotnet **
<a id=dotnet name=dotnet></a>

![image](https://github.com/metrico/qryn-docs/assets/1423657/78059424-d4c1-4ff2-86e7-5c970efec97e ':size=100')

- [Documentation](https://grafana.com/docs/pyroscope/latest/configure-client/language-sdks/dotnet/)
- [Examples](https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/dotnet)


<!-- tabs:end -->


<!-- tabs:end -->
