# 🗨️ _Frequently Asked Questions_

_Got any questions or ideas? Join our [matrix room](https://matrix.to/#/#qryn:matrix.org) and meet the team_

## General

?> What does _qryn_ mean? Why did you choose it?

It means _querying_ and we choose it overnight because Grafana literally owns the word `Loki` _*cough*_

----

?> Wait, is _qryn_ using the Grafana stack? Does it use Loki, Mimir?

Not at all - _it replaces them_. We simply allow using Grafana _(the user-interface, unmodified)_ and its native datasources, while _all_ other components and APIs are entirely provided by _qryn and ClickHouse_. qryn is a clear room implementation and it is **not** a fork and does not contain _any code_ from Grafana or its projects.

----

?> Loki, Mimir/Prometheus/Victoriametrics/etc already exists, why do we need _qryn_?

Why not? We're loving the challenge. Alternatives play an important role and can accelerate evolution in an lively ecosystem. **qryn** is just designed to make its end users happy and was implemented as a transparent, smart, lightweight _overlay_ on top ClickHouse, where all data ingested across protocols sits ready to be accessed, analyzed and correlated in thousands of way today and into the future leveraging its fast and ever growing capabilities.

---

## Technology

?> What is the underlying design?

**qryn** implements a complete LogQL API buffered by a fast bulking **LRU** sitting on top of **ClickHouse** tables and relying on its *columnar search and insert performance alongside solid distribution and clustering capabilities* for stored data. qryn does not parse or index incoming logs, but rather groups log streams using the same label system as Prometheus and supports LogQL, PromQL and Tempo APIs for querying data back.

On top of this dataset, multiple ingestion APIs and query languages are implemented, such as _PromQL, Tempo, Influx, Elastic and others._

<p align="center">
  <img src="https://user-images.githubusercontent.com/1423657/54091852-5ce91000-4385-11e9-849d-998c1e5d3243.png" />
</p>

---

### Fingerprints

?> _How are Series Fingerprints counted in qryn?_ TLDR; _Like Prometheus._

Fingerprint usage grows when _many different combinations of labels_ are used by logs and metrics.

This is called **cardinality**. Let's use some example inserts and analyze their cardinality:

##### Example 1
```
cpu_seconds{host="host1",cpu="0",type="user"} 1
cpu_seconds{host="host1",cpu="0",type="user"} 2
cpu_seconds{host="host1",cpu="0",type="user"} 3
cpu_seconds{host="host1",cpu="0",type="user"} 2
```

>  The label value is constant for all time series or logs.

This insert would use **one fingerprint**. 

##### Example 2
```
cpu_seconds{host="host1",cpu="0",type="user"} 1
cpu_seconds{host="host1",cpu="1",type="user"} 1
cpu_seconds{host="host1",cpu="0",type="user"} 2
cpu_seconds{host="host1",cpu="1",type="user"} 1
```

>  The label value of **cpu** is different for each time series even though *host* and *mode* are the same.

The insert would use **two fingerprints**.

##### Example 3

Labels using *6* different types, *10* different hosts and *4* CPUs they would use **240 fingerprints** _(6*10*4)_

##### Example 4

**Logs and Metrics share fingerprints**<br>
Using the same labels for both causes no additional load in the fingerprint calculation.

---

?> What formats does _qryn_ support?

**qryn** is an API polyglot and [supports multiple APIs and protocols](https://qryn.metrico.in/#/support) at once, out of the box.

#### Logs
> Logs store event data that is usually text-based; they can describe an error or a success, or identify systems and processes, and correlate to other events. 
#### Metrics
> Metrics are aggregatable and can be combined or divided to derive higher-level statistics. You can use metrics to monitor performance, request rates and errors. 
#### Traces
> Traces reveal data that is relevant to a particular request, such as the duration of a process, an SQL query or the correlation ID of an inbound HTTP request. 

<br>

![image](https://user-images.githubusercontent.com/1423657/187046009-bc3d46d2-a5bd-400c-84e2-f6968a5a6bac.png)


| API        | Ingest           | Query    | Notes  |
|---         |---               |---       |---     |
| Loki       | 🟢 logs, metrics | 🟢 logql  | full support |
| Prometheus | 🟢 metrics       | 🟢 promql | remote_write ingestion, logql+promql querying |
| Elastic    | 🟢 logs          | 👽 logql  | _index/bulk_ ingestion, logql querying |
| Influx     | 🟢 logs, metrics | 👽 logql  | line protocol ingestion  |
| OTLP/Zipkin| 🟢 traces, spans | 🟢 tempo  | zipking ingestion, tempo querying |

---

?> NodeJS is slow and _(insert argument)_ so _qryn_ must be slow and not serious.

False. NodeJS can be very fast when used right as an API and in our stack, ClickHouse does all the heavy-lifting.<br>
Since qryn is stateless it can easily be auto-scaled on top of CH at incredibly low cost and complexity.

This said - there are limits! This is why our stack has a commercial high-performance version [for cloud integrators](mailto:info@qxip.net)

----

?> Do I need to be a ClickHouse master to use it?

No, but you'll become one over time if you do.

---

?> Some of your queries are not as good as they _could be_

Join us make this better! Please open a [PR](https://github.com/metrico/qryn) or start a discussion with your proposal.

---

## Troubleshooting

?> JavaScript heap out of memory

The default Node heap size is **too small** for the workload. Increase it accordingly, ie: 

```
 export NODE_OPTIONS="--max-old-space-size=4096" # Increases node heap to 4 GB
 ```

---

?> npm ERR! File exists: /usr/bin/cloki

Welcome back, old user! Please remove any previous cloki versions before installing the qryn package
```
npm remove -g cloki
npm install -g qryn
```


---

## Disclaimers

?> ©️ QXIP BV, released under the GNU Affero General Public License v3.0. See LICENSE for details.

?> Proprietary _(Non-GPL)_ License available for cloud technocalogy integrators. Please [contact us](mailto://info@qryn.dev) for details.

<div style="font-size: 13px;">
- qryn is not affiliated or endorsed by Grafana Labs or ClickHouse Inc. All rights belong to their respective owners.<br>
- qryn is a 100% clear-room api implementation and does not fork, use or derivate from Grafana Loki code or concepts.<br>
- Grafana®, Loki™ and Tempo® are a Trademark of Raintank, Grafana Labs. <br>
- ClickHouse® is a trademark of ClickHouse Inc. <br>
- Prometheus is a trademark of The Linux Foundation.<br>
</div>
