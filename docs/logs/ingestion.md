# Log Ingestion

Ingesting logs with **qryn** is easy and painless. Just pick an integration and you're set!

### Integrations

The **qryn** LogQL API and is 100% compatible with Grafana Loki for data ingestion. 

This means _any Loki example_ can be used with **qryn** without modifications:

* [Vector](https://vector.dev/docs/reference/configuration/sinks/loki/) + [Examples](https://component-pages--vector-project.netlify.app/guides/integrate/sources/http/loki/)
* [Grafana Agent]()https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-agent/))+ [Examples](https://grafana.com/docs/grafana-cloud/quickstart/logs_agent_linuxnode/)
* [Promtail](https://grafana.com/docs/grafana-cloud/data-configuration/logs/collect-logs-with-promtail/)
* [Fluentd](https://grafana.com/docs/loki/latest/clients/fluentd/) + [Examples](https://grafana.com/blog/2022/06/23/how-to-send-logs-to-grafana-loki-with-the-opentelemetry-collector-using-fluent-forward-and-filelog-receivers/#:~:text=Set%20up%20a%20Grafana%20Cloud,click%20the%20Send%20Logs%20button.)
* [Logstash](https://grafana.com/docs/loki/latest/clients/logstash/)

----

The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->

## ** LogQL **

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

### Grafana Agent
Install [Grafana Agent](https://grafana.com/docs/grafana-cloud/data-configuration/agent/) using the official documentation.

The agent configuration is stored in `/etc/grafana-agent.yaml`

This example will scrape and send info from all logs in /var/log that end in log. They are labeled with varlogs as the job and job_name
```
loki:
  configs:
  - name: default
    positions:
      filename: /tmp/positions.yaml
    scrape_configs:
      - job_name: varlogs
        static_configs:
          - targets: [localhost]
            labels:
              job: varlogs
              __path__: /var/log/*log
    clients:
      - url: http://qryn.host:3100/loki/api/v1/push
        basic_auth:
          username: <User>
          password: <Password>
```
 
Here is another example, scraping logs for a minecraft server with logs stored in a subdirectory of the /home directory of a special minecraft user.
```
      - job_name: minecraftlog
        static_configs:
          - targets: [localhost]
            labels:
              job: minecraft
              __path__: /home/MCuser/minecraft/logs/latest.log
```
Anytime you change the agent configuration, you must restart the agent for the new configuration to take effect.
```
sudo systemctl restart grafana-agent.service
``` 

## ** Elastic **

![image](https://user-images.githubusercontent.com/1423657/184496304-4f35a365-efdc-4dca-9771-6b7b1deb9ae3.png ':size=100')

**qryn** implements an experimental Elastic API subset for ingestion.

* Elastic _(_index, _bulk)_

## ** Influx **

![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')

**qryn** implements the Influx v2 Write API subset for ingestion of Logs _(and metrics)_

#### CURL Examples

##### Logs
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
  --data-raw 'syslog,appname=myapp,facility=console,host=myhost,hostname=myhost,severity=warning facility_code=14i,message="warning message here",severity_code=4i,procid="12345",timestamp=1434055562000000000,version=1'
```  
##### Metrics
```bash
curl -i -XPOST 'http://qryn:3100/influx/api/v2/write' \
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
