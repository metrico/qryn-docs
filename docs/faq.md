# _Frequently Asked Questions_

## General

?> What does _qryn_ mean? Why did you choose it?

It means _querying_ and we choose it overnight because Grafana literally owns the word `Loki` _*cough*_

----

?> Loki, Prometheus and Tempo already exists, why do we need _qryn_?

Why not? Our system uses a different design where ultimately all data across ingested protocols is stored in ClickHouse, ready to be accessed, analyzed and correlated in thousands of way today and into the future.

---

## Technology

?> What exactly does _qryn_ support?

**qryn** is an API polyglot and supports multiple standards at once, out of the box.

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
