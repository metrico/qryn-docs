<!-- ![image](https://user-images.githubusercontent.com/1423657/197589119-bb790fba-dd50-412c-92c1-033675fa980a.png ':size=200') -->
<a href="https://qryn.cloud" target="_blank">
  <!-- <img src="https://user-images.githubusercontent.com/1423657/200078144-5d0b0960-2ad8-4b0c-9cdd-b7f8f9f516ae.png" width=500 /> -->
  <img src="https://user-images.githubusercontent.com/1423657/219213740-b047fed9-4c71-4446-9341-452ccdcbabdc.png" width=600 />  
</a>

# ☁️ [qryn:cloud](/cloud)
!> qryn:cloud requires activation & pull tokens! Please [contact us](mailto:info@qxip.net) to obtain a license.

### Distributed Mode
If you have too much of the data to fit into one clickhouse you may want to use clickhouse cluster to 
share the data between two or more instances. qryn:cloud distributed mode is designed for this purpose.  

qryn:cloud distributed mode is provided via an extra layer of `_dist` suffixed tables. 
The tables use the `Distributed` engine type so writes and reads are provided for the whole cluster.

The distribution/sharding is `fingerprint` based to consist session to the same cluster node.

#### How to create a minimal clickhouse cluster with docker-compose

1. Create a compose yaml file with two clickhouse servers:

```yaml
version: '3.8'
networks:
  common:
    driver: bridge

services:
  clickhouse:
    image: altinity/clickhouse-server:23.8.11.29.altinitystable
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
    image: altinity/clickhouse-server:23.8.11.29.altinitystable
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
docker run -it clickhouse/clickhouse-server cat /etc/clickhouse-server/config.xml >config.xml
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

### Multi Tenancy support

When one datasource is reused by a number of clients we may appear in a situation where we have to reduce the information each client can see. Qryn cloud project has `organisation id` mechanism to provide such feature.

#### Versions supported

- _qryn-go v1.2.39 or higher_
- _wryn-writer v.1.9.61 or higher_


#### Low level implementation.
Each table created by Qryn has the `org_id String` column. This column is included into the primary key so the database client can easily filter information according to its value.

For example there's `samples_v4` table schema:
```sql
CREATE TABLE cloki_nl.samples_v4
(
    `org_id` String,
    `fingerprint` UInt64,
    `timestamp_ns` Int64 CODEC(DoubleDelta),
    `value` Float64 CODEC(Gorilla),
    `string` String
)
ENGINE = MergeTree
PARTITION BY (org_id, toStartOfDay(toDateTime(timestamp_ns / 1000000000)))
ORDER BY (org_id, timestamp_ns)
TTL toDateTime(timestamp_ns / 1000000000) + toIntervalDay(7)
```
The org_id column takes part in every write and read request in Qryn. By default (single-tenant mode of qryn) org-id is initialized as `'0'` so all the data is written to the same organization part.

#### Configuration of multitenancy
The multi-tenant mode is configured in the configuration file of QRYN reader and writer or via the environment variables.

Configuration json file has the bool `multitenance_settings.enabled` option which should be set to `true` in order to enable the multitenant mode:
```
{ 
  database_d....,
  ...
  "multitenance_settings": {
    "enabled": true
  },
  ...
}
```

The corresponding env variable is: `QRYN_MULTITENANCE_SETTINGS_ENABLED=true`.

### Specifying tenant-id for read & write operations
After the setting is set to true, all the read and write operations start failing because Qryn starts looking for:
- the `X-Scope-OrgID` header in http requests
- the `X-Scope-OrgID` get parameter in websocker requests. 

If there's no org-id specified, then the request fails. 

After the header or the get parameter is provided with the ID, Qryn starts filtering all the read operations with it and adding the corresponding value to every write operation. 

### Troubleshooting

The following commands provide a reference to monitor partition usage:

##### Count Fingerprints by OrgID
```
:) SELECT org_id, count(DISTINCT fingerprint) FROM time_series_v2 GROUP BY org_id;
```
##### Count Traces by OrgID
```
:) SELECT oid, count(1) FROM tempo_traces GROUP BY oid;
```
##### Count Logs/Metrics by OrgID
```
:) SELECT org_id, count(1) FROM samples_v4 GROUP BY org_id;
```
##### Uncompressed size of Logs/Metrics by OrgID
```
:) SELECT org_id, sum(length(string)) FROM samples_v4 GROUP BY org_id;
```

