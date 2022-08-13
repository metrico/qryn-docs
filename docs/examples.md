# Examples

This section illustrates a number of example LogQL queries to get you started.

### LogQL Bootcamp

The main purpose of the Log Query Language is to allow a user to easily reach the data, and only the data that matters to them.

**qryn** allows many forms of data such as logs, values, strings etc, to be streamed into the Clickhouse database. Each type of stream is inserted with **labels** _(or tags)_ used to quickly filter the exact data stream we want to have returned to us to analyze and visualize data through Grafana or any other LogQL client.

There are two types of LogQL queries:

  * [Log queries](https://grafana.com/docs/loki/latest/logql/log_queries/) returning the contents of log lines as streams.
  * [Metric queries](https://grafana.com/docs/loki/latest/logql/metric_queries/) that convert logs into value matrices.

A LogQL query consists of:

  * A Log Stream selector
  * Filter expression

We can use operations on both the log stream selectors and filter expressions to refine them.

<!-- tabs:start -->

### ** Log Stream Selectors Operators **

* ```= : equals```
* ```!= : not equals```
* ```=~ : regex matches```
* ```!~ : regex does not match```

#### Examples

Return all log lines for the job systemd-journal

```{job="systemd-journal"}```

-------

Return all log lines for the unit ssh.service

```{unit="ssh.service"}```

-------

Return all log lines for the job systemd-journal and the unit cron.service

```{job="systemd-journal",unit="cron.service"}```

-------

Show all log lines for 2 jobs with different names

```{job=~"qryn/systemd-journal|systemd-journal"}```

-------

### ** Filter Expressions Operators **

Used for testing text within log line streams.

* ```|= : equals```
* ```!= : not equals```
* ```|~ : regex matches```
* ```!~ : regex does not match```

#### Examples

Return lines including the text "error"

```{job="systemd-journal"} |= "error"```

-------

Return lines not including the text "error"

```{job="systemd-journal"} != "error"```

-------

Return lines including the text "error" or "info" using regex

```{job="systemd-journal"} |~ "error|info"```

-------

Return lines not including the text "error" or "info" using regex

```{job="systemd-journal"} !~ "error|info"```

-------

Return lines including the text "error" but not including "info"

```{job="systemd-journal"} |= "error" != "info"```

-------

Return lines including the text "Invalid user" and including ("default" or "qryn") using regex

```{job="systemd-journal"} |~ "Invalid user (default|qryn)"```

-------

Return lines including the text "status 403" or "status 503" using regex

```{job="systemd-journal"} |~ "status [45]03"```

-------


### ** Range and Instance Vectors **

The data so far is returned as streams of log lines. We can graph these in visualizations if we convert them to vectors. We can aggregate the lines into numeric values, such as counts, which then become known as instance or range vectors.

* ```count_over_time : Shows the total count of log lines for time range```
* ```rate : Similar as count_over_time but converted to number of entries per second```
* ```bytes_over_time : Number of bytes in each log stream in the range```
* ```bytes_rate : Similar to bytes_over_time but converted to number of bytes per second```

#### Examples

The count of jobs at 1 minutes time intervals

```count_over_time({job="systemd-journal"}[1m])```

-------

The rate of logs per minute. Rate is similar to count_over_time but shows the entries per second.

```rate({job="systemd-journal"}[1m])```

-------

##### Info

```
rate = count_over_time / 60 / range(m)

eg,

12 / 60 / 2 = 0.1
```

The count of errors at 1h time intervals

```count_over_time({job="systemd-journal"} |= "error" [1h])```


### ** Aggregate Functions **

An aggregate function converts a range vector result into a single instance vector.

* ```sum : Calculate the total of all instance vectors in the range at time```
* ```min : Show the minimum value from all instance vectors in the range at time```
* ```max : Show the maximum value from all instance vectors in the range at time```
* ```avg : Calculate the average of the values from all instance vectors in the range at time```
* ```stddev : Calculate the standard deviation of the values from all instance vectors in the range at time```
* ```stdvar : Calculate the standard variance of the values from all instance vectors in the range at time```
* ```count : Count the number of elements all all instance vectors in the range at time```

#### Examples

Calculate the total of all instance vectors in the range at time

```sum(count_over_time({job="systemd-journal"}[1m]))```

-------

Show the minimum value from all instance vectors in the range at time

```min(count_over_time({job="systemd-journal"}[1m]))```

-------

Show the maximum value from all instance vectors in the range at time

```max(count_over_time({job="systemd-journal"}[1m]))```

-------

### ** Aggregate Group **

Convert an instance vector into a range vector organized by unit

#### Examples

Group a single log stream by unit

```sum(count_over_time({job="systemd-journal"}[1m])) by (unit)```

-------

Group multiple log streams by job

```sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal"}[1m])) by (job)```

-------

Group multiple log streams and specific unit by job

```sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal", unit="cron.service"}[1m])) by (job)```

-------

Group multiple log streams by job and unit

```sum(count_over_time({job=~"qryn/systemd-journal|systemd-journal"}[1m])) by (job,unit)```

-------


### ** Comparison Operators **

Comparison Operators. Used for testing numeric values present in scalars and vectors.

* ```== (equality)```
* ```!= (inequality)```
* ```> (greater than)```
* ```>= (greater than or equal to)```
* ```< (less than)```
* ```<= (less than or equal to)```

#### Examples

Returns values greater than 4

```sum(count_over_time({job="systemd-journal"}[1m])) > 4```

-------

Returns values less than or equal to 1

```sum(count_over_time({job="systemd-journal"}[1m])) <= 1```

-------

### ** Logical Operators **

These can be used to add filter conditions

* ```and : Both sides must be true```
* ```or : One on either side must be true```

#### Examples

Returns only values smaller than 20% and smaller than 30%

```{job="systemd-journal"} | json | mem > 20 and mem < 30```

-------

Return only values below 30% or above 80%

```{job="systemd-journal"} | json | mem < 30 or mem > 80```

-------

## ** Custom Functions **

Anything missing? qryn/LogQL can be extended using [custom plugins](https://github.com/metrico/qryn/wiki/LogQL-Plugins)

<!-- tabs:end -->

