# 🔎 Querying

In this section, we'll learn how to query and filter our ingested data. Let's get __qryn__!

In order to **explore** our _logs and metrics_, we'll need to use a _client_ such as _Grafana_

<!-- tabs:start -->
### ** ⭐ Grafana **

Let's explore using [Grafana](guide/datasources.md) and its _amazing visualizations_

![grafanaexplore](https://user-images.githubusercontent.com/1423657/184538094-13c11500-24ef-4468-9f33-dc9d564238e3.gif)

### LogQL
#### Find a Label 🏷️

Let's use our **qryn** `Loki` datasource find logs with label _dummy-server_
```
{job="dummy-server"}
```

![ezgif com-gif-maker (12)](https://user-images.githubusercontent.com/1423657/184545583-58610ae2-6a80-456b-8326-9c30a72e44ab.gif)

#### Find a String 🔎

Let's filter our logs with a string match for `peach`
```
{job="dummy-server"} |= "peach"
```


![ezgif com-gif-maker (13)](https://user-images.githubusercontent.com/1423657/184545713-3f6e90ba-3c6d-4dc4-b897-d10373feb695.gif)

##### Calculate metrics 📊

Let's calculate the minute rate for our matching logs 
```
rate({job="dummy-server"} |= "peach" [1m])
```

![image](https://user-images.githubusercontent.com/1423657/184548882-37268c27-7a01-4787-8259-11327158c2d5.png)


### Tempo
#### Find a Trace using LogQL 🧲

Let's use the **qryn** `Tempo` datasource to find traces for `{job="dummy-server"}` using _LogQL or Beta Search_

##### Beta Search
![image](https://user-images.githubusercontent.com/1423657/186616553-ca34e26a-6a59-4a04-98e8-c27fdf8f6159.png)

##### Loki Search
![image](https://user-images.githubusercontent.com/1423657/184548814-b5a4c641-010e-47b1-91c8-5aa402a85908.png)



### PromQL
#### Visualize metrics using PromQL 📈

#### Find a Label 🏷️

Let's use our **qryn** `Loki` datasource find metrics for label `cpu_percent_value`

![image](https://user-images.githubusercontent.com/1423657/185901793-37529481-5201-474f-b01e-b30386a8ef89.png)

##### Visualize metrics 📊

Let's calculate the `rate` for our matching metrics 

![image](https://user-images.githubusercontent.com/1423657/185901314-b94082cf-b2ea-4fc2-93df-5d11e9fe055c.png)

### Go Pro 🥊

That was easy, _wasn't it?_

Progress your knowledge using the [advanced LogQL guide](guide/logql.md)

### ** 👁️ View **

Let's explore using [qryn-view](view.md) our embedded user interface - _no installation needed_

![view](https://user-images.githubusercontent.com/1423657/166163594-c51cc598-50a2-4136-8792-91b45024c8d9.gif)

#### Connect 🔌
Access your local _view_ instance or use the [public view](https://view.cloki.org) to access your **qryn** API 

#### Find a Label 🏷️

Let's find logs with label 
```
{type="syslog"}
```

![ezgif com-gif-maker (14)](https://user-images.githubusercontent.com/1423657/184545892-d0649b5c-8e57-444c-a40f-417dfb2199ea.gif)

#### Find a String 🔎

Let's filter our logs to check if we need a _bigger boat_ 
```
{type="syslog"} |= "boat"
```

![image](https://user-images.githubusercontent.com/1423657/184545906-359e90bc-ba37-4490-ad02-d3717a4eebd1.png)

##### Calculate metrics 📊

Let's calculate the minute rate for our matching logs 
```
rate({type="syslog"} |= "boat" [1m])
```

![image](https://user-images.githubusercontent.com/1423657/166163222-9878936e-5114-4ece-883f-f721f5fba8ee.gif)

### Go Pro 🥊

That was easy, _wasn't it?_

Progress your knowledge using the [advanced LogQL guide](guide/logql.md)


### ** 🗄️ vLogQL **

Let's explore using [vLogQL](https://github.com/lmangani/vlogql) our compact _command line_ LogQL client and canary agent

?> vLogQL is made in [v](https://vlang.io)

#### 📦 Download Binary
Download and deploy the `vlogql` binary anywhere on your system
```
curl -fsSL github.com/lmangani/vLogQL/releases/latest/download/vlogql -O && chmod +x vlogql
```

#### 🔎 Usage
```
Usage: vlogql [options] [ARGS]

Options:
  -l, --limit <int>         logql query limit [LOGQL_LIMIT]
  -a, --api <string>        logql api [LOGQL_API]
  -q, --query <string>      logql query [LOGQL_QUERY]
  -t, --labels              get labels
  -v, --label <string>      get label values
  -s, --start <string>      start nanosec timestamp
  -e, --end <string>        end nanosec timestamp
  -x, --tail                tail mode
  -c, --canary              canary mode
  -h, --help                display this help and exit
```

#### ⭐ Examples 

##### Query w/o Labels

Let's find `5` logs with label `{type="syslog"}` with a regex match for `MiB`

```bash
# LOGQL_API="https://qryn:3100" ./vlogql --query '{type="clickhouse"} |~ "MiB"' --limit 5

---------- Logs for: {type="clickhouse"} |~ "MiB"
2022.03.13 10:39:19.765860 [ 29849 ] {} <Debug> MemoryTracker: Peak memory usage (for query): 8.11 MiB.
2022.03.13 10:39:19.761259 [ 29849 ] {115c1357-81d3-4277-ab04-882306f76e9d} <Debug> MemoryTracker: Peak memory usage (for query): 4.12 MiB.
2022.03.13 10:39:19.761288 [ 29849 ] {} <Debug> MemoryTracker: Peak memory usage (for query): 4.12 MiB.
2022.03.13 10:39:19.765798 [ 29849 ] {f050f9e5-f919-4680-b826-ea84be9542e0} <Debug> MemoryTracker: Peak memory usage (for query): 8.11 MiB.
2022.03.13 10:39:19.759982 [ 29849 ] {115c1357-81d3-4277-ab04-882306f76e9d} <Debug> DiskLocal: Reserving 1.00 MiB on disk `default`, having unreserved 2.63 TiB.
```

##### Query w/ Labels

Let's find `5` logs with label `{type="syslog"}` with a regex match for `MiB` and print `labels` 

```bash
# LOGQL_API="https://qryn:3100" ./vlogql --query '{type="clickhouse"} |~ "MiB"' --limit 4 --labels

---------- Logs for: {type="clickhouse"} |~ "MiB"
Log Labels: {'pid': '19639', 'level': 'Debug', 'call': 'MemoryTracker', 'type': 'clickhouse'}
2022.03.14 09:53:31.017408 [ 19639 ] {} <Debug> MemoryTracker: Peak memory usage (for query): 4.14 MiB.
2022.03.14 09:53:31.021778 [ 19639 ] {} <Debug> MemoryTracker: Peak memory usage (for query): 8.18 MiB.
2022.03.14 09:53:31.021759 [ 19639 ] {785ba1fa-3be3-4023-8a95-de4b92c096a4} <Debug> MemoryTracker: Peak memory usage (for query): 8.18 MiB.
2022.03.14 09:53:31.017389 [ 19639 ] {a34cbcc9-d11a-4a0a-8c7a-634e00322900} <Debug> MemoryTracker: Peak memory usage (for query): 4.14 MiB.
```
##### Query Labels

Let's query for available stream `labels`


```bash
# LOGQL_API="https://qryn:3100" ./vlogql --labels

---------- Labels:
['response', 'host', 'type']
```
##### Query Label Values

Let's query for available stream label `values`


```bash
# LOGQL_API="https://qryn:3100" ./vlogql --label type

---------- Values for: type
['clickhouse', 'prometheus']
```

##### Tail Logs by Tag _(websocket)_
```bash
# LOGQL_API="https://qryn:3100" ./vlogql --query '{type="clickhouse"}' --tail

---------- Logs Tail
Log Labels: {'pid': '1658', 'level': 'Debug', 'type': 'clickhouse'}
2022.03.18 16:54:47.634586 [ 1658 ] {} <Debug> system.query_views_log (2bbc858b-05df-49d1-abbc-858b05df69d1): Removing part from filesystem 202203_405891_405891_0
2022.03.18 16:54:51.704528 [ 1658 ] {} <Debug> cloki.time_series (bfb2e93e-f78d-4692-bfb2-e93ef78d8692): Removing part from filesystem 20220318_22425079_22559905_26963
2022.03.18 16:54:51.704730 [ 1658 ] {} <Debug> cloki.time_series (bfb2e93e-f78d-4692-bfb2-e93ef78d8692): Removing part from filesystem 20220318_22559906_22559906_0
2022.03.18 16:54:51.704910 [ 1658 ] {} <Debug> cloki.time_series (bfb2e93e-f78d-4692-bfb2-e93ef78d8692): Removing part from filesystem 20220318_22559907_22559907_0
2022.03.18 16:54:51.705140 [ 1658 ] {} <Debug> cloki.time_series (bfb2e93e-f78d-4692-bfb2-e93ef78d8692): Removing part from filesystem 20220318_22559908_22559908_0
```

##### Canary Logs _(push + websocket)_
```bash
# LOGQL_API="https://qryn:3100" CANARY_TIMER=10 ./vlogql --canary --labels

---------- Tail Canary Logs
PUSH Successful: {"streams":[{"stream": {"canary":"canary_9cwAkBFDlrcA","type":"canary"}, "values":[ ["1649007076406000000", "ts=1649007076406000000 count=3 type=canary tag=canary_9cwAkBFDlrcA"] ]}]}
Sleeping 10 seconds...
Log Labels: {'canary': 'canary_9cwAkBFDlrcA', 'type': 'canary'}
ts=1649007076406000000 count=3 type=canary tag=canary_9cwAkBFDlrcA
PUSH Successful: {"streams":[{"stream": {"canary":"canary_9cwAkBFDlrcA","type":"canary"}, "values":[ ["1649007086482000000", "ts=1649007086482000000 count=4 type=canary tag=canary_9cwAkBFDlrcA"] ]}]}
Sleeping 10 seconds...
Log Labels: {'canary': 'canary_9cwAkBFDlrcA', 'type': 'canary'}
ts=1649007086482000000 count=4 type=canary tag=canary_9cwAkBFDlrcA
```

<!-- tabs:end -->
