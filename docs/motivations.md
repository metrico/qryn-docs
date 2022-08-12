# Motivations

qryn implements a complete LogQL API buffered by a fast bulking LRU sitting on top of ClickHouse tables and relying on its columnar search and insert performance alongside solid distribution and clustering capabilities for stored data. qryn does not parse or index incoming logs, but rather groups log streams using the same label system as Prometheus. 2

