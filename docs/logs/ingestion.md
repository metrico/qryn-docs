# Log Ingestion

Ingesting logs with **qryn** is easy and painless. Just pick an integration and you're set!


### Integrations
The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->

## ** LogQL **

**qryn** implements a LogQL API and is 100% compatible with Grafana Loki for ingestion. Any Loki example can be used with **qryn**!

* Vector.io
  * [Loki sink](https://vector.dev/docs/reference/configuration/sinks/loki/)
  * [Examples](https://component-pages--vector-project.netlify.app/guides/integrate/sources/http/loki/)
* Grafana Agent
  * [Loki sink]()https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-agent/))
  * [Examples](https://grafana.com/docs/grafana-cloud/quickstart/logs_agent_linuxnode/)
* Promtail
  * [Examples](https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-promtail/)
* Fluentd
  * [Loki plugin](https://grafana.com/docs/loki/latest/clients/fluentd/)
  * [Examples](https://grafana.com/blog/2022/06/23/how-to-send-logs-to-grafana-loki-with-the-opentelemetry-collector-using-fluent-forward-and-filelog-receivers/#:~:text=Set%20up%20a%20Grafana%20Cloud,click%20the%20Send%20Logs%20button.)
* Logstash
  * [Examples](https://grafana.com/docs/loki/latest/clients/logstash/)
  

## ** Elastic **
**qryn** implements an experimental Elastic API subset for ingestion.

* Elastic _(_index, _bulk)_

## ** Influx **
**qryn** implements the Influx v2 Write API subset for ingestion of Logs _(and metrics)_

#### CURL Examples

##### Logs
```bash
curl -i -XPOST 'http://localhost:3100/influx/api/v2/write' \
  --data-raw 'syslog,appname=myapp,facility=console,host=myhost,hostname=myhost,severity=warning facility_code=14i,message="warning message here",severity_code=4i,procid="12345",timestamp=1434055562000000000,version=1'
```  
##### Metrics
```bash
curl -i -XPOST 'http://localhost:3100/influx/api/v2/write' \
  --data-raw 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```  
  

## ** ClickHouse MV **

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=100 />

ClickHouse populates the `system.text_log` table with its internal logs. Without using any client or file reading application, we can use ClickHouse Materialized View + URL Engine to ship our logs to **qryn** as soon as they are generated using the InfluxDB line format protocol

### Requirements
- Enable [opentelemetry](https://clickhouse.com/docs/en/operations/opentelemetry/) settings in your ClickHouse `config.xml`


### Create Materialized View

The following MV will push logs to the **qryn** `/influx/api/v2/write` API using the ClickHouse **URL engine**
```sql
CREATE MATERIALIZED VIEW default.influx_log_send
ENGINE = URL('http://qryn:3100/influx/api/v2/write', 'LineAsString')
AS SELECT format('syslog,level={0},logger_name={1} message="{2}" {3}', 
  toString(level), 
  replaceRegexpAll(toString(logger_name), '[^a-zA-Z0-9_]', '_'), 
  replaceAll(replaceRegexpAll(message, '["\\\\]', '\x00\\0'), '\x00', '\\'), 
  toString(toUnixTimestamp64Nano(event_time_microseconds))) FROM system.text_log;
```

!> Replace the **qryn** URL from the example to match your actual deployment!

_Done?_ Just wait a few seconds for the logs to flush and they will appear in **qryn**


?> That's it! You're now shipping logs straight off your ClickHouse core!

<!-- tabs:end -->
