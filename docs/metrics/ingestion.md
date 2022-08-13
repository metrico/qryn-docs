# Metric Ingestion

Ingesting metrics with **qryn** is easy and painless. Just pick an integration and you're set!


### Metrics
The following protocol APIs are supported for ingesting metrics:
<!-- tabs:start -->
## ** Prometheus **

### Prometheus Remote Write

![image](https://user-images.githubusercontent.com/1423657/184495268-d83cbc0a-a182-4fe4-9271-cb64d7a52f21.png ':size=100')

**qryn** natively supports the Prometheus _Remote Write_ API for storing metrics.

#### API Endpoints
- [x] `api/v1/prom/remote/write`
- [x] `api/prom/remote/write` _(alias)_

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
  - url: "http://qryn:3100/api/v1/write"
```

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


## ** LogQL **

![image](https://user-images.githubusercontent.com/1423657/184496222-ca95d80c-906f-4c77-a963-86f0b27a56b0.png ':size=100')

Loki doesn't allow inserting metrics through its API - but **qryn** does 👍

Both below format variations are accepted to insert labeled metrics using the `value` parameter in a stream:

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

?> _That's it!_ You are now inserting metrics throug LogQL in **qryn**


## ** Influx **

![image](https://user-images.githubusercontent.com/1423657/184496174-aca323dd-f40e-489a-a584-fa7348c0eab0.png ':size=100')

**qryn** implements the Influx v2 Write API subset for ingestion of Metrics _(and logs)_

### Usage

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

#### Read

Stored Metrics in **qryn** are unified into the same format and can be accessed using the _(unwrap_value)_ function

##### Unwrap Expressions

The following `LogQL` query will access metrics inserted by Influx clients or any other insert agent.
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

?> _That's it!_ You are now analyzing Influx data using **qryn**

<!-- tabs:end -->
