# Metric Ingestion

Ingesting metrics with **qryn** is easy and painless. Just pick an integration and you're set!


### Metrics
The following protocol APIs are supported for ingesting metrics:
<!-- tabs:start -->
## ** Prometheus **

### Prometheus Remote Write

![image](https://user-images.githubusercontent.com/1423657/184495268-d83cbc0a-a182-4fe4-9271-cb64d7a52f21.png ':size=100')

**qryn** natively supports the Prometheus Remote Write API _(Protobuf WriteRequest format)_ for storing metrics.

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


#### Read
Stored Metrics can be accessed using the qryn _(unwrap_value)_ function or through the _PromQL emulation APIs_

##### Unwrap Expressions
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

Examples: [LogQL Range Aggregations](https://grafana.com/blog/2021/01/11/how-to-use-logql-range-aggregations-in-loki/)

## ** LogQL **
* LogQL PUSH _(unwrap_metric)_
## ** Influx **
* Influx _(line protocol)_
<!-- tabs:end -->
