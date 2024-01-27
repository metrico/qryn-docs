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

![qryn_pyroscope_flow](https://github.com/metrico/qryn-docs/assets/1423657/e1d4232a-d2c4-467d-8b34-fae0ca95b42e)


#### qryn collector
```
otel-collector:
    container_name: otel-collector
    image: ghcr.io/metrico/qryn-otel-collector:latest
    volumes:
      - ./otel-collector-config.yaml:/etc/otel/config.yaml
    ports:
      - "8062 :8062 "   # Pyroscope gRPC receiver
    restart: on-failure
```

###### pyroscope `otel-collector-config.taml`
The following example enables Pyroscope ingestion using the qryn-collector. Integrate in your existing configuration.

```yml
receivers:
  pyroscopereceiver:
exporters:
  clickhouseprofileexporter:
    dsn: clickhouse://localhost:9000/qryn
service:
  pipelines:
    logs:
      receivers: [pyroscopereceiver]
      exporters: [clickhouseprofileexporter]
```

?> _That's it!_ You're now ready to ingest _continuous profiling into **qryn** using OTLP Collector! 

## ** Pyroscope Java **

<a id=java name=java></a>

You can use qryn with the [Pyroscope Java](https://github.com/grafana/pyroscope-java) client

The agent is distributed as a single JAR file `pyroscope.jar` containing native async-profiler libraries for:

* Linux on x64
* Linux on ARM64
* MacOS on x64
* MacOS on ARM64

<!-- tabs:end -->
