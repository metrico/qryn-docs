## Performance ⚙️

!> Benchmarks are a work in progress! ⏲️

**qryn** is regularly tested using the Victoriametrics Prometheus benchmark and various LogQL benchmarks

### Writer Performance
ClickHouse `INSERT` performance exceeding **~500k samples/thread/second** 

> _Resources: 4 CPUs, 3GB RAM, SSD Storage (2 cores for clickhouse, 1 core for qryn)_

![image](https://user-images.githubusercontent.com/1423657/187044328-b300b810-0e1b-46e3-8878-067d0a9fb6f7.png)

![image](https://user-images.githubusercontent.com/1423657/187044346-cd3a6d66-0f05-4993-a6ba-b44727bbdc81.png)
