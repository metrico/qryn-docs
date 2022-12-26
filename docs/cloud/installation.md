<!-- ![image](https://user-images.githubusercontent.com/1423657/197589119-bb790fba-dd50-412c-92c1-033675fa980a.png ':size=200') -->
<a href="https://app.gigapipe.com/signup?ref=qxip" target="_blank">
  <!-- <img src="https://user-images.githubusercontent.com/1423657/200078144-5d0b0960-2ad8-4b0c-9cdd-b7f8f9f516ae.png" width=500 /> -->
  <img src="https://user-images.githubusercontent.com/1423657/200078554-f8352174-9a6b-4f4a-90fc-1c6521d46c5b.png" width=600 />  
</a>

# ☁️  [qryn:cloud](/cloud)

!> qryn:cloud requires activation & pull tokens! Please [contact us](mailto:info@qxip.net) to obtain a license.

?> ClickHouse Server LTS version >=22 and <=22.9 is suggested. _22.10+ not yet supported._

## 📦 Installation

Let's install **qryn cloud** in a snap. All you need is a supported OS or `Docker` installed.

Stack configuration is 100% based on [ENV](/cloud/env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** Docker **

![image](https://user-images.githubusercontent.com/1423657/184507884-624b9598-62e1-413f-854e-8210ecac4e75.png ':size=300x100')

Install `qryn` on your system using `docker` and `docker-compose`

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Log into the Container Hub

```bash
docker login ghcr.io
```

Use the provided _remote token_ to access to the container repository instead of your password

```bash
echo {providedPullToken} | docker login ghcr.io -u yourUserName --password-stdin
```

##### Pull the containers
```bash
docker pull qxip/qryn-writer-cloud
docker pull qxip/qryn-go-cloud
```

##### Create Docker Compose File

Define a docker compose file to configure each element.

?> Adjust the clickhouse server and authentication details to match your setup

```yml
version: '3.1'

services:
  qryn-writer:
    image: qxip/qryn-writer-cloud:latest
    container_name: qryn-writer
    restart: unless-stopped
    expose:
      - 3100
    ports:
      - 3100:3100
    environment:
      - CLICKHOUSE_SERVER=clickhouse-server
      - QRYN_DATABASE_DATA_0_NAME=qryn
      - QRYN_DATABASE_DATA_0_HOST=clickhouse.server.com
      - QRYN_DATABASE_DATA_0_PORT=9000
      - QRYN_DATABASE_DATA_0_USER=default
      - QRYN_DATABASE_DATA_0_PASS=qryn1234
      - QRYN_DATABASE_DATA_0_DEBUG=true
      - QRYN_DATABASE_DATA_0_SECURE=false
      - QRYN_DATABASE_DATA_0_ASYNC_INSERT=false
      - QRYN_HTTP_SETTINGS_PORT=3100
      - QRYN_SYSTEM_SETTINGS_DB_TIMER=0.5
      - QRYN_SYSTEM_SETTINGS_DB_BULK=1000
      - QRYN_LOG_SETTINGS_STDOUT=true
      - QRYNCLOUD_LICENSE=XXXXXXXXX-XXXXXXXXXXXX-XXXXXXXX
    command:
      - sh
      - -c
      - ./cloki-writer -initialize_db && ./cloki-writer

  qryn-go:
    image: qxip/qryn-go-cloud:latest
    container_name: qryn-go
    restart: unless-stopped
    expose:
      - 3200
    ports:
      - 3200:3200
    environment:
      - CLICKHOUSE_SERVER=clickhouse-server
      - QRYN_DATABASE_DATA_0_NAME=qryn
      - QRYN_DATABASE_DATA_0_HOST=clickhouse.server.com
      - QRYN_DATABASE_DATA_0_PORT=9000
      - QRYN_DATABASE_DATA_0_USER=default
      - QRYN_DATABASE_DATA_0_PASS=qryn1234
      - QRYN_DATABASE_DATA_0_SECURE=false
      - QRYN_DATABASE_DATA_0_HTTPS=false
      - QRYN_DATABASE_DATA_0_DEBUG=true
      - QRYN_HTTP_SETTINGS_PORT=3200
      - QRYN_SYSTEM_SETTINGS_DB_TIMER=0.5
      - QRYN_SYSTEM_SETTINGS_DB_BULK=1000
      - QRYN_LOG_SETTINGS_STDOUT=true
      - QRYNCLOUD_LICENSE=XXXXXXXXX-XXXXXXXXXXXX-XXXXXXXX
```

##### Run Compose
```bash
docker compose up -d
docker compose ps
```

?> That's it! You are ready to access the stack using [qryn-view or Grafana](getting-started.md)

?> For an overview of all enviroment settings go [here](/cloud/env.md) and for configurations go [here](/cloud/config.md)

#### ** Kubernetes **
<a id=k8s name=k8s></a>

![image](https://user-images.githubusercontent.com/1423657/184507942-cb195a16-b6d1-451a-9d80-00550f261048.png ':size=100')

Install qryn on your system using `kubernetes`


#### Helm
Use `Kubernetes` and `helm` to get started using either a local or cloud ClickHouse instance.

?> You need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster. It is recommended to run this tutorial on a cluster with at least two nodes that are not acting as control plane hosts.

##### Pull the qryn service containers
```bash
docker pull qxip/qryn-writer-cloud
docker pull qxip/qryn-go-cloud
```

##### Rapid Example
```bash
kubectl apply -f qryn-deployment.yaml
```

###### `qryn-deployment.yml`
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: qryn-writer
  namespace: monitoring
  labels:
    io.metrico.service: qryn-writer
spec:
  replicas: 1
  selector:
    matchLabels:
      io.metrico.service: qryn-writer
  template:
    metadata:
      labels:
        io.metrico.service: qryn-writer
    spec:
      containers:
        - image: qxip/qryn-writer-cloud
          name: qryn-writer
          ports:
            - containerPort: 3100
          command:
            - sh
            - -c
            - ./cloki-writer -initialize_db && ./cloki-writer
          env:
            - name: QRYN_DATABASE_DATA_0_NAME
              value: qryn
            - name: QRYN_DATABASE_DATA_0_HOST
              value: CHANGEME_clickhouse_server_hostname
            - name: QRYN_DATABASE_DATA_0_PORT
              value: "9440"
            - name: QRYN_DATABASE_DATA_0_HTTPS
              value: "true"
            - name: QRYN_DATABASE_DATA_0_USER
              value: CHANGEME_clickhouse_username
            - name: QRYN_DATABASE_DATA_0_PASS
              value: CHANGEME_clickhouse_password
            - name: QRYN_DATABASE_DATA_0_DEBUG
              value: "true"
            - name: QRYN_DATABASE_DATA_0_SECURE
              value: "true"
            - name: QRYN_DATABASE_DATA_0_ASYNC_INSERT
              value: "false"
            - name: QRYN_HTTP_SETTINGS_PORT
              value: "3100"
            - name: QRYN_SYSTEM_SETTINGS_DB_TIMER
              value: "0.5"
            - name: QRYN_SYSTEM_SETTINGS_DB_BULK
              value: "1000"
            - name: QRYN_LOG_SETTINGS_STDOUT
              value: "true"
            - name: QRYNCLOUD_LICENSE
              value: CHANGEME_qryn_license
      restartPolicy: Always
      imagePullSecrets:
        - name: qryn-ghcr

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: qryn-go
  namespace: monitoring
  labels:
    io.metrico.service: qryn-go
spec:
  replicas: 1
  selector:
    matchLabels:
      io.metrico.service: qryn-go
  template:
    metadata:
      labels:
        io.metrico.service: qryn-go
    spec:
      containers:
        - image: qxip/qryn-go-cloud
          name: qryn-go
          ports:
            - containerPort: 3200
          env:
            - name: QRYN_DATABASE_DATA_0_NAME
              value: qryn
            - name: QRYN_DATABASE_DATA_0_HOST
              value: CHANGEME_clickhouse_server_hostname
            - name: QRYN_DATABASE_DATA_0_PORT
              value: "9440"
            - name: QRYN_DATABASE_DATA_0_HTTPS
              value: "true"
            - name: QRYN_DATABASE_DATA_0_USER
              value: CHANGEME_clickhouse_username
            - name: QRYN_DATABASE_DATA_0_PASS
              value: CHANGEME_clickhouse_password
            - name: QRYN_DATABASE_DATA_0_DEBUG
              value: "true"
            - name: QRYN_DATABASE_DATA_0_SECURE
              value: "true"
            - name: QRYN_DATABASE_DATA_0_ASYNC_INSERT
              value: "false"
            - name: QRYN_HTTP_SETTINGS_PORT
              value: "3200"
            - name: QRYN_SYSTEM_SETTINGS_DB_TIMER
              value: "0.5"
            - name: QRYN_SYSTEM_SETTINGS_DB_BULK
              value: "1000"
            - name: QRYN_LOG_SETTINGS_STDOUT
              value: "true"
            - name: QRYNCLOUD_LICENSE
              value: CHANGEME_qryn_license
      restartPolicy: Always
      imagePullSecrets:
        - name: qryn-ghcr

---

apiVersion: v1
kind: Service
metadata:
  name: qryn-writer
  namespace: monitoring
  labels:
    io.metrico.service: qryn-writer
spec:
  ports:
    - name: http
      port: 3100
      targetPort: 3100
  selector:
    io.metrico.service: qryn-writer

---

apiVersion: v1
kind: Service
metadata:
  name: qryn-go
  namespace: monitoring
  labels:
    io.metrico.service: qryn-go
spec:
  ports:
    - name: http
      port: 3200
      targetPort: 3200
  selector:
    io.metrico.service: qryn-go

```

#### ** APT **
Install `qryn` on your system using `deb` on a Debian system _(or derivate)_

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Download
```bash
# Use provided download link from QXIP
```
##### Install
```bash
dpkg -i qryn-writer-cloud_vx.x.x_amd64.deb
dpkg -i qryn-go-cloud_vx.x.x_amd64.deb
```
##### Configure
```bash
```
##### Run
```bash
# Execute init DB on first time run
/usr/bin/qryn-writer/ -initialize_db

systemctl enable qryn-writer
systemctl start qryn-writer

systemctl enable qryn-go
systemctl start qryn-go
```

#### ** RPM **

Install `qryn` on your system using `rpm` on a RHEL based OS _(or derivate)_

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Download
```bash
# Use provided download link from QXIP
```
##### Install
```bash
dpkg -i qryn-writer-cloud_vx.x.x_x86_64.rpm
dpkg -i qryn-go-cloud_vx.x.x_x86_64.rpm
```
##### Configure
Configure per [here](/cloud/config.md)
```bash
vim /etc/qryn-writer/qryn-writer.json
vim /etc/qryn-go/qryn-go.json
```
##### Run
```bash
# Execute init DB on first time run
/usr/bin/qryn-writer/ -initialize_db

systemctl enable qryn-writer
systemctl start qryn-writer

systemctl enable qryn-go
systemctl start qryn-go
```

<!-- tabs:end -->

## Data Rotation
<a id="data-rotation" name="data-rotation"></a>

*Requires qryn-writer v1.9.65 or higher*

### Time based rotation

The default rotation mechanism is configured in amount via amount of days for each node of the database configuration.

##### JSON
```
{
  "database_data": [{
     "ttl_days": 10
  }]
}
```

##### ENV
```
QRYN_DATABASE_DATA_0_TTL_DAYS = 10
```

?> The default days value is "7"

### Space based rotation

To force data rotateion before the HD is full, admins can use the emergency sweeper settings, in charge of deleting the oldest partitions in the database if the overall size of all the tables is more than the configured value. 

##### JSON
```
{ 
  "database_data": [{
    "emergency_sweep_limit": "200GB"
    ...
  }...],
  ...
}
```
##### ENV
```
QRYN_DATABASE_DATA_0_EMERGENCY_SWEEP_LIMIT = 200GB
```

The allowed units are: `B`, `KB`, `MB`, `GB`, `TB`, `PB`, `EB` .

?> the limit should be set larger than a single day's usage to avoid rotating live data

?> The same settings should be configured for **each node** in case of clusters. 

After the configuration is set and the writer is restarted, the emergency sweeper starts watching the overall size of the database and trying to keep it lower than 200GB. 

## Advanced payload codecs configuration

By default the system stores traces and logs payload with no codecs specified (lz4 compression). But the needed compression codec can be specified in the configuration.

The codec will be applied to the logs payload column (samples_v4.string) and the traces payload column (tempo_traces.payload).

In order to specify the codec (for example ZSTD(9)) please specify the configuration option:
```
{
  "database_data": [
    {
      ....
      "text_codec": "ZSTD(9)"
    }
    ...
  ]
  ...
}
```
The corresponding env variable would be: `QRYN_DATABASE_DATA_0_TEXT_CODEC=ZSTD(9)`

Every general purpose codec from https://clickhouse.com/docs/en/sql-reference/statements/create/table/#general-purpose-codecs can be specified.

Codecs should be specified for every database node.

After updating codecs configuration `qryn-writer -initialize_db` should be run to update the schema.

## Advanced log lines index configuration

In case if there are a lot of `|=` or `!=` line filter requests, a user can configure tokenbf or ngrambf_v1 skipping indexes over log lines.

Indexes are applied for logs payload (samples_v4.string).

In order to set the index, please specify the configuration option:
```
{
  "database_data": [
    {
      ....
      "logs_index": "tokenbf",
      "logs_index_granularity": 1
    }
    ...
  ]
  ...
}
```
The corresponding env variables:
- QRYN_DATABASE_DATA_0_LOGS_INDEX=tokenbf
- QRYN_DATABASE_DATA_0_LOGS_INDEX_GRANULARITY=1

Every index can be specified.

Cheatsheet: https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/mergetree/#functions-support

Index should be specified for each database node.

After updating index configuration `qryn-writer -initialize_db` should be run to update the schema.

## Header-based sharding

*Since: qryn-writer v1.9.71*

In case of using a clickhouse distributed setup the default sharding is based on: 
- fingerprint of logs
- hash of traceid + spanid of traces

Playload is distributed across of clickhouse nodes in a random way.

If this is not acceptable, there is a header based distribution provided. Each write request should have `X-Shard` header with the ID of clickhouse node where the payload should be saved.

### Configuration
JSON based configuration of the feature:
``` { ...
  "cluster_settings": {
    "distribution_header": true
  },
...}
```
Env based configuration: `QRYN_CLUSTER_SETTINGS_DISTRIBUTION_HEADER=true`

### Usage

After configuration the system accepts header: X-Shard: <Int64 ID OF A SHARD IN DISTRIBUTED SETUP>
If the ID is not set or the ID is negative, the default type of sharding is provided.
  
## CORS support
*since qryn-go v1.2.51*

Qryn supports cors headers. They can be configured with the config as below:

```
{
  ...
  "http_settings": {
    "cors": {"enable": true, "origin": "*"}
  },
  ...
}
```

The corresponding env variables: 
- `QRYN_HTTP_SETTINGS_CORS_ENABLE=true`
- `QRYN_HTTP_SETTINGS_CORS_ORIGIN=*`
  
## Debug information

*since qryn-go v1.2.62*
In order to see the in-server timings of the data read there's a configuration option to add internal debug info inside the response.
Implemented for prometheus requests.

###Usage
Configuration to set up:
```
{
  ...
  "system_settings": { ...
    "query_stats": true
  }
}
```
The corresponding environment option is `QRYN_SYSTEM_SETTINGS_QUERY_STATS=true`.
Every prometheus query_range response will carry a "stats" attribute with internal query timings.
  
## OrgID usage statistics

*since qryn-writer v1.9.81*

Qryn writer has the feature to write usage statistics connected to each orgID:
- uncompressed data volume 
- compressed data volume
- amount of time_series used today
  
The statistics are written into the `0` orgID.
The statistics are updated each minute.

### Usage
Configuration to set up:
```
{
  ...
  "writer": { ...
    "write_stats": true
  }
}
```
The corresponding environment option is `QRYN_WRITER_WRITE_STATS=true`.

After the configuration is on, the statistics start being written into the configured database.
Scaling of the writer with this option on is not suggested as it can lead to the clickhouse CPU overhead.

  ## Syncronous and asyncronous inserts
  
  *since qryn-writer v1.9.84.beta-1*
  
  In order to minimize the ingestion time qryn-writer includes the feature of asyncronous ingestion into the database.
  
  Writer in the asyncronous insert mode returns 2XX code immediately after the data was sent into the db. 
  It doesn't wait for the database to approve an insert request. 
  The insert request lasts for 5 seconds waiting for bigger amount of data to be sent to the DB. 
  Then the insert request is finished. If an error appeared at the finishing stage of the request, all the data is lost.
  
  So async inserts are much faster, but the delivery in this case is not guaranteed.
  
  ### Usage
  
  Default insert mode is configured on per-node basis in the `database_data` array.
  The insert mode can be configured on per-request basis. `X-Async-Insert` header with `0/1` values is accepted to forcefully apply or disallow the async inserts mode. If the header is omitted, the default mode is applied.
  <!-- tabs:start -->
  #### ** JSON configuration **
```
{
  "database_data": [
    {
      ....
      "async_insert": true
    }
    ...
  ]
  ...
}
```
  
  #### ** ENV configuration **
  
  `QRYN_DATABASE_DATA_0_ASYNC_INSERT=true`
  
  `QRYN_DATABASE_DATA_1_ASYNC_INSERT=true`
  
  ...
  
  `QRYN_DATABASE_DATA_X_ASYNC_INSERT=true`
  
  #### ** Header based configuration **
  
  `curl .... -H "X-Async-Insert: 0" ....` to force syncronous inserts
  
  `curl .... -H "X-Async-Insert: 1" ....` to force asyncronous inserts  
 <!-- tabs:end -->
  
