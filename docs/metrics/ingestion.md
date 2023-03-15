# 🔻 Metric Ingestion

The following protocol APIs are supported for ingesting metrics:
<!-- tabs:start -->
## ** Prometheus **

<a id=prometheus name=prometheus></a>

![image](https://user-images.githubusercontent.com/1423657/184496973-9f46e551-872d-4a25-877c-51a2e5f53e84.png ':size=100')


?> **qryn** natively supports the Prometheus _Remote Write_ API for storing metrics.

#### API Endpoints

See Supported API Endpoints [here](/support.md#prometheus-api).

<!--
![image](https://user-images.githubusercontent.com/1423657/150554482-988b5ccf-225c-4919-bc31-6fa18bef6f53.png)
--> 

#### Testing

Use the [promremote](https://github.com/m3dbx/prometheus_remote_client_golang) client for testing:
```
promremote -t=__name__:foo_bar -t=biz:baz -d=now,1415.92
```

### Usage

#### Write
Store data from Prometheus instances using the [remote_write API](https://prometheus.io/docs/prometheus/latest/storage/#remote-storage-integrations)

```
# Remote write configuration
remote_write:
  - url: "http://qryn:3100/api/v1/prom/remote/write"
```

!> Replace the **qryn** URL from the example to match your actual deployment!

?> _That's it!_ You are now ingesting Prometheus metrics in **qryn**

#### Read

Stored Metrics in **qryn** are unified into the same format and can be accessed using the _(unwrap_value)_ function - or through the experimental _PromQL emulation APIs_

##### Unwrap Expressions

The following `LogQL` query will access metrics inserted by Prometheus or any other insert agent.
```
rate({label1="val1"}| unwrap int_valued_label [1s]) by (another_label)
```

1) `{label1="val1"}`
  Fetch all log lines matching label filters.
2) `<expr> | unwrap int_valued_label`
  Use the extracted label int_valued_label as sample values instead of log lines for the subsequent range aggregation.
3) `rate(<expr> [1s])`
  Calculates the number of entries per second. 
4) `by (another_label)`
  Groups the resulting series by the selected label(s).

##### Supported Unwrap functions

- `rate(unwrapped-range)`: calculates per second rate of all values in the specified interval.
- `sum_over_time(unwrapped-range)`: the sum of all values in the specified interval.
- `avg_over_time(unwrapped-range)`: the average value of all points in the specified interval.
- `max_over_time(unwrapped-range)`: the maximum value of all points in the specified interval.
- `min_over_time(unwrapped-range)`: the minimum value of all points in the specified interval
- `first_over_time(unwrapped-range)`: the first value of all points in the specified interval
- `last_over_time(unwrapped-range)`: the last value of all points in the specified interval
- [Aggregation operators](https://github.com/lmangani/cLoki/wiki/LogQL-Supported-Queries#aggregation-operators) over unwrapped expressions are supported.

##### NOTES:

In Grafana queries, use $__interval for unwrap, ie: unwrap data [$__interval]


![image](https://user-images.githubusercontent.com/1423657/150602556-2306aebc-3d5c-4226-af85-ed3614798222.png)

* More Examples: [LogQL Range Aggregations](https://grafana.com/blog/2021/01/11/how-to-use-logql-range-aggregations-in-loki/)

?> _That's it!_ You are now analyzing Prometheus data using **qryn**



## ** Influx **

<a id=influx name=influx></a>

![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')

?> **qryn** implements the Influx v2 Write API subset for ingestion of Metrics _(and [logs](logs/ingestion#influx))_


### Telegraf

![image](https://user-images.githubusercontent.com/1423657/196438621-1f025393-c15d-4b0f-bf47-0d92ca2ecbec.png ':size=100')

[Telegraf](https://docs.influxdata.com/telegraf/v1.24/install/) can be used to easily send metrics and logs to **qryn**<br>


### API Usage

See Supported API Endpoints [here](/support.md#influx-api).

#### Write

Metrics can be inserted by Telegraf or any Influx `line_protocol` compatible agent.

##### Metrics
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```  
  
##### Logs
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'syslog,appname=myapp,facility=console,host=myhost,hostname=myhost,severity=warning facility_code=14i,message="warning message here",severity_code=4i,procid="12345",timestamp=1434055562000000000,version=1'
```  

!> Replace the **qryn** URL from the example to match your actual deployment!

?> _That's it!_ You are now inserting metrics from Influx clients in **qryn**

#### Read

Stored Metrics in **qryn** are unified into the same format and can be accessed using the _(unwrap_value)_ function

##### Unwrap Expressions

The following `LogQL` query will access metrics inserted by Influx clients or any other insert agent.
```
rate({label1="val1"}| unwrap int_valued_label [1s]) by (another_label)
```

1) `{label1="val1"}`<br>
  Fetch all log lines matching label filters.
2) `<expr> | unwrap int_valued_label`<br>
  Use the extracted label int_valued_label as sample values instead of log lines for the subsequent range aggregation.
3) `rate(<expr> [1s])`<br>
  Calculates the number of entries per second. 
4) `by (another_label)`<br>
  Groups the resulting series by the selected label(s).

##### Supported Unwrap functions

- `rate(unwrapped-range)`: calculates per second rate of all values in the specified interval.
- `sum_over_time(unwrapped-range)`: the sum of all values in the specified interval.
- `avg_over_time(unwrapped-range)`: the average value of all points in the specified interval.
- `max_over_time(unwrapped-range)`: the maximum value of all points in the specified interval.
- `min_over_time(unwrapped-range)`: the minimum value of all points in the specified interval
- `first_over_time(unwrapped-range)`: the first value of all points in the specified interval
- `last_over_time(unwrapped-range)`: the last value of all points in the specified interval
- [Aggregation operators](https://github.com/lmangani/cLoki/wiki/LogQL-Supported-Queries#aggregation-operators) over unwrapped expressions are supported.

##### NOTES:

In Grafana queries, use $__interval for unwrap, ie: unwrap data [$__interval]


![image](https://user-images.githubusercontent.com/1423657/150602556-2306aebc-3d5c-4226-af85-ed3614798222.png)

* More Examples: [LogQL Range Aggregations](https://grafana.com/blog/2021/01/11/how-to-use-logql-range-aggregations-in-loki/)

?> _That's it!_ You are now analyzing Influx data using **qryn**

## ** LogQL **

<a id=logql name=logql></a>

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

_Grafana Loki_ does not allow inserting metrics through its LogQL API - but **qryn** does 👍

We can insert labeled metrics using the `value` parameter in a _LogQL stream_:

#### Metrics
```bash
curl -i -XPOST -H "Content-Type: application/json" http://qryn:3100/loki/api/v1/push \
     --data '{"streams":[{"labels":"{\"__name__\":\"my_metric\"}","entries":[{"timestamp":"2022-08-08T16:00:06.944Z", "value":100}]}]}'
```
```json
{
    "streams": [
        {
            "labels": "{\"__name__\":\"my_metric\"}",
            "entries": [
                {
                    "timestamp":"1545840006945",
                    "value": 100
                }
            ]
        }
    ]
}
```

!> Replace the **qryn** URL from the example to match your actual deployment!

?> _That's it!_ You are now inserting metrics throug LogQL in **qryn**


## ** OTEL Collector **

<a id=grafana name=grafana></a>

![image](https://user-images.githubusercontent.com/1423657/196469086-3d85efd5-7ef9-4d42-a677-5591470b7cae.png ':size=200')

The [OTEL Collector]([https://opentelemetry.io/docs/collector/](https://github.com/metrico/otel-collector)) offers a vendor-agnostic implementation of how to receive, process and export telemetry data. It removes the need to run, operate, and maintain multiple agents/collectors. 

This works with improved scalability and supports a large variety of open-source observability data formats _(e.g. Jaeger, Prometheus, Fluent Bit, etc.)_ and allows aggregating and sending traces to **qryn** using the _Tempo API_

For large scale ingestion, we suggest using the dedicated [qryn opentelemetry distribution](https://github.com/metrico/otel-collector) for ClickHouse.

### Examples

![image](https://user-images.githubusercontent.com/1423657/217615085-cb5c7858-798e-4896-8ad6-3b88cfb16c6c.png)


#### qryn collector
```
otel-collector:
    container_name: otel-collector
    image: ghcr.io/metrico/qryn-otel-collector:latest
    volumes:
      - ./otel-collector-config.yaml:/etc/otel/config.yaml
    ports:
      - "4317:4317"     # OTLP gRPC receiver
      - "4318:4318"     # OTLP HTTP receiver
      - "14250:14250"   # Jaeger gRPC
      - "14268:14268"   # Jaeger thrift HTTP
      - "9411:9411"     # Zipkin port
      - "24224:24224".  # Fluent Forward
    restart: on-failure
```

###### qryn collector `config.taml`
The following example enables all the supported formats at once. Filter the required sections to build your configuration.

```yml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
  jaeger:
    protocols:
      grpc:
        endpoint: 0.0.0.0:14250
      thrift_http:
        endpoint: 0.0.0.0:14268
  zipkin:
    endpoint: 0.0.0.0:9411
  fluentforward:
    endpoint: 0.0.0.0:24224
  prometheus:
    config:
      scrape_configs:
        - job_name: 'otel-collector'
          scrape_interval: 5s
          static_configs:
            - targets: ['exporter:8080']
processors:
  batch:
    send_batch_size: 10000
    timeout: 5s
  memory_limiter:
    check_interval: 2s
    limit_mib: 1800
    spike_limit_mib: 500
  resourcedetection/system:
    detectors: ['system']
    system:
      hostname_sources: ['os']
  resource:
    attributes:
      - key: service.name
        value: "serviceName"
        action: upsert
  spanmetrics:
    metrics_exporter: otlp/spanmetrics
    latency_histogram_buckets: [100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms]
    dimensions_cache_size: 1500
  servicegraph:
    metrics_exporter: otlp/spanmetrics
    latency_histogram_buckets: [100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms]
    dimensions: [cluster, namespace]
    store:
      ttl: 2s
      max_items: 200
  metricstransform:
    transforms:
      - include: calls_total
        action: update
        new_name: traces_spanmetrics_calls_total
      - include: latency
        action: update
        new_name: traces_spanmetrics_latency
exporters:
  qryn:
    dsn: tcp://clickhouse-server:9000/cloki?username=default&password=*************
    timeout: 10s
    sending_queue:
      queue_size: 100
    retry_on_failure:
      enabled: true
      initial_interval: 5s
      max_interval: 30s
      max_elapsed_time: 300s
    logs:
       format: json
  otlp/spanmetrics:
    endpoint: localhost:4317
    tls:
      insecure: true
extensions:
  health_check:
  pprof:
  zpages:
  memory_ballast:
    size_mib: 1000

service:
  extensions: [pprof, zpages, health_check]
  pipelines:
    logs:
      receivers: [fluentforward, otlp]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
    traces:
      receivers: [otlp, jaeger, zipkin]
      processors: [memory_limiter, resourcedetection/system, resource, spanmetrics, servicegraph, batch]
      exporters: [qryn]
    # for align with https://grafana.com/docs/tempo/latest/metrics-generator/span_metrics/#how-to-run
    metrics/spanmetrics:
      receivers: [otlp]
      processors: [metricstransform]
      exporters: [qryn]
    metrics:
      receivers: [prometheus]
      processors: [memory_limiter, resourcedetection/system, resource, batch]
      exporters: [qryn]
```

?> _That's it!_ You're now _tracing spans to **qryn** using OTLP Collector! 


## ** SNMP **

![image](https://user-images.githubusercontent.com/1423657/196438621-1f025393-c15d-4b0f-bf47-0d92ca2ecbec.png ':size=200')

[Telegraf](https://docs.influxdata.com/telegraf/v1.24/install/) can be used to easily ingest SNMP metrics into **qryn**

The [snmp input](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/snmp/README.md) plugin uses polling to gather metrics from SNMP agents. Support for gathering individual OIDs as well as complete SNMP tables is included.

#### input config

```yaml
# Retrieves SNMP values from remote agents
[[inputs.snmp]]
  ## Agent addresses to retrieve values from.
  ##   format:  agents = ["<scheme://><hostname>:<port>"]
  ##   scheme:  optional, either udp, udp4, udp6, tcp, tcp4, tcp6.
  ##            default is udp
  ##   port:    optional
  ##   example: agents = ["udp://127.0.0.1:161"]
  ##            agents = ["tcp://127.0.0.1:161"]
  ##            agents = ["udp4://v4only-snmp-agent"]
  agents = ["udp://127.0.0.1:161"]

  ## Timeout for each request.
  # timeout = "5s"

  ## SNMP version; can be 1, 2, or 3.
  # version = 2

  ## Unconnected UDP socket
  ## When true, SNMP reponses are accepted from any address not just
  ## the requested address. This can be useful when gathering from
  ## redundant/failover systems.
  # unconnected_udp_socket = false 

  ## Path to mib files
  ## Used by the gosmi translator.
  ## To add paths when translating with netsnmp, use the MIBDIRS environment variable
  # path = ["/usr/share/snmp/mibs"]

  ## SNMP community string.
  # community = "public"

  ## Agent host tag
  # agent_host_tag = "agent_host"

  ## Number of retries to attempt.
  # retries = 3

  ## The GETBULK max-repetitions parameter.
  # max_repetitions = 10

  ## SNMPv3 authentication and encryption options.
  ##
  ## Security Name.
  # sec_name = "myuser"
  ## Authentication protocol; one of "MD5", "SHA", "SHA224", "SHA256", "SHA384", "SHA512" or "".
  # auth_protocol = "MD5"
  ## Authentication password.
  # auth_password = "pass"
  ## Security Level; one of "noAuthNoPriv", "authNoPriv", or "authPriv".
  # sec_level = "authNoPriv"
  ## Context Name.
  # context_name = ""
  ## Privacy protocol used for encrypted messages; one of "DES", "AES", "AES192", "AES192C", "AES256", "AES256C", or "".
  ### Protocols "AES192", "AES192", "AES256", and "AES256C" require the underlying net-snmp tools
  ### to be compiled with --enable-blumenthal-aes (http://www.net-snmp.org/docs/INSTALL.html)
  # priv_protocol = ""
  ## Privacy password used for encrypted messages.
  # priv_password = ""

  ## Add fields and tables defining the variables you wish to collect.  This
  ## example collects the system uptime and interface variables.  Reference the
  ## full plugin documentation for configuration details.
  [[inputs.snmp.field]]
    oid = "RFC1213-MIB::sysUpTime.0"
    name = "uptime"

  [[inputs.snmp.field]]
    oid = "RFC1213-MIB::sysName.0"
    name = "source"
    is_tag = true

  [[inputs.snmp.table]]
    oid = "IF-MIB::ifTable"
    name = "interface"
    inherit_tags = ["source"]

    [[inputs.snmp.table.field]]
      oid = "IF-MIB::ifDescr"
      name = "ifDescr"
      is_tag = true
```

#### output config
Use the `http` output with `prometheusremotewrite` format to write metrics into **qryn**

```yml
  [[outputs.http]]
    url = "http://qryn:3100/api/prom/remote/write"
    data_format = "prometheusremotewrite"

    [outputs.http.headers]
      Content-Type = "application/x-protobuf"
      Content-Encoding = "snappy"
      X-Prometheus-Remote-Write-Version = "0.1.0"
```

Start Telegraf and explore your SNMP metrics using Grafana

![image](https://user-images.githubusercontent.com/1423657/196466648-20e5f638-b146-443b-b98a-29dcd1b951fa.png)


?> _That's it!_ You are now ingesting SNMP metrics into **qryn**

## ** Curl **

No API guide would be complete without a Curl example - _and we're no exception!_

We can insert labeled metrics using the `value` parameter in a _LogQL stream_:

#### Insert Metrics
```
curl -i -XPOST -H "Content-Type: application/json" http://qryn:3100/loki/api/v1/push \
     --data '{"streams":[{"labels":"{\"__name__\":\"test_metric\"}","entries":[{"timestamp":"'"$(date --utc +%FT%T.%3NZ)"'", "value":100}]}]}'
```
```json
{
    "streams": [
        {
            "labels": "{\"__name__\":\"test_metric\"}",
            "entries": [
                {
                    "timestamp":"1545840006945",
                    "value": 100
                }
            ]
        }
    ]
}
```
!> Replace the **qryn** URL from the example to match your actual deployment!

?> _That's it!_ You are now inserting metrics throug Curl in **qryn**

<!-- tabs:end -->
