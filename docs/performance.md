# 🚤 Performance

!> Benchmarks are a work in progress! Share your experience and performance with us.

**qryn** is regularly tested using the Victoriametrics Prometheus benchmark and various LogQL benchmarks

### Writer Performance ([qryn-js](https://github.com/metrico/qryn))

> Protocol: HTTP/S

ClickHouse `INSERT` performance reaches **~20k samples/thread/second** on _modest hardware_ using _qryn-js_

Use *pm2* or any other method to cluster `qryn-js` worker instances horizontally.

--------

### Writer Performance ([qryn-otel-collector](https://github.com/metrico/otel-collector))

> Protocol: BINARY

_T.B.D._ please share your performance reports!

--------


### Writer Performance ([qryn-cloud](https://qryn.cloud))

> Protocol: BINARY

ClickHouse `INSERT` performance exceeds **~500k samples/thread/second** on _modest hardware_ using _qryn-go_

> _Resources: 4 CPUs, 3GB RAM, SSD Storage (2 cores for clickhouse, 1 core for qryn-go)_

![image](https://user-images.githubusercontent.com/1423657/187044328-b300b810-0e1b-46e3-8878-067d0a9fb6f7.png)

![image](https://user-images.githubusercontent.com/1423657/187044346-cd3a6d66-0f05-4993-a6ba-b44727bbdc81.png)

> qryn:cloud is developed in Go/Rust and available for integrators and resellers under a commercial license

--------

