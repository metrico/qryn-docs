# 🔻 Profiling Ingestion

The following protocol APIs are supported for ingesting continuous profiling events:

<!-- tabs:start -->
## ** OTEL Collector **

<a id=grafana name=grafana></a>

![image](https://user-images.githubusercontent.com/1423657/196469086-3d85efd5-7ef9-4d42-a677-5591470b7cae.png ':size=200')

The [OTEL Collector]([https://opentelemetry.io/docs/collector/](https://github.com/metrico/otel-collector)) offers a vendor-agnostic implementation of how to receive, process and export telemetry data. It removes the need to run, operate, and maintain multiple agents/collectors. 

This works with improved scalability and supports a large variety of open-source observability data formats _(e.g. pprof, etc.)_ and allows aggregating and sending profiling data to **qryn** using the _Pyroscope API_

This guide is based on the [qryn opentelemetry distribution](https://github.com/metrico/otel-collector) for qryn + clickhouse.

### Examples

#### qryn otel-collector

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

## ** Pyroscope Java **

<a id=java name=java></a>

You can use qryn with the [Pyroscope Java](https://github.com/grafana/pyroscope-java) client

The agent is distributed as a single JAR file `pyroscope.jar` containing native async-profiler libraries for:

* Linux on x64
* Linux on ARM64
* MacOS on x64
* MacOS on ARM64

<!-- tabs:end -->
