# Examples

This section illustrates a number of example queries to help familiarize with **qryn**

### LogQL

LogQL is a PromQL-inspired query language acting like a distributed grep with powerful _filters and aggregations_.

There are two types of _LogQL_ queries:

  * [Log queries](https://grafana.com/docs/loki/latest/logql/log_queries/) returning the contents of log lines as streams.
  * [Metric queries](https://grafana.com/docs/loki/latest/logql/metric_queries/) that convert logs into value matrices.

**LogQL** queries combine a mandatory **Stream Selectors** _fetching all relevant logs by fingerprint_, followed by optional **Filter Expressions** _reducing and interpolating the returned dataset_.


<!-- tabs:start -->

## ** Log Stream selector **

The _stream selector_ block is always mandatory and determines which log streams should be included in your query using operators

```
{app="clickhouse"}
```

In this example, logs that have a label `app` matching `clickhouse` will be included in the results

### Log Stream Selectors Operators

The following label operators are supported in label selectors:

* `= `: equals
* `!=` : not equals
* `=~` : regex matches
* `!~` : regex does not match

#### Examples

Return all log lines for the job systemd-journal

```
{job="systemd-journal"}
```

-------

Return all log lines for the unit ssh.service

```
{unit="ssh.service"}
```

-------

Return all log lines for the job systemd-journal and the unit cron.service

```
{job="systemd-journal",unit="cron.service"}
```

-------

Show all log lines for 2 jobs with different names

```
{job=~"qryn/systemd-journal|systemd-journal"}
```

-------

## ** Filter Expression **

Once labels are selected the resulting data is be filtered using filter expressions.

### Filter Expressions Operators

The following filter operators are supported in expressions:

* `|=` : equals
* `!=` : not equals
* `|~` : regex matches
* `!~` : regex does not match

#### Examples

Return lines including the text "error"

```
{job="systemd-journal"} |= "error"
```

-------

Return lines not including the text "error"

```
{job="systemd-journal"} != "error"
```

-------

Return lines including the text "error" or "info" using regex

```
{job="systemd-journal"} |~ "error|info"
```

-------

Return lines not including the text "error" or "info" using regex

```
{job="systemd-journal"} !~ "error|info"
```

-------

Return lines including the text "error" but not including "info"

```
{job="systemd-journal"} |= "error" != "info"
```

-------

Return lines including the text "Invalid user" and including ("default" or "qryn") using regex

```
{job="systemd-journal"} |~ "Invalid user (default|qryn)"
```

-------

Return lines including the text "status 403" or "status 503" using regex

```
{job="systemd-journal"} |~ "status [45]03"
```

-------


<!-- tabs:end -->


We can use operations on both the log **stream selectors** and **filter expressions** to refine them.

<!-- tabs:start -->

### ** Range Vectors **

Just like Prometheus, LogQL supports aggregating data over a selected ranges to transform it into an instance vectors.

The currently supported functions are:

* `count_over_time` : Shows the total count of log lines for time range
* `rate` : Similar as count_over_time but converted to number of entries per second
* `bytes_over_time` : Number of bytes in each log stream in the range
* `bytes_rate` : Similar to bytes_over_time but converted to number of bytes per second

The folllowing example counts all the log lines within the last five minutes for the Clickhouse job.

```
count_over_time({job=”clickhouse”}[5m])
```

Extend the example to the per-second rate of all non-timeout errors within the last ten seconds for the job.

```
rate( ( {job="clickhouse"} |= "error" != "timeout)[10s] ) )
```


#### Examples

The count of jobs at 1 minutes time intervals

```
count_over_time({job="systemd-journal"}[1m])
```

-------

The rate of logs per minute. Rate is similar to count_over_time but shows the entries per second.

```
rate({job="systemd-journal"}[1m])
```

-------

##### Info

```
rate = count_over_time / 60 / range(m)

eg,

12 / 60 / 2 = 0.1
```

The count of errors at 1h time intervals

```
count_over_time({job="systemd-journal"} |= "error" [1h])
```


### ** Aggregate Functions **

Like PromQL, LogQL supports a subset of built-in aggregation operators that can be used to aggregate elements of a single vector, resulting in a new vector of fewer elements with aggregated values:

* `sum` : Calculate the total of all instance vectors in the range at time
* `min` : Show the minimum value from all instance vectors in the range at time
* `max` : Show the maximum value from all instance vectors in the range at time
* `avg` : Calculate the average of the values from all instance vectors in the range at time
* `stddev` : Calculate the standard deviation of the values from all instance vectors in the range at time
* `stdvar` : Calculate the standard variance of the values from all instance vectors in the range at time
* `count` : Count the number of elements all all instance vectors in the range at time

#### Examples

Calculate the total of all instance vectors in the range at time

```
sum(count_over_time({job="systemd-journal"}[1m]))
```

-------

Show the minimum value from all instance vectors in the range at time

```
min(count_over_time({job="systemd-journal"}[1m]))
```

-------

Show the maximum value from all instance vectors in the range at time

```
max(count_over_time({job="systemd-journal"}[1m]))
```

-------

### Aggregate Group

Convert an instance vector into a range vector organized by unit

#### Examples

Group a single log stream by unit

```
sum(count_over_time({job="systemd-journal"}[1m])) by (unit)
```

-------

Group multiple log streams by job

```
sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal"}[1m])) by (job)
```

-------

Group multiple log streams and specific unit by job

```
sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal", unit="cron.service"}[1m])) by (job)
```

-------

Group multiple log streams by job and unit

```
sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal"}[1m])) by (job,unit)
```

-------


### ** Comparison Operators **

Comparison Operators. Used for testing numeric values present in scalars and vectors.

* `==` : equal
* `!=` : not equal
* `>` : greater than
* `>=` : greater than or equal to)
* `<` : less than
* `<=` : less than or equal to

#### Examples

Returns values greater than 4

```
sum(count_over_time({job="systemd-journal"}[1m])) > 4
```

-------

Returns values less than or equal to 1

```
sum(count_over_time({job="systemd-journal"}[1m])) <= 1
```

-------

### ** Logical Operators **

Operatora can be used to add filter conditions:

* `and` : Both expressions must be true
* `or` : Either expression must be true

#### Examples

Returns only values smaller than 20% and smaller than 30%

```
{job="systemd-journal"} | json | mem > 20 and mem < 30
```

-------

Return only values below 30% or above 80%

```
{job="systemd-journal"} | json | mem < 30 or mem > 80
```

-------

<!-- tabs:end -->


?> Anything missing? qryn/LogQL can be extended using [custom plugins](https://github.com/metrico/qryn/wiki/LogQL-Plugins)


