# 📖 LogQL

**LogQL** is a _PromQL-inspired_ query language acting like a distributed grep with powerful _filters and aggregations_.

?> _Relax. It's all quite simple once you try it!_

There are just two types of _LogQL_ queries:

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

We can use operations on both the log **stream selectors** and **filter expressions** to refine them.

## ** Parser  **

<!-- tabs:start -->

### ** Parser Expression **
Parser expressions can parse and extract labels from the log content. Those extracted labels can then be used for filtering using label filter expressions or for metric aggregations without bloating cardinality.

<!-- tabs:start -->

### ** json **
The json parser operates in two modes:

* without parameters:
  * Adding `| json` to your pipeline will extract all json properties as labels if the log line is a valid json document. Nested properties are flattened into label keys using the _ separator.
  * ```{job="0.6611336793589486_json"} | json```

* with parameters:
  * Using `| json label="expression"` in your pipeline will extract only the specified json fields to labels. 
  * ```{job="0.6611336793589486_json"} | json my_field="json_field"```

### ** logfmt **
The logfmt parser extracts any `key=value` pairs from the processed logs.

Example:

##### Input
* ```YYYY-MM-DDT00:00:00Z ... somelog name=qryn value=123 something=else```

##### LogQL
* ```{type="clickhouse"} |~"somelog" | logfmt```

##### Extracted Labels
* ```{ name: "qryn", value: 123, something: "else" }```


### ** regexp **
The regexp parser operates against log string and requires named groups for matching.

Example: extract a new label named `token` from a string ie: 
```YYYY-MM-DDT00:00:00Z ... Reserving 1.1Mb of memory```

 * ```{type="clickhouse"} |~"Reserving" | regexp "Reserving (?<token>\\d+.\\d+)"```

<!-- tabs:end -->


#### ** Line Format **
Line Format expression allows the re-formatting of parts of a log line. It can extract items from parsed json.

To extract a *parameter* we use "{{}}" to surround it and extract it as the displayed log line as a result. Any thing can be added to this line format, to allow for additional notes or formatting.

```
{job="0.6611336793589486_json"} | json | line_format "My field : {{my_field}}"
```

Creates new log lines of the string "My field: VALUE"

#### ** Label Filter Expression **
Label filter expression allows filtering log line using their original and extracted labels. It can contain multiple predicates.

A predicate contains a *label identifier*, an *operation* and a *value* to compare the label with.

Label filters work like label matchers and use the same operations (`=`,`!=`,`=~`,`!~`).

```
{job="0.6611336793589486_json"} | json | my_field="VALUE"
```
<!-- tabs:end -->

<!-- end of subtab -->

<!-- tabs:end -->

<!-- tabs:start -->

### ** Range Vectors **

Just like Prometheus, LogQL supports aggregating data over a selected ranges to transform it into an instance vectors.

The currently supported `range vector` functions are:

* `rate` : Calculates the number of entries per second
* `count_over_time` : Counts the entries for each log stream within the given range.
* `bytes_over_time` : Counts the number of bytes in each log stream in the range
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

Count of errors at 1h time intervals

```
count_over_time({job="systemd-journal"} |= "error" [1h])
```


### ** Unwrap **

Unwrap Expressions allow for the unwrapping of a value to be used in an aggregation. It can extract any parsed json value.

Supported `unwrap range` functions for operating over unwrapped ranges are:

* `rate(unwrapped-range)`: calculates per second rate of all values in the specified interval
* `sum_over_time(unwrapped-range)`: the sum of all values in the specified interval
* `avg_over_time(unwrapped-range)`: the average value of all points in the specified interval
* `max_over_time(unwrapped-range)`: the maximum value of all points in the specified interval
* `min_over_time(unwrapped-range)`: the minimum value of all points in the specified interval

#### Examples

Extract a value from a json log and select a field to unwrap into metrics for visualization:
```
avg_over_time({job="0.6611336793589486_json"} | json myField="my_field" | unwrap myField [5s])"
```

The function extracts the value of `my_field` and aggregates it into an average of `5s` buckets over the given timerange.


### ** Aggregate **

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


### ** Comparison Ops **

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

### ** Logical Ops **

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

