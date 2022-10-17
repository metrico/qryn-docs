## ☁️ [qryn:cloud](/#/cloud)

## Distributed mode
If you have too much of the data to fit into one clickhouse you may want to use clickhouse cluster to 
share the data between two or more instances. qryn:cloud distributed mode is designed for this purpose.  

qryn:cloud distributed mode is provided via an extra layer of `_dist` suffixed tables. 
The tables use the `Distributed` engine type so writes and reads are provided for the whole cluster.

The distribution/sharding is `fingerprint` based to consist session to the same cluster node.

#### How to create a minimal clickhouse cluster with docker-compose

1. Create a compose yaml file with two clickhouse servers:

```yaml
version: '2.1'
networks:
  common:
    driver: bridge

services:
  clickhouse:
    image: clickhouse/clickhouse-server:22.6.4.35
    container_name: clickhouse.min.deploy
    hostname: clickhouse.min.deploy
    domainname: clickhouse.min.deploy
    networks:
      - common
    expose:
      - "9000"
    volumes:
      - ./config.xml:/etc/clickhouse-server/config.xml
      - ./_clickhouse-data:/var/lib/clickhouse
  clickhouse2:
    image: clickhouse/clickhouse-server:22.6.4.35
    container_name: clickhouse2.min.deploy
    hostname: clickhouse2.min.deploy
    domainname: clickhouse2.min.deploy
    networks:
      - common
    expose:
      - "9000"
    volumes:
      - ./config.xml:/etc/clickhouse-server/config.xml
      - ./_clickhouse-data2:/var/lib/clickhouse
```

2. Export the default config.xml file from clickhouse server docker image:
```
docker run -it clickhouse/clickhouse-server:22.6.4.35 cat /etc/clickhouse-server/config.xml >config.xml
```

3. `vi config.xml` or use your preferred text editor

4. Alter the config.xml file to configure the cluster 
   - find the `remote_servers` tag
   - inside the tag create your own cluster configuration like the next one:
```xml
    <test_cluster_min>
        <shard>
            <replica>
            <host>clickhouse.min.deploy</host>
            <port>9000</port>
            </replica>
        </shard>
        <shard>
            <replica>
                <host>clickhouse2.min.deploy</host>
                <port>9000</port>
            </replica>
        </shard>
    </test_cluster_min>
```
your cluster tag should be inside `remote_servers` (between `<remote_servers>` and `</remote_servers>`)
`test_cluster_min` in this example is the name of the cluster you create. Feel free to choose or alter it.
`shard` - is a set of clickhouse servers where distributed table will send parts of rows.
`replica` - is a clickhouse server where to copy the shard of data. all replicas inside one shard will eventually 
store the copy of the data.


5. `docker-compose up` - run the cluster in the environment. You have a cluster setup.

#### Configuration of qryn reader and writer
After we created a cluster, we need to configure qryn to use it.
The distributed mode is turned on by json file configuration or by an env var.
The json configuration for the example is the next:
```json
{
  "database_data": [
    {
      "help": "Settings for Clickhouse Database (data)",
      "user": "default",
      "node": "clickhouse",
      "pass": "",
      "name": "qryn",
      "host": "clickhouse.min.deploy",
      "port": 9000,
      "table_prefix": "",
      "read_timeout": 30,
      "write_timeout": 30,
      "strategy": "failover",
      "primary": true,
      "debug": true,
      "secure": false,
      "cluster_name": "test_cluster_min"
    },
    {
        "help": "Settings for Clickhouse Database (data)",
        "user": "default",
        "node": "clickhouse",
        "pass": "",
        "name": "qryn",
        "host": "clickhouse2.min.deploy",
        "port": 9000,
        "table_prefix": "",
        "read_timeout": 30,
        "write_timeout": 30,
        "strategy": "failover",
        "primary": true,
        "debug": true,
        "secure": false,
        "cluster_name": "test_cluster_min"
    }
  ]
}
```
Please pay attention to the `cluster_name` configuration. It is the name of the cluster you put in the config.xml.
The corresponding env vars should be: 
```
QRYN_DATABASE_DATA_0_NAME=qryn
QRYN_DATABASE_DATA_0_HOST=clickhouse.min.deploy
QRYN_DATABASE_DATA_0_PORT=9000
QRYN_DATABASE_DATA_0_CLUSTER_NAME=qryn
QRYN_DATABASE_DATA_0_DEBUG=true
QRYN_DATABASE_DATA_0_USER=default
QRYN_DATABASE_DATA_0_PASS=
QRYN_DATABASE_DATA_0_CLUSTER_NAME=test_cluster_min
QRYN_DATABASE_DATA_1_NAME=qryn
QRYN_DATABASE_DATA_1_HOST=clickhouse2.min.deploy
QRYN_DATABASE_DATA_1_PORT=9000
QRYN_DATABASE_DATA_1_CLUSTER_NAME=qryn
QRYN_DATABASE_DATA_1_DEBUG=true
QRYN_DATABASE_DATA_1_USER=default
QRYN_DATABASE_DATA_1_PASS=
QRYN_DATABASE_DATA_1_CLUSTER_NAME=test_cluster_min
```
Please pay attention to the `QRYN_DATABASE_DATA_0_CLUSTER_NAME` and `QRYN_DATABASE_DATA_1_CLUSTER_NAME` configurations. 
It is the name of the cluster you put in the config.xml.

?> Before using qryn:writer `-initialize_db` should be executed to create the `_dist` tables
