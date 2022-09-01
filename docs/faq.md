# 🗨️ _Frequently Asked Questions_

_Got any questions or ideas? Join our [matrix room](https://matrix.to/#/#qryn:matrix.org) and meet the team_

## General

?> What does _qryn_ mean? Why did you choose it?

It means _querying_ and we choose it overnight because Grafana literally owns the word `Loki` _*cough*_

----

?> Wait, is _qryn_ using the Grafana stack? Does it use Loki, Mimir?

Nope. We only use Grafana _(the user-interface)_ while _all_ other components are provided by _qryn and ClickHouse

----

?> Loki, Prometheus and Tempo already exists, why do we need _qryn_?

Why not? Alternatives are good and can help accelerate evolution in an lively ecosystem. **qryn** is just designed to make its users happy and was implemented as a transparent and lightweight _overlay_ on top ClickHouse, where all data ingested across protocols sits ready to be accessed, analyzed and correlated in thousands of way today and into the future leveraging its fast and ever growing capabilities.

---

## Technology

?> What is the underlying design?

**qryn** implements a complete LogQL API buffered by a fast bulking **LRU** sitting on top of **ClickHouse** tables and relying on its *columnar search and insert performance alongside solid distribution and clustering capabilities* for stored data. qryn does not parse or index incoming logs, but rather groups log streams using the same label system as Prometheus and supports LogQL, PromQL and Tempo APIs for querying data back.

On top of this dataset, multiple ingestion APIs and query languages are implemented, such as _PromQL, Tempo, Influx, Elastic and others._

<p align="center">
  <img src="https://user-images.githubusercontent.com/1423657/54091852-5ce91000-4385-11e9-849d-998c1e5d3243.png" />
</p>

---


?> What exactly does _qryn_ support?

**qryn** is an API polyglot and supports multiple standards at once, out of the box.

#### Logs
Logs store event data that is usually text-based; they can describe an error or a success, or identify systems and processes, and correlate to other events. 
#### Metrics
Metrics are aggregatable and can be combined or divided to derive higher-level statistics. You can use metrics to monitor performance, request rates and errors. 
#### Traces
Traces reveal data that is relevant to a particular request, such as the duration of a process, an SQL query or the correlation ID of an inbound HTTP request. 



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

<div style="font-size: 13px;">
- qryn is not affiliated or endorsed by Grafana Labs or ClickHouse Inc. All rights belong to their respective owners.<br>
- qryn is a 100% clear-room api implementation and does not fork, use or derivate from Grafana Loki code or concepts.<br>
- Grafana®, Loki™ and Tempo® are a Trademark of Raintank, Grafana Labs. <br>
- ClickHouse® is a trademark of ClickHouse Inc. <br>
- Prometheus is a trademark of The Linux Foundation.<br>
</div>
