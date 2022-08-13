# Log Ingestion

Ingesting logs with **qryn** is easy and painless. Just pick an integration and you're set!


### Integrations
The following protocol APIs are supported for ingesting logs:
<!-- tabs:start -->

## ** LogQL **
* LogQL PUSH

## ** Elastic **
* Elastic _(_index, _bulk)_

## ** Influx **
* Influx _(line protocol syslog)_

## ** ClickHouse MV **

<img src="https://avatars.githubusercontent.com/u/54801242?s=200&v=4" width=100 />

ClickHouse populates the `system.text_log` table with its internal logs. Without using any client or file reading application, we can use ClickHouse Materialized View + URL Engine to ship our logs to **qryn** as soon as they are generated.

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
@lmangani

```

!> Replace the **qryn** URL from the example to match your actual deployment!

?> That's it! You're now shipping logs straight off your ClickHouse core

<!-- tabs:end -->
