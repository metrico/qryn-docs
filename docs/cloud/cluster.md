# qryn:cloud
## Distributed mode
qryn:cloud distributed mode is provided via an extra layer of `_dist` suffixed tables. 
The tables use the `Distributed` engine type so writes and reads are provided for the whole cluster.

The distribution/sharding is `fingerprint` based to consist session to the same cluster node.

## Configuration

The distributed mode is turned on by json file configuration or by an env var:
```
{
  "database_data": [
     {
       ...
      "cluster_name": "test_cluster_two_shards"
    }
  ...
}
```

The corresponding env var should be: `QRYN_DATABASE_DATA_0_CLUSTER_NAME=test_cluster_two_shards`
cluster name is the name of a cluster from the config.xml file of clickhouse server:
```
<remote_servers>
    <test_cluster_two_shards>
        <shard>
            <replica>
                <host>shard1.local</host>
                <port>9000</port>
            </replica>
        </shard>
        <shard>
            <replica>
                <host>shard2.local</host>
                <port>9000</port>
            </replica>
        </shard>

    </test_cluster_two_shards>
</remote_servers>
```

Before using Qryn the writer with `-initialize_db` flag should be started in order to create the `_dist` tables.
