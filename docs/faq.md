# _Frequently Asked Questions_

## General

?> What does _qryn_ mean? Why did you choose it?

It means _querying_ and we choose it overnight because Grafana literally owns the word `Loki` _*cough*_

----

?> Loki, Prometheus and Tempo already exists, why do we need _qryn_?

Why not? Our system uses a different design where ultimately all data across ingested protocols is stored in ClickHouse, ready to be accessed, analyzed and correlated in thousands of way today and into the future.

---

## Technology

?> NodeJS is slow and _(insert argument)_ so _qryn_ must be slow and not serious.

False. NodeJS can be very fast when used right as an API and in our stack, ClickHouse does all the heavy-lifting.<br>
This said, there are limits so we also develop a golang/rust high-performance version of the stack [licensed to cloud integrators](mailto:info@qxip.net)

----

?> Do I need to be a ClickHouse master to use it?

No, but you'll become one over time if you do.

---
